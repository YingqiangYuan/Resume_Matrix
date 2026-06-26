# 05 — 执行计划

## 一、排序原则：风险优先 + Demo Deadline 倒推

本项目的排序逻辑是 **"先打通端到端最高风险的一条链路，再横向扩"**，理由如下：

1. **最大未验证假设**：Snowflake + Strand Agent + AgentCore Runtime + Bedrock KB 组合在 Cedar Ridge 内部从未跑过端到端。这条链路通不通是项目第一不确定性，所以 Phase 1 必须先用 1 条业务问题打通它，不分散到 5 条上
2. **CMIO Demo Deadline 倒推**：2026-09 第二周（具体落在 Thu 2026-09-10）给 CMIO 看 demo 是硬约束（Doc 01 §6）。从这个日期往前倒推：
   - **1 周** dry-run + demo + handoff (Phase 4，含 Labor Day 09-07 假日)
   - **3 周** evaluation + guardrail + Rachel/Sophia UAT (Phase 3)
   - **4 周** P2 房间余量 + P4 高危预警 + KB + Visualization Tool (Phase 2，其中 P4 高危预警最有 demo 张力)
   - **3 周** 端到端 vertical slice 打通 (Phase 1)
   - **2 周** onboarding + dev 环境 (Phase 0)
3. **学生学习曲线**：John Doe 没用过 Strand / AgentCore / Snowflake / Bedrock 任意一个，Phase 0 必须给 2 周完整 onboarding 时间，否则 Phase 1 会卡在环境问题上
4. **Stakeholder 期望管理**：Hannah 的 20 条 SQL 一次性全做完不现实。本项目按 Doc 03 优先级（P0/P1/P2）排，明确告诉 Hannah 哪些会进 MVP、哪些只在 semantic layer 预埋

---

## 二、整体时间线

| Phase | 时间段 | 工作日 | 扣除的联邦假日 | 核心目标 | 涉及模块 |
|-------|--------|--------|----------------|----------|----------|
| Phase 0 | 2026-06-15 → 2026-06-28 | 9 天 | Juneteenth Fri 06-19 | Onboarding + dev 环境 | 全栈本地搭建 |
| Phase 1 | 2026-06-29 → 2026-07-19 | 14 天 | Independence Day observed Fri 07-03 | P1 Shift Handover 端到端 vertical slice | Strand + MCP + Snowflake Query Tool + Semantic Layer (subset) + CDK staging |
| Phase 2 | 2026-07-20 → 2026-08-16 | 20 天 | — | P2 Room Availability + P4 High-Risk Alert + KB | 扩 semantic layer、加 Knowledge Retrieval Tool、加 Visualization Tool、KB corpus |
| Phase 3 | 2026-08-17 → 2026-09-05 | 15 天 | — | Evaluation + Guardrail + Pilot UAT | Evaluation harness、CloudWatch dashboard、Rachel + Sophia UAT |
| Phase 4 | 2026-09-08 → 2026-09-12 | 4 天 | Labor Day Mon 09-07（Phase 4 起点改为 Tue 09-08） | CMIO demo + handoff | Dry-run、demo、文档归档、代码移交 |

**总计 62 工作日（扣除 3 个联邦假日）= 约 13 个日历周 = ~3 个月**。Buffer 安排：Phase 2 与 Phase 3 各内含 2 天 unplanned buffer，Phase 4 因 Labor Day 仅 4 个工作日（Tue–Fri），handoff 工作量被前移到 Phase 3 末尾以保证 Day 4 能在 Fri 09-11 收口。

---

## 三、各 Phase 详述

### Phase 0 — Onboarding & Setup（2026-06-15 → 2026-06-28，9 工作日，扣除 Juneteenth 06-19）

**入口条件**：John Doe 入职手续完成，HR、IT、Engineering 三方账户已开通。

**做什么**：
1. Day 1-2：onboarding 流程（HR、IT、Cedar Ridge 内训）
2. Day 3-4：读 Doc 01-05 + Hannah 的 20 条 SQL 模板 + Diego 的 Snowflake 数据字典 + Cedar Ridge 内部 OB 业务术语表
3. Day 5：与 Kevin、Diego、Hannah 各一对一 kickoff
4. Day 6-7：搭本地 dev 环境——Python 3.12、`strands-agents` SDK、`mcp` SDK、Snowflake CLI、AWS CDK CLI、Next.js 项目骨架
5. Day 8-9：跑通官方"Hello Strand Agent"示例 + 官方"AgentCore Runtime quickstart"示例，本地能调通一个 dummy tool
6. Day 10：Phase 0 exit demo（向 Kevin 演示本地能跑 Strand Agent + 一个 dummy MCP tool）

