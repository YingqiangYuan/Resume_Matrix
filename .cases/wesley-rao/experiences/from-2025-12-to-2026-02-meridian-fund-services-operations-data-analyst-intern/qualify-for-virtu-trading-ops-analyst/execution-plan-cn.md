# 执行计划：Virtu Financial, Trading Operations Analyst

- **生成日期**：2026-07-04
- **对标岗位**：Trading Operations Analyst @ Virtu Financial（Austin, TX / New York，base $125K-$140K，接受应届）
- **本计划响应的 gap 分析**：[gap-analysis-cn.md](gap-analysis-cn.md)（1 个 🔴 Core + 3 个 🟡 Important + 3 个 🟠 Nice-to-have）
- **本计划支撑的 case**：[case-cn.md](case-cn.md)（已过三轮 review，判定 approve；自动化对账与异常 break 管理工具）
- **JD 说明**：JD 未单独放本文件夹，但其原文（岗位职责、SQL+Python 硬门槛、HackerRank OA 流程、no finance experience necessary 等）已完整嵌在 gap 分析的引用里，本计划的"JD 连接点"字段据此对齐。
- **学生投入**：约 20 小时/周（学期内兼职强度），计划周期约 12 周（2026-08-18 至 2026-11-08），对齐 case gantt 的阶段零到阶段四，并覆盖"1-3 个月内启动求职"窗口。

---

## 1. 什么是 mini-POC（先把定义钉死）

这一节必须先读，因为它是本 skill 最常被误读的地方。**mini-POC 是一个 skill-learning（技能学习）项目，不是一个假装的业务项目。**

它的目的，是把一个具体技术技能（一个框架、一种数据格式、一套部署模式、一条合规要求），从"我读过文档"推进到"我亲手写过、跑过、且撞过至少一堵必须 debug 穿过去的墙"。

**好的 mini-POC 例子（通用）：**

- 用建一个覆盖 50 篇文章的 Wikipedia 问答 agent 来学 Strand Agents。
- 用在一个 CDK app 里 provision 一套 Lambda + S3 + IAM 来学 AWS CDK Python。
- 用写一个把 Synthea Bundle JSON 拍平成三张关系表的 Python 脚本来学 FHIR 解析。
- 用在 Wikipedia 语料上建一个 50 条 Q-A 黄金集的 hit-rate + faithfulness 评测台来学 RAG evaluation。

**坏的 mini-POC 例子（不要产出这种）：**

- "为一家虚构的妇产医院建一个医疗 BI 看板"（这是假业务项目，不是技能训练）。
- "建一个带 SSO 和审计日志的多租户 agent 平台"（太宽、糊成了真实产品）。
- "交付一套面向客户的心内科临床笔记 RAG 管道"（又是假企业语境）。

语料和数据可以完全公开（Wikipedia、Synthea、HuggingFace 数据集、公开 CSV，或自己按规则生成的合成数据）。不需要真实企业语境，不需要客户。产出物允许看起来像一个周末 hack，只要它逼出的底层技能正是目标技能。

**这条规则的深层原因**：学生已经有了一个由 `mini-project-design` 产出的、承载业务语境的完整 case（那个对账工具）。mini-POC 存在的意义，是当面试官问"你用过 X 吗"时，学生能回答"用过，我建了一个小的 Y 来学 X，这是 repo"。**它们是证据片段，不是简历条目。** 如果你发现某个 POC 开始长得像"某某公司对账平台"，那就是失败了，把它改回"一个 SQL 对账查询练习"这种技能训练的名字。

**落到本案例的语境**：Wesley Rao 已经有对账工具这个 case 承载业务故事。下面 7 个 POC 是把 case 里每个技术决策背后的技能，单独拎出来练到"亲手写过、能讲取舍"。其中最核心的 POC-01（SQL）会和 case 项目物理重叠（对账引擎本身就是 SQL 的落地载体），这是刻意的：SQL 是他唯一的 🔴 Core gap，也是 OA 硬闸，必须用最真刀真枪的方式练。

---

## 2. Case 对齐摘要

