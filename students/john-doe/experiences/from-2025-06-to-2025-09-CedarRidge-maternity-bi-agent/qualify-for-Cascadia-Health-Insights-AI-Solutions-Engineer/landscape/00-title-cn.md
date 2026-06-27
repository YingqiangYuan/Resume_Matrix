# Cascadia Health Insights × AI Solutions Engineer，New Grad 方向调研报告

## 报告元信息

- 公司：Cascadia Health Insights（CHI，PNW 区域型 B2B SaaS 医疗数据 + AI 平台公司，私有，~$280M 营收，~1,500 人，Seattle HQ）
- 岗位：AI Solutions Engineer (New Grad)，Customer-Embedded Engineering 子团队，挂在 Customer Success 大部门下
- 岗位申请截止：2026-02-15 起 rolling
- 生成日期：2026-02-20
- 学生定位：University of Washington M.S. Computer Science 在读，2026 年 12 月毕业，美国公民，Q1 = A（已锁定 Cascadia 为定向目标，两周后面试），Q2 = US citizen + Seattle 本地（市场维度跳过 visa 与跨州迁移）
- 报告原则：facts only，no recommendation。本报告只描述事实，不输出"是否值得加入"的判断。决策权完全在学生自己。
- 数据真实性声明：行业、市场、对标公司、薪资基准等公开数据使用真实 citation。Cascadia 公司本身是教学用虚构案例，公司内部事实标注 (illustrative) 或 (Cascadia 内部材料)。学生在真实场景操作时，Cascadia 这一栏对应的应是真实公司的招股书 / 财报 / 官网。

---

## 四个维度的

### Industry: US 医疗数据分析行业是 EHR 巨头 + 分析中型玩家 + 区域专精的三层结构

US Healthcare Data Analytics / Digital Health Software Vendors 是 B2B SaaS 性质的成熟成长行业，2024 年市场规模约 USD 95-105B、CAGR 8-10% [01-industry §3]。结构上分三层：EHR 巨头 Epic 和 Oracle Health（前 Cerner）占据底层交易系统，毛利率 60-75%；分析层中型玩家 Innovaccer、Health Catalyst、Arcadia 在 EHR 之上提供 BI 平台，毛利率 45-60%；区域型供应商（如 Cascadia）凭借地理深度、客户关系和行业 know-how 守住本地市场。2009 年 HITECH Act 推动 EHR 普及催化了行业第一波增长，2020 年 ONC 互操作性规则（FHIR 强制采用）打开了第三方应用的数据访问窗口 [01-industry §5]。2024-2026 年的新一波是 agentic AI / 自然语言 BI Agent，Cascadia Insight Assistant 正是这一赛道的产品。监管框架（HIPAA、HITECH、TJC、CMS value-based purchasing）既是壁垒也是门槛。详见 [01-industry-cn.md](01-industry-cn.md)。

### Company: Cascadia 是 PNW 区域版 Innovaccer + Health Catalyst 的混合体

按 FY25 数据，Cascadia 营收约 $280M、客户约 80 家、平均 ACV $3.5M、员工 1,500 人，2019 年起约 30% 股权被 PE 持有，私有公司未来 3-5 年存在 IPO 或二次私有交易窗口（Cascadia 2025 Annual Report, illustrative）。和 Innovaccer（全美 1,600+ 客户、$200M+ ARR、$3.2B 估值）、Health Catalyst（全美 1,000+ 客户、$310M 营收、上市公司）相比，Cascadia 客户数小一个数量级，但单客户合同价值更高，走的是"少而深"的路径 [02-company §1]。2025 年发布的 Insight Assistant 已部署 12 家，FY26 目标 30 家，是当前公司增长叙事的核心。反向爆破信号：PNW 全区医院床位约 7 万张，即便 100% 渗透天花板约 $400-500M，跨区域扩张会和 Innovaccer / Health Catalyst 在 Texas / Midwest 的存量市场正面碰撞 [02-company §1]。详见 [02-company-cn.md](02-company-cn.md)。

