# 01 — 项目背景与业务需求

## 一句话总结

**MaternaPulse BI Agent** 是 Cedar Ridge Women's Health 为旗下 6 家分院的妇产科 (Obstetrics) 病房一线护理与运营管理团队搭建的**内部自然语言 BI 助手**：Charge Nurse 在交班、找床、排手术、识别高危产妇时，可以直接用自然语言提问 (例如 "Any beds open in labor on the third floor?")，由部署在 AWS Bedrock AgentCore Runtime 上的 Strand Agent 在 Snowflake 数据仓库上生成并执行 SQL，把答案以文字 + 表格 + 必要图表的混合方式回写到 Demo Chat UI，替代过去依赖分析师手写报表的 ad-hoc 流程。Pilot 上线后预期把 OB 一线 ad-hoc 分析请求 backlog 砍掉 30%，让 Charge Nurse 在交班这种关键时刻 6 秒内拿到准确的病房全景。

---

## 1. 公司背景与商业模式

### 1.1 Cedar Ridge Women's Health 是谁

Cedar Ridge Women's Health（虚构）是一家总部位于美国 Oregon 州 Portland 都会区的**区域性妇产科 / 妇科连锁医院**，业务范围覆盖太平洋西北（Pacific Northwest）。

| 维度 | 设定 |
|------|------|
| 总部 | Portland, Oregon, US |
| 分院数 | 6 家（Portland 都会区 4 家 + Salem 1 家 + Vancouver, WA 1 家） |
| 业务范围 | Maternity & OB/GYN — 产前门诊、待产分娩、产后恢复、新生儿监护 |
| 床位规模 | 全网总床位 ~1,600 张，其中妇产科 ward 床位 ~340 张 |
| 年分娩量 | ~28,000 例 / 年（按全网 6 院合计） |
| 员工规模 | ~2,800 人（含临床、护理、行政、IT、分析） |
| 年营收量级 | ~$520M USD（以保险报销 + 自付为主） |
| 监管框架 | HIPAA、HITECH、The Joint Commission (TJC)、CMS、Oregon Health Authority |

Cedar Ridge 不是 Epic / Cerner 这种级别的"大平台医院"，它在临床 EMR 上用的是一家中型 vendor 的解决方案，自己保留了对**患者流转 (patient flow) 与运营调度**的灵活权——也就是说，EMR 负责的是病历、医嘱、计费这种"重"事情，而**病房调度、交班、床位余量、产程跟踪**这些"轻"但高频的运营动作，Cedar Ridge 选择自己建工具，因为它们直接决定护士的工作体验和单位时间能服务多少产妇。

### 1.2 怎么赚钱

Cedar Ridge 的单位经济（unit economics）核心来自三件事：

1. **每例分娩的报销与自付收入**：商业保险 (~55%) + Medicaid (~35%) + 自费 (~10%) 的 payer mix，平均一例顺产 net revenue ~$11K，剖宫产 ~$18K。
2. **床位周转效率**：床位是稀缺资源，住院时长 (LOS) 每多 1 天就吃掉一个可能的下一例分娩。产后 LOS 准确预测能让 admissions team 提前 24 小时预约下一位产妇的床。
3. **避免医疗事故的高额成本**：一次未识别的妊娠期高血压综合征 (Preeclampsia)、一次未及时叫医生的胎心异常，对应的医疗损失赔偿 + 监管罚款 + 声誉损失可能达到百万级别。

这三件事都直接绑在"**护士能不能实时、准确、低摩擦地看清楚病房**"上。这就是 MaternaPulse BI Agent 的商业基底。

### 1.3 数据已经躺在 Snowflake 里

Cedar Ridge 的 Data Platform 团队 12 个月前完成了一次内部基础设施现代化：所有 EMR 抽数、患者流转日志、护理操作记录、shift 排班、设备维护事件已经通过 nightly batch 同步进 Snowflake (`CEDAR_RIDGE_OB` 数据库，分 `RAW` / `STAGING` / `ANALYTICS` 三层 schema)。Senior Clinical Analyst Hannah Liu 在过去半年里基于这套 Snowflake 数据**手写了 ~20 条核心 SQL**，回应妇产科一线的 5 类常见运营问题：交班 (Shift Handover)、房间余量 (Room Availability)、出院预测 (LOS Prediction)、高危预警 (High-Risk Alert)、医嘱排程 (Order Scheduling)。这 20 条 SQL 在内部已经被反复审核过，**口径稳定**，但每次要跑都得排进 Hannah 的 ticket queue。

这是本项目的起点：数据有了、查询逻辑有了，缺的是"让一线护士不通过 Hannah 直接拿到答案"的接入层。