**交付物**：
- 本地 dev 环境 README（如何 setup，含 LLM provider 切换说明）
- Phase 0 exit demo 录屏

**负责人**：John Doe 主写；Kevin & Diego 提供 onboarding 资料；Hannah 提供 SQL 模板与术语表。

**排序理由**：先把"陌生技术栈"的不确定性消除，避免 Phase 1 被环境问题阻塞。

---

### Phase 1 — Vertical Slice on P1 Shift Handover（2026-06-29 → 2026-07-19，14 工作日，扣除 Independence Day observed Fri 07-03）

**入口条件**：Phase 0 exit demo 通过；Snowflake `CEDAR_RIDGE_OB` service account `MATERNAPULSE_AGENT_RO` 已开通；AWS CDK staging 账户已开通。

**做什么**：
1. Week 1（5 天）：在本地实现 Snowflake Query Tool（包含 semantic layer subset，只覆盖 REQ-01 所需的 `active_census` 1 个 metric）。能在本地跑 `python tool.py "How many patients are here right now?"` 拿到正确答案
2. Week 2（4 天，扣除 07-03）：把 Strand Agent + Snowflake Query Tool 串起来，让 agent 能调用工具回答 REQ-01。同时实现最简 Demo Chat UI（单页 chat，硬编码 endpoint）。**本周末完成 LLM provider 抽象层（REQ-23）的代码骨架**：环境变量 `LLM_PROVIDER` 支持 `openai` / `gemini` / `bedrock_claude` 三选一；本 Phase 仅验证 `openai` 走通（Cedar Ridge 已为 demo 开了 OpenAI API key），`gemini` 留到 Phase 2、`bedrock_claude` 留到 Phase 3 在 Cedar Ridge pilot 账户接入
3. Week 3（5 天）：用 CDK 把 Strand Agent + MCP 部署到 staging 的 AgentCore Runtime，端到端从 Vercel demo UI → AgentCore App → Strand → MCP → Snowflake 跑通

**交付物**：
- 一条端到端 demo 链路（在 Cedar Ridge staging 上）
- Snowflake Query Tool 代码 + 单元测试
- AgentCore Runtime CDK stack
- 第一版 semantic layer YAML（1 个 metric + 必要的 dimension）
- **LLM provider 抽象层骨架（REQ-23）**，`openai` provider 跑通
- Phase 1 exit demo（向 Kevin + Priya + Hannah 演示，提问"How many patients are here right now?"拿到正确答案）

**负责人**：John Doe 主写所有代码；Kevin daily PR review；Diego 在 Snowflake 性能问题上 office hour 支持；Hannah review semantic layer。

**排序理由**：用 1 条最简单的业务问题（REQ-01）打通最高风险的端到端链路。如果这条通不了，后面 4 条做了也是白做。

---

### Phase 2 — Expand to P2 + P4 + 补齐 P1（2026-07-20 → 2026-08-16，20 工作日）

**入口条件**：Phase 1 exit demo 通过；CDK staging 部署稳定。

**做什么**：
1. Week 1（5 天）：扩 Snowflake Query Tool 支持 REQ-05、REQ-06、REQ-07（房间余量类）；**同时补齐 P1 Shift Handover 剩余 REQ-02 (Hannah Q5 临近分娩窗口函数) + REQ-03 (Hannah Q6 未确认告警)** ——它们与房间余量同样基于 admission/labor_progress/alert 表，schema 与 semantic layer 公用；扩 semantic layer 增加 `beds_by_status`、`available_soon`、`approaching_delivery`、`open_alerts` 等 metric
2. Week 2（5 天）：实现 Knowledge Retrieval Tool + Bedrock Knowledge Base；上传 OB 术语表 + 口径文档 + 20 条 SQL 模板说明作为 corpus；**激活 LLM provider 抽象层的 `gemini` 路径（REQ-23）**，跟 `openai` 跑同一份 eval 看差异；**实现 REQ-04 (Hannah Q14 today day shift roster)** ——它是简单 JOIN，借这一周加进去
3. Week 3（5 天）：实现 Visualization Tool（matplotlib → PNG → S3）；扩 Strand Agent 让它能决定何时调用 Viz Tool；UI 加图片渲染
4. Week 4（5 天，含 2 天 buffer）：实现 REQ-09、REQ-10（高危预警类）——这是 demo 张力最大的两条，BP 趋势识别。和 Hannah 一起把 Q7 的窗口函数语义写进 semantic layer；**顺手实现 REQ-11 (最新 vital 异常列表)** ——与 REQ-09/10 共用 vital_sign 表 join 逻辑，边际成本低；**同时打磨 REQ-24 (北美数字格式)** ——在 system prompt + post-processing 层强制逗号千分位、24h 时间格式，抽 30 条 agent 回答人工审稿；**Week 4 末日预留 1 次 Rachel + Sophia pre-UAT preview（30-60 min）**，用 5 条她们的真实问题跑一遍 staging agent，提前曝光 Phase 3 UAT 可能爆雷的问题（呼应 §六 R5 缓解）