case 的技术重心是**一个跑在真实 PostgreSQL 上的 SQL 对账引擎**，关键技术决策有 7 条，本计划逐条映射到 POC：

- 决策 1/2/3：**对账逻辑用 SQL 写在 PostgreSQL 里**（不用 pandas）、**用 FULL OUTER JOIN**（不用 INNER）、**用 PostgreSQL**（不用 SQLite）。→ POC-01。
- 决策 4：**交易对账做 T+1 结算日过滤**（`settle_date <= as_of_date`）。→ POC-02（domain 理解驱动）+ POC-01（落地成 SQL）。
- 决策 5：**分批成交先聚合到订单级再匹配**，按 as-of 口径。→ POC-03（用 Python 重写他 C++ 撮合项目里的分批成交模块）+ POC-01。
- 决策 6：**持久化 break 台账 + aging + 状态机**（resolved vs 数据缺失 vs 复发）。→ POC-01（窗口函数 + 状态机）+ POC-05（每日调度刷新台账）。
- 决策 7：**透明规则打分**（不用 ML）。→ POC-01（打分 SQL）。

补充：case §6.5 的看板与调度对应 POC-05；§8 的手工 vs 工具对照对应 POC-06；JD 的对内外精确沟通对应 POC-07（贴尾英文写作）。**Python OA 手感（POC-03）和数学 brain teaser（POC-04）是并行刷题轨，不由某条 case 决策直接派生**（brain teaser 尤其是纯面试形态训练，见 POC-04 的标注）。

---

## 3. 执行原则与时间线

**总量**：约 20 小时/周 × 12 周 ≈ 240 小时。这个预算足以把唯一的 🔴 Core gap（SQL）从零推到面试可信 + 撑起 case 的对账引擎，同时让 3 个 🟡 和 3 个 🟠 到"能讲 2 分钟"。**它不足以把任何 gap 做到 production-grade，本计划也不承诺这一点。**

**三条硬原则（来自 gap 分析 handoff，不可动）：**

1. **SQL 排第一、第一周就上、不可延后。** 它是 OA 和技术电面的硬闸，不补则前面所有强项归零。POC-01 独占前 3 周主时段（对应 case 阶段零"SQL 集中攻坚"），并在 Week 3 做一次 SQL OA 模拟当 checkpoint。这是整个计划最不能失守的部分。
2. **不要把 SQL 过度扩展到 DBA / 调优 / 分布式。** JD 要的是"能写查询做对账"，不是数据库管理员。POC-01 的边界严格锁在 JOIN / GROUP BY / 窗口函数 / 日期函数 / CTE，不碰索引调优、分区、复制这些噪音。
3. **Python 刷题（POC-03）+ brain teaser（POC-04）并入同一条"限时刷题"并行轨，贯穿全程、与对账项目互不阻塞。** 它们不占独立主周，而是每日/每周固定时段推进（gap 分析纠缠三）。

**技术锁定**：PostgreSQL + Python + SQL，精确匹配 JD 的 "SQL and Python" 栈。**不引入 Spark、云数仓、任何 ML 框架、Bloomberg 终端**（前者是 JD 没要求的重型设施，后者学生拿不到且仅加分）。

**深度 vs 可信度的分配**：POC-01 拿"深度treatment"（真做、能讲每个 JOIN 的取舍）；POC-02/03/04 拿"面试可信treatment"（能开场 30 秒 + 深入 2 分钟）；POC-05/06/07 是对账项目的自然副产品或贴尾 wrapper，不单独重投入。

**两个要避免的失败模式**：(1) 完美主义拖死，每个 POC 到 demo-ready（70-80 分能讲故事）就停手转下一个；(2) 把 SQL 学成 DBA，严守原则 2 的边界。

**不在本计划范围内**：简历改写、recruiter 沟通、薪资谈判、行为面 STAR 整理，这些是 onsite 倒数两周的另一条工作流；英文沟通的深度打磨交给 `qualify-mock-interview`（本计划只把它作为贴尾 wrapper 起步）。

---

## 4. POC 优先级矩阵

按"面试影响"、"与其他 POC 的纠缠度"、"时间成本"三维排序。严重度沿用 gap 分析的 🔴/🟡/🟠。