### 1.4 历史背景：Halcyon POC 已经做过一次

一年前 (2025 春)，Cedar Ridge 曾外包一家叫 Halcyon Health Analytics 的 healthcare data/AI 咨询公司在 Portland 主院的一个 ward 做过一次小型 POC，证明了"在 ~500 行沙盘数据上 AI agent 能正确回答这 20 类业务问题"。POC 验收通过后，Cedar Ridge 决定**把这套能力内化（in-house）**——数据从 Halcyon 的 SQLite 沙盘搬到 Cedar Ridge 自己的 Snowflake，覆盖全部 6 家分院的全部 15 个妇产科 ward，并新成立**AI / Analytics Engineering** 小组负责长期维护与扩展。MaternaPulse 就是这次内化生产化的产物。Halcyon 不再深入参与，只在合同里保留了一名 architect 做季度 advisory。

---

## 2. 触发事件

为什么是**现在** (2026 Q2) 启动 MaternaPulse？三件事在 Q1 同时撞上来：

1. **护士短缺持续恶化 (Nursing Shortage)**：根据 Oregon Center for Nursing 2025 年底报告，Oregon 全州注册护士缺口扩大到 ~6,800 人。Cedar Ridge 妇产科 ward 的 RN 平均加班时长从 2024 同期的每周 4.2 小时升到 2026 Q1 的 6.9 小时。Charge Nurse 在交班、找床这种"明明数据就在系统里"的检索动作上耗费的时间被反复 escalate 到 CMIO 办公室。
2. **分析团队 backlog 翻倍**：Hannah 所在的 Clinical Analytics 团队收到的 OB 运营类 ad-hoc 报表请求从 2024 Q4 的 ~80 张/月涨到 2026 Q1 的 ~170 张/月。Director of Analytics Engineering Priya Raman 在 2026 Q1 OKR review 上申请扩编 2 名 analyst，被 CFO 以"先看看 AI 能不能扛"为由打回。
3. **CMIO 战略指令**：CMIO Dr. Marcus Chen 在 2026-03 一次内部 town hall 上明确表态：未来 6 个月内，Cedar Ridge 妇产科运营**自助查询率**（不通过分析师人工写 SQL 就能拿到答案的请求比例）必须从当前 ~12% 提到 40%，到 2027 年底提到 60%。

三件事捏在一起，CMIO 把 OB ward 选为第一个"agent 化"的科室——理由是 OB 的运营状态机相对标准化、SQL 模板已经被 Hannah 沉淀好、护理团队对工具变更的容忍度也比 ICU 这种高风险科室高。**MaternaPulse 的 demo 必须在 2026-09 月底之前给 CMIO 看到可工作的端到端 demo**，这是 Doc 05 时间线的硬约束。

---

## 3. 项目范围 (Scope)

### 3.1 In Scope（本项目要做的）

| 范围项 | 详细 |
|--------|------|
| **首要 stakeholder** | Charge Nurse / Clinical Nurse 一线护理 |
| **数据范围** | Snowflake `CEDAR_RIDGE_OB` 数据库，11 张核心妇产科表（详见 Doc 03 数据需求概览），覆盖 6 家分院的全部 15 个妇产 ward |
| **业务问题覆盖** | P1 Shift Handover、P2 Room Availability 列为 P0；P4 High-Risk Alert 列为 P1；P3 LOS Prediction、P5 Order Scheduling 列为 P2 |
| **Agent 框架** | Strand Agents (Python), 部署在 AWS Bedrock AgentCore Runtime 的 App + MCP 双 endpoint 上 |
| **RAG 知识库** | Amazon Bedrock Knowledge Base，corpus = Cedar Ridge 内部 OB 业务术语表 + 指标口径文档 + Hannah 沉淀的 SQL 模板说明 |
| **Semantic Layer** | YAML 写的 metric / dimension / time_window / glossary，作为 NL→SQL 的口径地图 |
| **基础设施** | AWS CDK (Python) 部署所有 AWS 资源；区域 `us-east-1` |
| **Demo Chat UI** | Next.js + Vercel，**仅供 John Doe 与 stakeholder 在 demo 阶段使用** |
| **评估框架** | Golden set 直接复用 Hannah 的 20 条 SQL 模板的预期结果，覆盖 SQL accuracy / answer accuracy / hallucination rate / p95 latency |

### 3.2 Out of Scope（显式排除）

> 这些是容易被误解为本项目应该做、但实际不做的范围项。每一项都附理由。