**交付物**：
- 3 个 tool 全部实现（Snowflake Query / KB Retrieval / Visualization）
- Bedrock KB corpus 上线
- Semantic layer 覆盖 P0/P1 全部业务 metric（含 REQ-01 至 REQ-11 对应口径）
- REQ-09 / REQ-10 / REQ-11 全部实现（高危预警类三件套）
- LLM provider 抽象层激活 `openai` + `gemini` 两路（REQ-23 进度 2/3）
- 北美数字格式打磨完成（REQ-24）
- 1 次 Rachel + Sophia pre-UAT preview 记录（含曝光的潜在问题清单）
- Phase 2 exit demo（演示 5 条业务问题：交班 + 临近分娩 + 未确认告警 + 房间余量 + BP 趋势）

**负责人**：John Doe 主写；Kevin 每周 review；Hannah 提供 KB corpus 内容与口径 review。

**排序理由**：在端到端链路通了之后，按"demo 张力 + 学习曲线"加业务问题——P2 房间余量是 Charge Nurse 最高频的，P4 高危预警是 CMIO 最想看到的"AI 比单点阈值聪明"的故事。

---

### Phase 3 — Evaluation + Hardening + Pilot UAT（2026-08-17 → 2026-09-05，15 工作日）

**入口条件**：Phase 2 exit demo 通过；Rachel & Sophia 在 8 月初已被通知 UAT 时间；Cedar Ridge pilot 账户的 Bedrock Claude 访问权限已开通。

**做什么**：
1. Week 1（5 天）：实现 Doc 03 §四的 evaluation harness——跑 30 条 golden conversation、自动算 SQL accuracy / answer accuracy / hallucination rate / p95 latency 四个指标，输出 markdown report；CloudWatch dashboard 上线；**激活 LLM provider 抽象层的 `bedrock_claude` 路径（REQ-23 收尾，3/3 完成）**，让 eval harness 同时跑三套 provider 对比差异 ≥ 10% 报警 Kevin（呼应 §六 R4）
2. Week 2（5 天）：guardrail 实现——PII 屏蔽（REQ-19）、显式拒答（REQ-21）、citation requirement（REQ-20）；audit log 双写（REQ-22）；**实现 REQ-25 (UI loading + retry 路径)** ——5s 无响应 UI 显式显示 loading 状态与 retry 按钮，staging 上 mock timeout 5 次全通过
3. Week 3（5 天，含 2 天 buffer）：Rachel + Sophia UAT——**集中演练期间密度提升到每周 2 次 60 分钟**（与 Doc 02 §2.6 闭环），每次跑 10 条她们的真实交班场景。根据反馈调整 system prompt 与 hybrid 输出策略

**交付物**：
- Evaluation report（达到 Doc 01 §4 全部技术指标阈值，含三套 LLM provider 对比）
- CloudWatch dashboard（5 个 widget：tool 调用量、错误率、p95 latency、token 用量、成本）
- LLM provider 抽象层全部三路打通（REQ-23 完成）
- UI loading + retry UX 实现（REQ-25 完成）
- Rachel + Sophia UAT 报告（含 30 条 golden conversation 的 thumbs up/down 评分）
- **`docs/runbook.md` 与 transition memo 第一版**（利用 Week 3 buffer 时段起稿，前移自原 Phase 4 工作量，详见 Phase 4 排序理由）
- Phase 3 exit go/no-go（Sophia + Priya + Marlene 联签 pilot ready）

**负责人**：John Doe 主写；Kevin 在 guardrail 设计上深度参与；Hannah 在 SQL accuracy 评判上做最终裁判；Rachel + Sophia 做 UAT。

**排序理由**：MVP 的功能在 Phase 2 末已经齐了，Phase 3 是把"能跑通"升级到"能给 CMIO 看"——评估、护栏、用户验收三件套。

---

### Phase 4 — CMIO Demo & Handoff（2026-09-08 → 2026-09-12，4 工作日，扣除 Labor Day Mon 09-07）

