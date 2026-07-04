# Virtu Financial × Trading Operations Analyst：方向调研报告

生成日期：2026-07-03。研究对象：Virtu Financial（NASDAQ: VIRT）的 Trading Operations Analyst（entry level，Austin, TX / New York，base $125,000-$140,000）。

学生现状：Q1 = B（心里已有大致方向，但仍不确定，希望用事实降低不确定性再决定是否把它作为未来几个月的锚点）；签证 Q2 = B（国际学生，需要 sponsorship，因此 04-market 含扩展版 H1B 章节）。

本报告的最高原则是只呈现事实、不给建议。四个维度（industry / company / role / market）各自独立、并行、可核查。是否值得把这个方向作为锚点，完全由读者自己判断。

## 四个维度速览

**Industry（电子做市 / 资本市场）。** 这是一个高度集中的寡头行业：美国零售股票订单流约 80% 以上由前三大 wholesale market maker（批发做市商，即 Citadel Securities、Virtu、G1 Execution）处理，Virtu 是其中唯一的上市公司。行业盈利靠买卖价差与 rebate，最大外部变量是监管：2025 年 SEC 在新主席 Atkins 下撤回了 14 项 Gensler 时代提案（包括威胁 payment for order flow 的 Order Competition Rule），构成阶段性利好，而 Reg NMS 更细的 tick size 仍是结构性逆风。详见 01-industry-cn.md。

**Company（Virtu Financial）。** Virtu FY2025 创纪录：营收 $3,632.1M（+26.2% YoY），净利约 $912.3M（近乎翻倍），Market Making 板块贡献约 $2,408M 交易收入。重大变化：2025 年 7 月 CEO Douglas Cifu 退休，前 CTO Aaron Simons 接任。稳定性信号偏正：过去 12 个月净增约 58 人、无公开裁员、持续派息加回购；反向信号是 2025 年底 SEC 就信息隔离墙问题以 $2.5M 和解。详见 02-company-cn.md。

**Role（Trading Operations Analyst）。** 这个岗位面对的不是整体消失，而是"任务重构 + 人力密度下降 + 技能门槛上移"：T+1 结算（2024-05-28 生效）让批处理让位于实时处理，运营重心转向 exception management（异常处理）。级别实质差异清晰：entry 处理异常、mid 减少异常并设计流程、senior 对风险与团队负责，入门到 VP 常跨 10-15 年。Virtu 把 SQL 设为硬门槛、Python 从加分变必需，本身即自动化方向信号。详见 03-role-cn.md。

**Market（美国就业市场）。** 薪资呈"雇主类型双峰"：银行/托管机构的 trade-processing base 约 $62K-$90K，而 quant market maker 的 trading-ops base 高得多（Jane Street 约 $169K、HRT 约 $185K、Jump 约 $200K），Virtu 的 $125K-$140K 处于中间。岗位地理高度集中在 NY/Jersey City、Chicago、Austin/Dallas，几乎全 onsite。前瞻偏平到偏降：BLS 预计 Brokerage Clerks 2024-2034 约 -9.5%。H1B 方面做市商与大行都 sponsor，2025 年 9 月新增的 $100K 新申请费基本豁免境内 OPT→H1B 的 change-of-status。详见 04-market-cn.md。

## 报告索引

| 文件 | 回答的核心问题 |
|---|---|
| [01-industry-cn.md](01-industry-cn.md) | 电子做市/资本市场行业怎么赚钱、格局如何、监管与 AI 走向 |
| [02-company-cn.md](02-company-cn.md) | Virtu 的客户、财务、历史、文化强度、稳定性与未来 3-5 年 |
| [03-role-cn.md](03-role-cn.md) | Trading Ops Analyst 日常做什么、entry/mid/senior 差异、AI 替代 vs 增强 |
| [04-market-cn.md](04-market-cn.md) | 美国该岗位的存量、薪资曲线、招聘公司、地理、H1B 与前瞻 |

## 跨文件盲点汇总（需要学生自己补）

1. **该岗位在美精确活跃开放数，以及按窄 title 的薪资 P25/P75**（04 未能核实）。可在 LinkedIn/Indeed 用精确 title 手动计数，或找 senior 学长确认。
2. **Virtu 该岗位真实的内部晋升节奏、在职者院校/前雇主/去向**（03 未能核实）。可通过 LinkedIn 搜 Virtu Trading Operations 现任者画像，或 info chat 直接问。
3. **做市商 vs 投行 vs 券商/清算机构（DTCC、BNY、State Street）对国际学生的具体 sponsor 立场**（04，Q2=B 关键）。可在 myvisajobs / h1bdata 逐家核实。
4. **Virtu 当前精确美国市场份额，以及 Citadel Securities / Jane Street 的 2025 最新估值**（01 因 SEC 10-K 被 403 拦截未核实）。可查最新 10-K 或行业研究。
5. **Virtu 官方 attrition 率、明确的 RTO 天数、新任 CTO 姓名**（02 未能核实）。可看 Glassdoor 最新条目或 info chat。
6. **编程测试平台口径**：JD 说 HackerRank，社区有 Codility 说法（03）。可向近期面试者确认，直接影响面试准备。

## 如何使用这份报告

- 拿盲点清单里的具体问题去做 info chat：优先问 Virtu 现任 Trading Ops、以及做市商 vs 大行的 sponsor 立场。
- 把这份报告当成对照基线，用同一个 skill 再跑 2-3 个替代方向（例如大行 operations、或 quant 做市商的其他 entry 岗），横向比较薪资曲线与 AI 替代风险。
- 阅读 04-market-cn.md 的薪资部分时，注意不要把公司整体的 LCA 中位当作该岗位薪资，二者口径不同。
- 把 03-role-cn.md 的 entry/mid/senior 差异当作面试与职业规划的框架，重点关注 exception management 与自动化技能。