| 排除项 | 理由 |
|--------|------|
| **Agent 写 Snowflake** (INSERT / UPDATE / DELETE) | 本项目永远是 read-only BI。任何"写回 EMR / 写回 Snowflake"的诉求转给上游 EMR vendor 处理。Snowflake service account 角色只赋 SELECT 权限。 |
| **跨数据源 federation** (Snowflake + EMR + 第三方) | 数据已经统一在 Snowflake，不引入 federation 层。 |
| **训练或微调 LLM** | LLM 永远走外部 API：demo 走 OpenAI / Gemini，production 走 Claude on Bedrock。 |
| **实时流处理** (Kafka, Flink) | Snowflake 已是 nightly batch + intraday micro-batch 同步，不引入流式架构。 |
| **移动端 App** | Demo Chat UI 只支持 Web。一线护士演示时用病房工作站的浏览器访问。 |
| **多语言 i18n** | UI 与回答均为英文；项目设计文档为中文叙述。 |
| **企业内部生产前端** | Cedar Ridge IT 的 web team 会用企业内部 stack 接手生产前端，与 AgentCore App endpoint 对接的契约由本项目敲定。**本项目不实现企业生产前端**。 |
| **Halcyon POC 沙盘的 SQLite 实现** | POC 阶段已经过关，本项目从 Snowflake 起步，**不重做 SQLite 沙盘**。 |
| **新生儿 (newborn) / 计费 / 跨科室会诊数据** | 这些属于 OB 之外或 future scope，不进 MVP。 |
| **跨 Cedar Ridge 集团的 BI 整合** (急诊、住院、门诊) | 仅限妇产科。其他科室是 future roadmap。 |

### 3.3 项目体量

| 维度 | 量级 |
|------|------|
| 项目时长 | 3 个月（2026-06-15 → 2026-09-12，详见 Doc 05） |
| 投入 | John Doe 1 名 intern 写代码 + 上下游 5 名 stakeholder 兼职配合 |
| 部署目标 | 1 个 staging 环境 + 1 个 pilot 环境（仅 Portland 主院 1 个 ward 接入真实 stakeholder UAT） |
| 数据量 | Snowflake `CEDAR_RIDGE_OB.ANALYTICS` schema 内 ~11 张核心表，单表行数 50K – 2.4M 不等，总体量 < 80GB |

---

## 4. 成功标准

所有指标都是**可量化的**，在 Doc 05 的里程碑里会绑定 go/no-go 关卡。

### 4.1 技术指标

| 指标 | 目标值 | 测量方式 |
|------|--------|----------|
| **SQL accuracy** (agent 生成的 SQL 在 golden set 上的语义正确率) | ≥ 90% on 20 条 Hannah golden queries | Doc 14 设计的 eval harness 自动跑 |
| **Answer accuracy** (最终回答与 ground truth 的语义一致率) | ≥ 85% on 20 条 golden conversations | LLM-as-judge + 人工抽审 |
| **Hallucination rate** (回答中出现数据不存在的字段 / 编造数字) | < 5% | LLM-as-judge + 关键字 grep |
| **p95 end-to-end latency** (UI 发出问题 → 收到完整回答) | ≤ 6 秒 | CloudWatch RUM + AgentCore trace |
| **Cold start p95** (AgentCore Runtime 实例冷启动) | ≤ 12 秒 | AgentCore observability |
| **Citation rate** (回答里引用 KB snippet 的比例) | ≥ 70% for 涉及术语 / 口径的回答 | 自动正则匹配 citation marker |

### 4.2 业务指标

| 指标 | 目标值 | 测量方式 |
|------|--------|----------|
| **Portland Main pilot ward 自身 OB ad-hoc 分析请求 backlog 下降** | ≥ 30% 在 pilot ward 上线 8 周后（**仅看该 ward 自身的 ticket，不是 Hannah 团队全局 backlog**） | Clinical Analytics 团队 ticket queue 按 ward 标签过滤统计；详见 Doc 03 §2.2 |
| **Charge Nurse UAT 满意度** | ≥ 7 / 10 thumbs-up 比例 on 30 条 golden conversation 演练 | Charge Nurse Rachel Park + Sophia Kim 评分 |
| **Pilot ward 自助查询率** (绕过分析师直接拿到答案的查询数 / 该 ward 同期总查询数) | ≥ 40% 在 pilot 第 8 周 | Agent CloudWatch 日志 + 分析师 ticket queue 交叉对账 |
| **每月 LLM + AWS 运行成本（pilot ward）** | ≤ $500 USD / 月 | Cost Explorer + LLM provider billing dashboard |

### 4.3 合规与安全标准