### Role: AI Solutions Engineer 是 Forward Deployed Engineer 范式在医疗 AI 的本地化

本岗位本质是 "Solutions Engineer + AI 知识 + 客户驻场" 的混合体，2020-2023 年随 Palantir Forward Deployed Engineer 模式扩散而成型，2024-2025 年 Generative AI 应用爆发后演化成独立赛道 [03-role §1]。日常预计 40-50% 写代码（Strand Agents、AWS CDK、Python、SQL），20-25% 在客户站点驻场对接 Charge Nurse / Floor Manager / 临床运营总监，10-15% 写英文文档面向合规官，剩下时间在内部 review 和评估迭代 [03-role §3]。Cascadia 团队挂在 Customer Success 而不是 Product Engineering 下，这种组织放置类似 Palantir 把 FDE 放在 Delivery 序列，对升 Staff 以上职级速度的影响需要列入入职预期管理 [03-role §1]。Entry $95-120K base → Mid $145-175K → Senior $200K+，AI 自动化暴露：编程任务高（按 Anthropic Economic Index）、客户沟通和业务转译低 [03-role §5 §8]。详见 [03-role-cn.md](03-role-cn.md)。

### Market: PNW AI Solutions Engineer 市场 Seattle 集中，K 形分化但 AI 角色逆势

LinkedIn US Seattle 区检索 "AI Solutions Engineer" 返回 600+ 条 [04-market §1.1]，扩到全 PNW 约 1,100 条，Seattle 一城集中了该岗位族 55-65% 开放数 [04-market §1.1]。Indeed Hiring Lab 数据显示 US tech 整体招聘较 2020 基线下降 19%、entry-level 下降 25%，但 AI developers 类岗位逆势翻倍以上 [04-market §2]。Cascadia 给出的 $95-120K base 处于 PNW 科技 entry 薪资 P30-P45（被 Amazon / Microsoft 的 $155-185K total comp 抛在身后），但在 healthcare AI vendor 这个 niche 内是 P45-P55 中位线 [04-market §3]。BLS 给出 software developers occupation 2024-2034 增速 17%，healthcare IT 子赛道增速更高 [04-market §4]。AI 自动化对 Solutions Engineer 暴露曲线：高写作沟通成分 + 高客户接触成分 = 短期暴露低于纯编程岗位 [04-market §6]。详见 [04-market-cn.md](04-market-cn.md)。

---

## 文件索引

| 文件 | 回答什么问题 |
| --- | --- |
| [job-description.md](../job-description.md) | Cascadia 这条岗位 JD 原文（Purpose、Accountability、Education/Experience、Working Conditions、Compensation、Location、Application Process） |
| [01-industry-cn.md](01-industry-cn.md) | US 医疗数据分析与数字健康软件行业怎么运作？商业模式、行业体量、生命周期阶段、监管框架、未来 3-5 年的 AI 与互操作性走向？ |
| [02-company-cn.md](02-company-cn.md) | Cascadia 自家近 24 个月发生了什么？业绩、产品、客户结构、领导层、Customer-Embedded Engineering 团队定位、未来 3-5 年路径与 PE 退出窗口？ |
| [03-role-cn.md](03-role-cn.md) | "AI Solutions Engineer 嵌在 Customer Success 团队"日常是什么？entry / mid / senior 差别？AI 自动化窗口？转 Core Product Engineering 难度？ |
| [04-market-cn.md](04-market-cn.md) | PNW AI Solutions Engineer / Forward Deployed Engineer 当前行情？healthcare AI vendor vs FAANG 薪资差距？5 年 hiring 走势？ |

---

## 跨文件盲点合并

下面 6 条是四份文件共同标出的 "unable to verify" 项，按对面试和决策的重要性排序。每条都给出可以去问谁的具体抓手。