| POC | Gap / 技能 | 严重度 | 面试影响 | Entanglement（解锁 / 共用什么） | 估时 | 综合排序 |
|---|---|---|---|---|---|---|
| POC-01 | SQL 对账实操（PostgreSQL） | 🔴 | 极高（OA 硬闸） | 是 case 对账引擎本体；与 POC-02/05 共用同一份合成数据 + 库 | 15 天 | 1 |
| POC-03 | Python 面试/OA 就绪 | 🟡 | 高（OA 第一关） | 重写 C++ 分批成交模块，喂 case 交易对账；与 POC-04 共用刷题轨 | 8 天 | 2 |
| POC-02 | 交易生命周期/结算对账 domain | 🟡 | 高（onsite/行为面可信度） | 驱动 POC-01 的 T+1 过滤；与 POC-01 共用合成数据设计 | 5 天 | 3 |
| POC-04 | 数学 brain teaser/快速定量 | 🟡 | 中高（电面夹带） | 与 POC-03 共用"限时刷题"并行轨 | 6 天 | 4 |
| POC-05 | 自动化/调度叙事 | 🟠 | 中 | case 对账引擎的调度层；复用 POC-01 的库和查询 | 4 天 | 5 |
| POC-06 | Excel/金融数据工具 | 🟠 | 中低 | 复现手工对账基线，喂 case §8 的对照指标 | 3 天 | 6 |
| POC-07 | 专业英文沟通（贴尾 wrapper） | 🟠 | 中 | 覆盖在所有 POC 上，每个硬 POC 结束写一份英文一页纸 | 4 天 | 7 |

**排序口径说明**：这里的"面试影响"是 Virtu 这个具体 JD + 招聘流程下的影响，不是通用市场认可度。POC-01（SQL）排第一没有悬念，它是 OA 直接考、不过就出局的硬闸。POC-03（Python）排第二是因为 OA 第一关就是 HackerRank Python 编程题。POC-02（domain）排第三高于 POC-04，是因为它决定 onsite 和行为面"你知道这岗位在干嘛吗"的可信度。

---

## 5. 🔴 Core POC 深挖

Core 只有一个，但它占了约 15 天预算（全程约 6 分之 1 强），是整个计划的真正主干。8 条统一结构：技能 / 输入 / 产出物 / 成功标准 / 支撑的 case 决策 / JD 连接点 / 估时 / tutorial 索引 / 踩坑或衍生。

### POC-01：用建一个两表对账查询集来学 SQL（PostgreSQL）

- **技能**：SQL 的对账核心动作，`FULL OUTER JOIN` 找单侧存在的记录、`GROUP BY + HAVING` 聚合、窗口函数 `ROW_NUMBER()` 排名、日期函数做 aging 和 T+1 过滤、`WITH`（CTE）把复杂逻辑分步。这是 Wesley 唯一的 🔴 Core gap，简历技术栈零 SQL。
- **输入 / setup**：本地用 Docker 起一个 PostgreSQL 实例；用一个合成数据生成器造两套本应一致的账（内部持仓 vs 托管行持仓、内部成交 vs 券商确认、现金两侧），按已知规则注入 break 并记入"真值台账"。全部公开可自建，无需任何付费资源。
- **产出物**：(1) 一组对账 SQL 查询（持仓/现金/交易各一套）；(2) 合成数据生成器 + 建表 DDL；(3) 一份"每条查询在找哪类 break、为什么用这个 JOIN"的注释文档；(4) 一份 HackerRank/Codility 中等 SQL 题的限时练习记录。
- **成功标准（checklist，面试官"你做过 X 吗"能直接映射成 yes/no）**：☐ 能现场手写 INNER/LEFT/FULL OUTER JOIN 并说清各自何时用；☐ 能写 `GROUP BY + HAVING` 聚合出每账户风险敞口；☐ 能用一个窗口函数给每账户 break 排名；☐ 能用一句 SQL 找出"内部有、托管行没有"的 break（即一个 break）；☐ 能说清 WHERE 与 HAVING 的区别、LEFT JOIN 后 NULL 意味着什么；☐ 在中等 SQL 题上能不查文档、限时做对。
- **支撑的 case 决策**：决策 1（SQL on PostgreSQL 不用 pandas）、决策 2（FULL OUTER JOIN 不用 INNER）、决策 3（PostgreSQL 不用 SQLite）、决策 6（aging 用窗口函数）、决策 7（打分用 SQL 规则）。**5 条决策里 5 条落在这个 POC 上，它就是 case 的心脏。**
- **JD 连接点**：JD "Strong experience with **SQL** and Python"；招聘流程第一关 HackerRank 含 SQL；调研称技术电面"考 Python 和 SQL"、"Virtu 把 SQL 设为硬门槛"。
- **估时**：15 天。分布：3 天基础（SELECT/JOIN/GROUP BY）、3 天 FULL OUTER JOIN + 窗口函数、3 天日期函数 + T+1 过滤 + CTE、3 天把查询落成 case 的对账引擎、3 天限时刷题 + SQL OA 模拟。
- **tutorial 索引**：[tutorials/01-sql-reconciliation-cn.md](tutorials/01-sql-reconciliation-cn.md)。
- **踩坑/衍生**：最常见的坑是习惯性用 `INNER JOIN`，把"只有一边有"的 break 悄悄过滤掉，这恰恰是要找的东西。衍生：跑通单日对账后，加一个"跨天 aging"的持久化台账（决策 6），用 `LEFT JOIN` 昨天的台账 + 日期差算 aging，这一步同时练了 SQL 又直接产出 case 阶段三的核心。