| 标准 | 目标 |
|------|------|
| **PHI 隔离** | LLM context 中**不出现**任何 PHI（patient_name、phone、emergency_contact 等）。所有 PHI 列在 Semantic Layer 标记 `sensitive: true`，由 Snowflake Query Tool 默认从 SELECT 中剔除。 |
| **审计 trail** | 每一条 user query、生成的 SQL、返回的行数（不含具体值）写入 CloudWatch + Snowflake `MATERNAPULSE_AUDIT.QUERY_LOG`，保留 ≥ 90 天，满足 TJC 内审要求。 |
| **角色权限** | Snowflake service account 仅 `SELECT` 权限；agent 用户身份通过 AgentCore identity 透传到 row access policy。 |

---

## 5. 团队概览

详细团队结构与协作模式见 Doc 02。这里仅列出核心 stakeholder 与 John Doe 的角色定位。

| 姓名 | Title | 与 John Doe 的关系 |
|------|-------|--------------------|
| **John Doe** | Full-stack AI/Data Intern | 本项目主要执行人，写代码与文档 |
| **Kevin Zhang** | Senior AI Engineer | John Doe 的直接 mentor + tech reviewer |
| **Priya Raman** | Director, AI / Analytics Engineering | John Doe 的间接 manager，预算与产品方向 |
| **Diego Martinez** | Senior Data Engineer | Snowflake 数据仓库 owner，semantic layer co-author |
| **Hannah Liu** | Senior Clinical Analyst | 上游 — 提供 20 条核心 SQL 模板与口径定义，做 SQL accuracy review |
| **Rachel Park** | Charge Nurse, Portland Main OB Ward | 首要下游 stakeholder，UAT 主要 evaluator |
| **Sophia Kim** | OB Floor Nurse Manager, Portland Main | 次要下游 stakeholder，签字验收 Pilot 上线 |
| **Dr. Marcus Chen** | CMIO | Executive sponsor，2026-09 demo 拍板人 |
| **Marlene Wong** | CNO | Pilot 上线签字方（Sophia 的上级），与 Priya + Sophia 联签 pilot ready |

John Doe 的定位是**Junior intern**：他执行 Kevin 制定的技术方案，按 Hannah 给出的口径写 semantic layer，按 Rachel & Sophia 的反馈打磨 agent 行为。他**不**自己拍板架构（架构骨架已经由 Priya & Kevin 在 2026-05 锁定），**不**自己定义 metric 口径（这些来自 Hannah），**不**直接跟 CMIO 汇报（中间隔着 Priya & Kevin）。

---

## 6. 项目时间线概览

详细 Phase 划分与里程碑见 Doc 05。这里给出时间线的骨架：

| 时间 | Phase | 核心目标 |
|------|-------|----------|
| 2026-06-15 → 2026-06-28 | Phase 0 — Onboarding & Setup | John Doe 入职 onboard，读 Hannah 的 20 条 SQL 与 Diego 的 Snowflake 数据字典；搭起本地 dev 环境（demo LLM provider 走 OpenAI API） |
| 2026-06-29 → 2026-07-19 | Phase 1 — Vertical Slice on P1 (Shift Handover) | 跑通 1 条端到端链路：Charge Nurse 问 "Show me where everyone is right now" → agent 返回正确人头数 + 状态分布 |
| 2026-07-20 → 2026-08-16 | Phase 2 — Expand to P2 + P4 | 加 Room Availability + High-Risk Alert，扩 semantic layer，加 visualization tool |
| 2026-08-17 → 2026-09-05 | Phase 3 — Evaluation + Hardening | Doc 14 golden set 跑通，加 guardrail、observability、cost optimization；Rachel & Sophia UAT |
| 2026-09-08 → 2026-09-12 | Phase 4 — Final Demo & Handoff | CMIO demo Thu 09-10 + handoff Fri 09-11；起点 Tue 09-08 避开 Labor Day Mon 09-07 |

P3 (LOS Prediction) 与 P5 (Order Scheduling) 在 MVP 范围内列为 P2 优先级，**只在 semantic layer / KB 里预埋口径，不在 agent 里完整实现**——留给 intern 结束后的下一位工程师扩展。这是出于 3 个月 intern + 单人开发的合理体量约束。

---

## 7. 与后续文档的关系

| 文档 | 本文档为它提供的输入 |
|------|----------------------|
| Doc 02 Team & Collaboration | 团队人名、汇报线、stakeholder 关系骨架（本文档 §5） |
| Doc 03 Business Requirements | 5 个业务问题 P1–P5、合规边界、显式排除项 |
| Doc 04 Architecture | 技术栈骨架（Snowflake + Strand + AgentCore + KB + CDK）、数据范围、部署区域 |
| Doc 05 Execution Plan | 项目时长、Phase 骨架、CMIO demo 硬 deadline |

本文档是后续 4 篇文档的 **source of truth**——公司背景、触发事件、Scope 与成功标准在写后续文档时遇到不一致**以本文档为准**。