1. **Customer-Embedded Engineering 团队具体规模与汇报层级**（VP 是谁、headcount 多少、与 Core Product Engineering 的 headcount 比例），02 §9 + 03 §6 双向标 unable to verify。可在面试中直接问 hiring manager "这个 team 当前 headcount，过去 12 个月 attrition 与 net hiring，以及 FY26 计划新增多少"。
2. **12 → 30 deployment 扩张的每客户工程师配比假设**（一名 AI Solutions Engineer 同时负责几个 deployment？），02 §6 + 03 §3 双向推断但无证据。可在 info chat 中直接问"过去 12 个月一名新 grad SE 大概同时承担几个客户、单客户 ramp-up 时长平均多久"。
3. **PE owner（30% 股权）退出时间表与 IPO 窗口**，02 §1 + 02 §11 双向标 unable to verify。PE 持有期通常 5-7 年，2019 年介入意味着 2024-2026 是退出窗口，若公司选择 IPO 会影响员工 RSU 流动性；可在 face-to-face 与 hiring manager 委婉问"公司目前 funding posture 与未来 18 个月资本结构有无重大变化预期"。
4. **Cascadia Insight Assistant 真实 adoption 指标**（NRR、expansion vs new logos、单客户 query volume 月增速），02 §3 + 02 §6 标 illustrative。可在面试技术轮要求看 case study 或匿名版 product demo 数据。
5. **2-4 days/month 客户出差的实际分布**（是不是 PNW 内有几条固定线路？是不是每月 4 天集中而不是分散？），03 §3 + 03 §6 标 unable to verify。可在面试结尾问 hiring manager"过去三个月你的 team 实际平均出差天数 / 月，PNW 之外有没有跨州客户"。
6. **Cascadia 内部 AI 工具（Copilot / Cursor / Claude Code）部署比例与开放策略**，03 §8 标 unable to verify。Customer-Embedded Engineering 高度依赖文档与代码生成，内部 AI 工具配置直接影响新人前 3 年学习曲线；可在技术轮间隙礼貌问"团队当前用什么工具写 Python 和 SQL，公司有没有 sponsored Copilot license"。

---

## 如何使用这份报告

1. **当作面试 due diligence 资料**：02 §6 的"过去 12 个月事件清单"+ 03 §5 的"entry / mid / senior 拆解"+ 04 §3 的 Cascadia 薪资 P30-P45 区间是 hiring manager 面谈时可以直接核对的素材，用来验证（或反驳）你已经做出的"Cascadia 是对的方向"的初步判断。
2. **找 Cascadia 现职 / 前职校友做 info chat**：把上面 6 条盲点直接抄成问题清单，用 LinkedIn 找 1-2 个 University of Washington / 西雅图 PNW 健康 IT 圈的校友花 30 分钟交叉核对。Customer-Embedded Engineering 团队规模小，找到现职 SE 难度中等。
3. **对照 1-2 个 alternative 方向**：可以把同一个 skill 跑在 (a) Innovaccer / Health Catalyst 同序列 SE 岗位，或 (b) Snowflake / Databricks 在 Seattle 的 Solutions Engineer 岗位，或 (c) Microsoft Health & Life Sciences / Amazon Health 的 healthcare AI 工程岗。对照三组数据后再确认 anchor 是否仍然是 Cascadia。
4. **把 Python + SQL + Strand Agents 视为及格线**：技术轮的最低 bar 在 03-role §2 已列清（Python 一份生产级 code sample、SQL 含 window functions、至少一个 RAG 评估的实操经验），业务上下文（HIPAA / TJC handoff / FHIR / Charge Nurse workflow）的最小可读 mental model 在 01-industry §6 + 03-role §1 已画清。
5. **这份报告不替你做决策**：facts only 是底层原则。判断"是否要把接下来几个月 anchor 在 Cascadia"这件事完全留给学生本人。报告提供的是看清现状与 3-5 年走向的工具，不是 verdict。