---

## 6. 🟡 Important POC 深挖

3 个 POC，对应 gap 分析的 3 个 🟡。共同特征"做好显著加分、不做也不一票否决"，总预算约 19 天。目标是"每个都能 30 秒开场 + 2 分钟深入"。

### POC-03：用 Python 重写 C++ 分批成交模块 + 限时刷题来练 OA 手感

- **技能**：Python 在限时 OA 场景下的流畅度，常用惯用法（list/dict 推导、`collections`、`enumerate/zip`、字符串处理）、以及把已有算法直觉用 Python 快速敲出来。Wesley 会 Python（青岚实操 + Pandas），但算法主力是 C++，刷题手感未验证，这是"手感迁移"非"从零学"。
- **输入 / setup**：把他 C++"动态事件驱动撮合系统"里的分批成交（partial fill）匹配模块用 Python 重写并跑通；配一批 HackerRank 中等难度题（哈希去重、双指针、简单 DP、字符串解析）。
- **产出物**：(1) Python 版分批成交聚合模块（喂给 case 的交易对账）；(2) 一个刷题解答仓库（按题型分类）；(3) 一份"C++ 到 Python 我踩了哪些语法/惯用法坑"的短记。
- **成功标准**：☐ 能在 HackerRank 中等题上用 Python 限时做对不卡语法；☐ 能把 C++ 分批成交模块用 Python 重写并跑通；☐ 能讲清 `defaultdict`/`Counter` 等惯用法什么时候用；☐ 能解释 as-of 聚合口径（截至某日已成交量，不假设订单填满）。
- **支撑的 case 决策**：决策 5（分批成交先聚合到订单级再匹配，按 as-of 口径）。
- **JD 连接点**：招聘流程 "an online programming test via HackerRank"；调研"电面考 Python"。
- **估时**：8 天，分散在全程并行轨（每周固定时段），不占独立主周。
- **tutorial 索引**：[tutorials/03-python-oa-readiness-cn.md](tutorials/03-python-oa-readiness-cn.md)。
- **踩坑/衍生**：坑是"C++ 思维写 Python"（手动 for 循环而不用推导/内置函数），会在限时场景吃亏。衍生：把 Python 版分批成交模块和 POC-01 的 SQL 版做一次结果对照，验证两条路算出的聚合量一致，这本身是很好的面试谈资。

### POC-02：用建一份交易生命周期 domain 资料 + 落地 T+1 过滤来学结算对账语言