**入口条件**：Phase 3 exit go/no-go 通过；Labor Day 09-07 假期（休息）。

**做什么**：
1. Day 1 (Tue 09-08) – Day 2 (Wed 09-09)：与 Priya 排练 demo 脚本——10 个 demo 问题（覆盖 P1 + P2 + P4 + 1 个 hallucination 拒答场景），2 次 dry-run（一次自演、一次给 Kevin + Hannah 看）；Phase 3 末已经预写好的 `docs/runbook.md` 与 transition memo 在这两天做最终 review
2. Day 3 (Thu 09-10)：**CMIO demo（25 分钟演示 + 10 分钟 Q&A）**，现场 Priya 主持，John Doe 演示，Kevin 与 Hannah 后排支持；demo 当天午后做最终 go/no-go 复盘
3. Day 4 (Fri 09-11)：handoff——代码 freeze 在 main 分支的特定 tag；运行手册与 transition memo 由 Kevin 终审签字；PR review 权限转给 Kevin；John Doe 走 offboarding 流程

**关于 4 工作日的工作量重新分配**：因为 Labor Day 占掉 Phase 4 第一天，原 5 天计划压缩为 4 天。压缩方式不是"塞进去"，而是 **handoff 物料前移到 Phase 3 末尾起稿**——`docs/runbook.md` 第一版与 transition memo 第一版在 Phase 3 Week 3 buffer 时段完成，Phase 4 Day 4 只做 review 与签字，不从零写。

**交付物**：
- CMIO demo 录屏
- `docs/runbook.md`（含部署、回滚、常见故障排查、cost monitoring）
- 离职 transition memo（写给下一位接手的 engineer）

**负责人**：John Doe 演示；Priya 主持；Kevin 接手代码 ownership。

**排序理由**：demo 是项目最高 visibility 时刻，必须把所有"惊喜"在 dry-run 里消灭。handoff 在 demo 后做，避免 John Doe 临走前还在 fight production issue。Labor Day 不可绕过，所以工作量必须前置而不是后置。

---

## 四、关键里程碑与质量关卡

> **关于日期约定**：M0 / M1 / M2 / M3 的标注时间是对应 Phase 的日历结束日，若落在周末，**实际 exit demo / sign-off 都在前一个 Friday 完成**（M4 / M5 直接标 Thu / Fri 是因为 Phase 4 短，需要精确到日）。

| 里程碑 | 时间 | 判定标准 | 不通过的回退路径 |
|--------|------|----------|------------------|
| **M0 Onboarding done** | 2026-06-28 (Sun，实际 exit Fri 06-26) | Phase 0 exit demo 跑通 dummy tool；本地 dev README 写完 | 延期 5 天 → Kevin 直接给 John Doe 1 周带教 |
| **M1 Vertical Slice 通了** | 2026-07-19 (Sun，实际 exit Fri 07-17) | REQ-01 在 staging 上 p95 ≤ 4s，SQL accuracy = 100% (1/1) | 延期 5 天 → Kevin 接手 Snowflake Query Tool 主写权 |
| **M2 全 P0 + 关键 P1 功能实现** | 2026-08-16 (Sun，实际 exit Fri 08-14) | P0 业务需求 (REQ-01、02、03、05、06、09、10、11) + P1 业务需求 (REQ-04、07) + 横切 P0 (REQ-17 到 REQ-22) + LLM provider 抽象层 openai+gemini 两路 (REQ-23 进度 2/3) + 北美数字格式 (REQ-24) 全部在 staging 跑通；Bedrock KB 上线；Rachel + Sophia pre-UAT preview 完成 | 延期 5 天 → 砍掉 REQ-04、REQ-07 两条 P1 |
| **M3 Pilot Ready** | 2026-09-05 (Sat，实际 exit Fri 09-04) | Doc 01 §4 全部技术指标达标；LLM provider 三路全部打通 (REQ-23 完成) + UI loading/retry (REQ-25) 实现；Sophia + Priya + Marlene 联签 | 延期到 2026-09-09 → 砍掉 Rachel 部分 UAT 改为 Priya 抽样 |
| **M4 CMIO Demo 通过** | 2026-09-10 (Thu) | Marcus 当场给"continue to broader rollout"的 verbal go | 失败 → Priya 写 "lessons learned"，pilot 维持 6 个月观察 |
| **M5 Handoff Complete** | 2026-09-11 (Fri) | 运行手册 + transition memo 由 Kevin 签字（Phase 3 末已起稿，Phase 4 Day 4 终审）；代码 freeze tag pushed | 不通过 John Doe 不能离职（按合同条款） |

