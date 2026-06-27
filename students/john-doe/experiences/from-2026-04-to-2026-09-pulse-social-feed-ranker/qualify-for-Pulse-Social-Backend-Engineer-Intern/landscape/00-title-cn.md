# Pulse Social × Backend Engineer Intern Summer 2026 方向调研报告

> 这是为 08 教学示例编写的 abbreviated landscape index。在真实 workflow 里, `understand-landscape` skill 会产出完整的 5 篇文档 (00-title + 01-industry + 02-company + 03-role + 04-market)。这里为了节省篇幅只保留 index, 用以演示 08 工作流阶段 1 的产出形态, 不展开 01-04 深篇。CedarRidge qualify-for 文件夹里有完整 5 篇示例, 学生可参考那个看深篇长什么样。

## 报告元信息

- 公司: Pulse Social (虚构, consumer social product, 200 employees, 约 8M MAU)
- 岗位: Backend Engineer Intern, Summer 2026
- 生成日期: 2026-01-20
- 学生定位: UW M.S. Computer Science 在读, 2026 年 12 月毕业, 美国公民, 西雅图本地, 无 production backend / distributed systems / Go 经历
- 报告原则: facts only, no recommendation

## 四个维度的 (abbreviated)

### Industry: US 消费社交 (sub-FAANG niche segment)

US consumer social apps 行业当前的玩家分两层: FAANG (Meta, Snap, ByteDance) 占据流量主体; 中型独立社交 (Discord, BeReal, Strava, Pinterest, Pulse 本身) 在细分场景维持 niche。Pulse 定位是 sub-Meta 规模, 但比同体量产品在 feed quality 工程实践上更投入。监管框架: CDA 230 加各州 (CA, FL, TX) 不断迭代的未成年保护法。AI 时代 feed ranking 投入持续, 但中型公司预算远低于 FAANG, 玩法是"少而精"。完整篇省略。

### Company: Pulse Social (200 人, Seattle 总部, 私有)

8M MAU, B2C, 主营 home feed product。后端栈正在从 Python monolith 拆向 Go microservice。Platform Engineering 团队 14 人, Feed Infra subgroup 是当前 quarter 主押注。recent: data science 团队抱怨 deploy tax 影响实验速度, 这是 backend 团队 2026 投入 feed-ranker microservice 的核心 trigger。完整篇省略。

### Role: Backend Engineer Intern (consumer social, feed infra subgroup)

混合体 = Go production code 加适度的客户向架构思考。日常 60-70% 写代码, 10-15% 写文档, 15-20% review + design 讨论。Mentor 1:1 频次高, intern own 一个 focused 项目而非杂工。Intern 转正率 (rumor) 约 60-70%, 没有 public data 验证。AI 自动化暴露: 编程任务高 (Anthropic Economic Index), 系统设计中, 跨服务通信调试低。完整篇省略。

### Market: PNW Backend Engineer Intern 市场

Seattle 是 PNW backend intern hiring 中心 (Amazon, Microsoft, Pulse, Smartsheet, Tableau 等)。LinkedIn 当前 "Backend Engineer Intern Seattle" 检索 ~400 条 active req, 但 stale ratio 估计 40%+。薪资: P50 大约 USD 45-55/hr, Pulse 给的 50-65/hr 在中位偏上, 加 $8K 搬迁补贴是 PNW intern 较为优厚的待遇。趋势: 2022 peak 后 backend intern 招聘整体回落约 20%, AI/ML intern 逆势增长。完整篇省略。

## 文件索引

| 文件 | 状态 | 回答什么问题 |
|---|---|---|
| [job-description.md](../job-description.md) | 已有 | Pulse Backend Engineer Intern JD 原文 |
| 01-industry-cn.md | abbreviated | US consumer social 行业全景 (本文件已涵盖) |
| 02-company-cn.md | abbreviated | Pulse 公司维度 |
| 03-role-cn.md | abbreviated | Backend Engineer Intern 角色维度 |
| 04-market-cn.md | abbreviated | PNW Backend Intern 就业市场维度 |

完整 5 文档版省略 (本目录只保留 00-title)。在真实场景跑 `understand-landscape` skill 会一次产出完整 5 篇, 总计 ~25K 中文字, 与 CedarRidge qualify-for/landscape/ 的密度一致。