- **技能**：post-trade operations 的行业语言，能用术语讲清一笔交易从成交（execution）到清算（clearance）到交收（settlement）的链路，定义 break、解释 T+1 为什么压紧对账时间窗、custodian 对账是什么。Wesley 对这套词汇零暴露，C++ 项目给了工程直觉但没给行业语言。
- **输入 / setup**：读透 gap 分析引用的 `03-role` 调研 + 公开的 post-trade 科普；把理解沉淀成资料，并落地成 POC-01 数据里的 `settle_date` 字段和过滤逻辑。
- **产出物**：(1) 一页交易生命周期术语表（execution/clearance/settlement/break/T+1/custodian/fail to deliver）；(2) 一张结算时间线图（T 日成交 → T+1 交收）；(3) 一个 break 分类举例（内部记 100 股、托管行记 90 股）。
- **成功标准**：☐ 能 60 秒讲清一笔交易从成交到交收的链路；☐ 能定义 break 并举一个具体例子；☐ 能解释 T+1 为什么逼着对账在开盘前抢完、为什么逼着流程自动化；☐ 能说清 agency-side（基金行政）与 principal（Virtu 做市）对账语境的差异。
- **支撑的 case 决策**：决策 4（T+1 结算日过滤），domain 理解直接变成 `WHERE settle_date <= as_of_date` 这句 SQL。
- **JD 连接点**：JD "managing the **clearance, settlement, and reconciliation** of all trades and positions"；JD 明写 "No finance experience is necessary"（所以这不是 OA 闸，是 onsite 可信度）。
- **估时**：5 天，与 POC-01 的合成数据设计交织进行。
- **tutorial 索引**：[tutorials/02-trade-lifecycle-domain-cn.md](tutorials/02-trade-lifecycle-domain-cn.md)。
- **踩坑/衍生**：坑是学成"背名词"而不能落地，一定要把每个术语接到 POC-01 的一段 SQL 或一个字段上。衍生：准备好"为什么是 ops 不是 quant"的真诚答案（gap 分析 verdict 条件二），这是行为面必问。

### POC-04：用刷 30-50 道经典 quant brain teaser 来练快速定量推理

- **技能**：brain-teaser 形态下的临场速度，期望值、条件概率、组合计数、简单博弈的快速口算 + 边算边讲思路。Wesley 概率统计底子强（GPA 接近满绩、3.9+，ML 背景），**知识不缺，缺的是这种特定形态的练习量**。
- **输入 / setup**：一本经典 quant brain teaser 题集（如 Heard on the Street / 50 Challenging Problems in Probability）+ 一个记录解题过程的 notebook。
- **产出物**：(1) 30-50 道经典题的解答 notebook（含推理过程，不只答案）；(2) 一份"最容易错的 5 类题型"的自我总结。
- **成功标准**：☐ 能在 60-90 秒内口算并讲清一道经典题（如两骰子和为 7 的概率、破损硬币期望翻面次数）；☐ 能边算边把思路讲清（面试看推理过程）；☐ 覆盖期望值/条件概率/组合/简单博弈四类。
- **支撑的 case 决策**：**无直接映射，特此标注**。这是纯面试形态训练，不由任何 case 技术决策派生，对应的是 JD "Outstanding **quantitative problem-solving** skills" 和调研"电面夹带 math brain teaser"。它合理地活在 case 之外，是并行刷题轨的一部分（gap 分析纠缠三），不是 case 缺了东西。
- **JD 连接点**：JD "Outstanding quantitative problem-solving skills"；调研"电面夹带若干 math brain teaser"。
- **估时**：6 天，全程并行轨，与 POC-03 共用"限时刷题"时段。
- **tutorial 索引**：[tutorials/04-quant-brain-teaser-cn.md](tutorials/04-quant-brain-teaser-cn.md)。
- **踩坑/衍生**：坑是"只对答案不讲过程"，面试看的是推理。衍生：找人做 5 分钟口头 brain teaser 快问快答，暴露"会算但讲不清"的死角。

---

## 7. 🟠 Nice-to-have POC 深挖

3 个 POC，都是对账项目的自然副产品或贴尾 wrapper，总预算约 11 天，不单独重投入。

### POC-05：用给对账查询加一个每日调度来学自动化叙事