---

## 五、John Doe 的工作分配

| Phase | John Doe 工作日 | 70% 核心开发 | 20% 测试 / debug | 10% 文档 / demo 排练 |
|-------|----------------|---------------|------------------|----------------------|
| Phase 0 | 9 (扣除 Juneteenth) | 4 天 setup | 2 天 跑通 quickstart | 3 天 读文档 |
| Phase 1 | 14 (扣除 Independence Day observed) | 9 天 写 agent + tool + CDK | 3 天 调试 staging 部署 | 2 天 写 Phase 1 exit demo + 文档 |
| Phase 2 | 20 | 14 天 扩 tool + KB + viz + 补齐 P1 Handover + 格式打磨 | 4 天 端到端测试 | 2 天 demo 准备 |
| Phase 3 | 15 | 9 天 eval + guardrail + bedrock_claude 接入 + UI retry | 4 天 UAT 反馈循环 | 2 天 dashboard + handoff 物料起稿 |
| Phase 4 | 4 (扣除 Labor Day) | 0 | 1 天 dry-run | 3 天 demo + handoff 签字 |
| **合计** | **62 工作日** | **36 天** | **14 天** | **12 天** |

---

## 六、风险与缓解

| # | 风险 | 概率 | 影响 | 缓解 | 关联里程碑 |
|---|------|------|------|------|------------|
| R1 | AgentCore Runtime + Strand Agents 在 Cedar Ridge 网络里跑不通（VPC、PrivateLink 问题） | 中 | 高 | Phase 0 末就尝试一次 staging 部署 dummy；预留 Kevin 1 天救火窗口 | M0、M1 |
| R2 | Snowflake Query Tool 生成的 SQL 跟 Hannah 的口径不一致 | 高 | 中 | 把所有 metric 公式从 Hannah Q&A 里抠出来放 YAML；每条 metric 上线前 Hannah 显式签字；evaluation harness 在 M2 之前必须先跑通 SQL accuracy 这一项 | M2 |
| R3 | Bedrock KB chunking 策略不当导致 retrieval 命中率低 | 中 | 中 | Phase 2 Week 2 留 1 天调参；fallback 是把 chunk size 砍半 + 加 metadata filter | M2、M3 |
| R4 | LLM provider 切换（demo OpenAI → prod Bedrock Claude）导致 agent 行为不一致 | 中 | 中 | 同一份 system prompt + 同一份 evaluation harness 跑两个 provider，差异 ≥ 10% 必须报警 Kevin | M3 |
| R5 | Rachel UAT 反馈强烈不满（术语、节奏、信息密度） | 中 | 中 | Phase 2 末安排 1 次 Rachel pre-UAT preview；Phase 3 第 1 周就开始 UAT 而不是等 last minute | M3 |
| R6 | 9 月初 CMIO 临时改 demo 时间 / 议程 | 低 | 高 | Priya 在 8 月初就跟 CMIO 办公室锁死 demo time slot；同步建立"如果延 1 周怎么办"的 plan B（继续跑 UAT） | M4 |
| R7 | John Doe 学习曲线超预期，Phase 1 拖延 | 中 | 高 | Phase 0 给完整 2 周日历时间（9 个工作日，扣除 Juneteenth）做 onboarding 而不是只给 1 周；Phase 1 的 Week 1 设硬关卡——如果 Snowflake Query Tool 跑不通本地，Kevin 立即接手 1 天结对 | M1 |
| R8 | Cedar Ridge AWS 账户因 IT policy 拒绝 AgentCore Runtime 部署 | 低 | 高 | Priya 在 Phase 0 就跟 IT 安全团队走完审批；如不通过，fallback 是 Cedar Ridge IT 的 sandbox 账户 | M0 |

---

## 七、与其他文档的关系

| 文档 | 本文档如何引用它 |
|------|------------------|
| Doc 01 §4 | M3 与 M4 的判定标准全部来自 Doc 01 的技术 + 业务 + 合规指标 |
| Doc 02 | 每个 Phase 的"负责人"段落直接引用 Doc 02 的人物分工 |
| Doc 03 | 每个 Phase 的"做什么"段落直接引用 Doc 03 的 REQ-XX 编号 |
| Doc 04 §九 | 本文档的 Phase 顺序与 Doc 04 末尾实现文档规划的 P0 优先级对齐——如果未来继续推进 Doc 06+，按 09 → 10 → 07 → 11 → 12 → 14 的顺序展开，正好对应 Phase 1 → Phase 3 的开发节奏 |