- **技能**：把一个手工流程脚本化/自动化，用 cron 或 APScheduler 让对账在凌晨源文件落地后自动跑、刷新 break 台账。
- **输入 / setup**：复用 POC-01 的 PostgreSQL 库和对账查询，包一层调度。
- **产出物**：(1) 一个每日调度脚本；(2) 一份运行日志；(3) 一段"从手工找 break 到脚本自动分类标记"的叙事。
- **成功标准**：☐ 调度能在指定时间自动跑全量对账；☐ 能讲清"哪些步骤适合自动化、哪些必须留给人判断"；☐ 能说清这一层如何对接上游 IT 的数据管道（case §5 的协作接点）。
- **支撑的 case 决策**：决策 6（持久化台账每日刷新）+ case §6.5 调度层。
- **JD 连接点**：JD "work closely with software engineers to further **automate**... operational workflows"。
- **估时**：4 天。
- **tutorial 索引**：[tutorials/05-automation-scheduling-cn.md](tutorials/05-automation-scheduling-cn.md)。
- **踩坑/衍生**：坑是把调度做成"生产级"（告警/容错/SLA），那超出 case scope（§4 明确 out of scope）。衍生：加一个"跑完发一封汇总邮件"，是很轻的自动化叙事加分。

### POC-06：用一个 Excel 对账工作簿复现手工基线来对照工具价值

- **技能**：Excel 做对账（VLOOKUP/数据透视）+ 对 Bloomberg 等金融数据工具的基本认知。目的不是把 Excel 学深，而是复现"手工对账"基线，给 case §8 的"手工 vs 工具"对照提供真实一端。
- **输入 / setup**：拿 POC-01 的一个账户合成数据，用 Excel 手工核对一遍并计时。
- **产出物**：(1) 一个 Excel 对账工作簿；(2) 手工核对单账户的计时记录（n=3，诚实标注非严格研究）；(3) 一段 Bloomberg 是什么/做什么的认知短记。
- **成功标准**：☐ 能用 Excel 复现一次手工对账并讲清它慢在哪；☐ 能诚实给出手工 vs 工具的量级对照；☐ 知道 Bloomberg 在这类岗位的定位（加分非必需）。
- **支撑的 case 决策**：case §8"关于省时间的诚实处理"（手工计时基线）。
- **JD 连接点**：调研"同类岗 Excel + SQL 必需、Bloomberg 加分"。
- **估时**：3 天。
- **tutorial 索引**：[tutorials/06-excel-recon-tools-cn.md](tutorials/06-excel-recon-tools-cn.md)。
- **踩坑/衍生**：坑是花时间追 Bloomberg 终端（学生拿不到，仅加分），知道它是什么即可，不投入。

### POC-07：给每个 POC 贴尾写一份英文一页纸来练专业沟通

- **技能**：把技术工作翻译成精确、简洁的英文书面表达。这是 wrapper，不是独立项目。gap 分析明确这一项深度打磨交给 `qualify-mock-interview`，本 POC 只做"起步 + 积累样本"。
- **输入 / setup**：POC-01/02/03/05 每个完成后，贴尾写一份英文一页纸（问题、做法、产出、下一步）。
- **产出物**：4 份英文一页纸（每份约 250-300 词），汇总成一个 writing sample。
- **成功标准**：☐ 每份能被一个不懂细节的人 90 秒读懂；☐ 用词精确、无冗余；☐ 每份有可执行的 next step。
- **支撑的 case 决策**：无直接 case 决策映射；对应 JD 的软性要求，是所有 POC 的"输出层薄膜"。
- **JD 连接点**：JD "communicate information **precisely and with agility** to parties both internal and external"。
- **估时**：4 天，分摊在每个硬 POC 结束的半天。
- **tutorial 索引**：[tutorials/07-professional-english-writing-cn.md](tutorials/07-professional-english-writing-cn.md)。
- **踩坑/衍生**：坑是写成中式英语长句，每写完用朗读法自检，听到卡顿就重写。深度打磨留给 mock interview。

---

## 8. 跨 POC 纠缠分析

7 个 POC 高度纠缠，排期必须利用这一点摊薄成本。最强的三条耦合：

**耦合一（最重要）：POC-01 + POC-02 + POC-05 共用同一份合成对账数据 + 同一个 PostgreSQL 库，三者其实是 case 对账引擎的三层。** POC-01 是 SQL 查询本体、POC-02 是驱动 `settle_date` 过滤的 domain 理解、POC-05 是包在外面的每日调度。所以合成数据只建一次，这三个 POC 前后脚做，摊薄建库和造数的成本。这条线是计划的主干，占 Week 1 到 Week 11 的主时段。

**耦合二：POC-03 的 Python 分批成交模块 = case 交易对账的一块，与 POC-01 共用同一份 trade/fill 合成数据。** Wesley 用 Python 重写 C++ 撮合模块后，可以和 POC-01 的 SQL 聚合结果做对照验证，一份数据同时喂两个 POC，还产出一个"两条路结果一致"的面试谈资。

**耦合三：POC-03（Python 刷题）+ POC-04（brain teaser）共用"限时刷题"并行轨。** 两者都靠限时训练、都与对账项目互不阻塞，并入同一条每日/每周刷题节奏，不各自占主周。

**POC-07（英文写作）是覆盖在所有 POC 上的薄膜**，不排独立周，贴在每个硬 POC 尾巴上，到计划末自然积累出 writing sample。

---

## 9. 教程目录索引

教程内容不在本 skill 范围（由 `qualify-coach` 在交互教学中撰写），这里只锁文件名和归档位置。每个 POC 子目录建议含 `README-cn.md`（讲清做什么）、`src/`（代码）、`data/` 或 `sql/`（数据或查询）、`docs/`（英文一页纸等产出）。

| POC | Gap / 技能 | 严重度 | Tutorial 文件 | POC 脚手架路径 |
|---|---|---|---|---|
| POC-01 | SQL 对账实操 | 🔴 | `tutorials/01-sql-reconciliation-cn.md` | `pocs/poc-01-sql-reconciliation/` |
| POC-02 | 交易生命周期 domain | 🟡 | `tutorials/02-trade-lifecycle-domain-cn.md` | `pocs/poc-02-trade-lifecycle-domain/` |
| POC-03 | Python OA 就绪 | 🟡 | `tutorials/03-python-oa-readiness-cn.md` | `pocs/poc-03-python-oa-readiness/` |
| POC-04 | 数学 brain teaser | 🟡 | `tutorials/04-quant-brain-teaser-cn.md` | `pocs/poc-04-quant-brain-teaser/` |
| POC-05 | 自动化/调度 | 🟠 | `tutorials/05-automation-scheduling-cn.md` | `pocs/poc-05-automation-scheduling/` |
| POC-06 | Excel/金融工具 | 🟠 | `tutorials/06-excel-recon-tools-cn.md` | `pocs/poc-06-excel-recon-tools/` |
| POC-07 | 专业英文沟通 | 🟠 | `tutorials/07-professional-english-writing-cn.md` | `pocs/poc-07-professional-english-writing/` |

---

## 10. 周度排期（12 周，2026-08-18 至 2026-11-08）

节奏：SQL 主干前置（对齐 case 阶段零到阶段四），Python/brain teaser 全程并行贴在"次要"列，英文一页纸贴尾。Mock interview 在 Week 8、Week 12；SQL OA 模拟在 Week 3（真实 OA 闸的 checkpoint）。

| 周次 | 起止 | 主推 POC | 次要（并行/贴尾） | 里程碑 |
|---|---|---|---|---|
| W1 | 08-18~08-24 | POC-01 SQL Day1-5（SELECT/JOIN/GROUP BY） | POC-04 brain teaser 启动 | SQL 环境起、基础查询跑通 |
| W2 | 08-25~08-31 | POC-01 Day6-10（FULL OUTER JOIN、窗口函数） | POC-03 Python 刷题启动 + POC-04 | 能手写 FULL OUTER JOIN + ROW_NUMBER |
| W3 | 09-01~09-07 | POC-01 Day11-15（日期函数、CTE、对账查询找 break） | POC-04 | **SQL OA 模拟**（HackerRank 中等 SQL 限时），阶段零收尾 |
| W4 | 09-08~09-14 | POC-02 domain Day1-5 + 合成数据 schema | POC-03 + POC-04 | 合成数据生成器 + 建表（case 阶段一） |
| W5 | 09-15~09-21 | POC-01 落地：持仓/现金对账引擎 | POC-07 英文一页纸#1 | 持仓/现金对账跑通（case 阶段二上半） |
| W6 | 09-22~09-28 | POC-01 + POC-02：交易对账 + T+1 过滤 | POC-03 重写 C++ 分批成交模块 | settle_date 过滤跑通 |
| W7 | 09-29~10-05 | POC-01：分批成交聚合 + as-of 口径 | POC-07 #2 + POC-04 | 交易对账三难点全跑通（case 阶段二完成） |
| W8 | 10-06~10-12 | POC-01：异常分类 + 打分 + aging 状态机 | **Mock #1**（SQL + domain 深挖） | break 台账 + 状态机（case 阶段三） |
| W9 | 10-13~10-19 | POC-05 每日调度 | POC-07 #3 + POC-03 | 每日自动对账跑通 |
| W10 | 10-20~10-26 | POC-06 Excel 对账 + Bloomberg 认知 | POC-03 Python OA 冲刺 | Excel 手工基线 + 计时对照 |
| W11 | 10-27~11-02 | POC-01：Streamlit 看板 + 指标测量 | POC-07 #4 | 看板 + 召回率/假阳性指标报告（case 阶段四） |
| W12 | 11-03~11-08 | 收口：portfolio + README + demo | **Mock #2**（full loop：SQL OA + 行为面 + why ops not quant） | 完整 case 可讲 + 全部 POC 有产出物 |

**Mock #1（W8）** 重点考 SQL 现场写查询 + 交易生命周期 domain，对应 POC-01/02 进度。**Mock #2（W12）** 是 full loop：SQL OA 限时 + 行为面（含"为什么 ops 不是 quant"）+ case 深挖。**W3 的 SQL OA 模拟**是最关键的早期 checkpoint，如果这时限时 SQL 还做不对，要立刻把 POC-01 的主时段再往后压两周、砍掉一切 🟠。

**12 周结束的状态目标**：POC-01 有可运行的对账 SQL + case 对账引擎跑通 + 能限时做对中等 SQL 题；POC-02/03/04 各能 30 秒开场 + 2 分钟深入；POC-05/06/07 有可展示副产品；整个 repo 在 GitHub public；4 份英文一页纸成 writing sample。这个状态足以支撑 Virtu 的 OA + 技术电面 + onsite。

---

## 11. 反馈路径（POC 也是 case 的可行性探针）

除了产出面试证据，mini-POC 还是一次低成本的 case 可行性试跑。**如果 POC-01（SQL，🔴 Core）在多次 AI 辅导后仍然连基本进展都做不出来**（比如三周下来仍写不出正确的 FULL OUTER JOIN 对账、限时 SQL 题持续做不对），那不是"再磨狠一点"的信号，而是 **case 设计对当前吸收能力可能过难**的信号。

正确的动作不是硬磨，而是把信号反馈回 `mini-project-design` 的 case-difficulty-rollback 模式：**开一个全新终端**（原设计对话被三轮已辩护的决策污染，会本能地维护现有 case），带上辅导反馈作为输入，拿一个更低难度的新 `case-cn.md`；然后对新 case 重跑 `qualify-gap-analyze`（gap 可能移位）→ 重跑本 skill → `qualify-coach` 在重建的计划上恢复。

对 Wesley 这个案例，SQL 是唯一 🔴 且 gap 分析判它"最容易补的一项"，所以触发 rollback 的概率不高；但这条逃生通道要知道它存在。**在第 12 周的 mock 之前发现 case 不可建，远比第 12 周才发现便宜。** 判断阈值就放在 W3 的 SQL OA 模拟：那一关过不去，先调计划（延长 SQL 窗口）；如果延长后 W6 仍无基本进展，才升级到 case rollback。
