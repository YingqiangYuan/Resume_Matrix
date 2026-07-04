# POC-05：用给对账查询加一个每日调度来学自动化叙事

> 严重度：🟠 Nice-to-have ｜ 估时：4 天 ｜ 对应 tutorial：[../../tutorials/05-automation-scheduling-cn.md](../../tutorials/05-automation-scheduling-cn.md)

## 一句话 pitch

用 cron/APScheduler 把 POC-01 的对账查询包成每日自动跑的任务，来学"把手工流程脚本化"的自动化叙事。

## 支撑的 case 决策

case 决策 6（持久化台账每日刷新）+ case §6.5 调度层。

## 输入与 setup

- 复用 POC-01 的 PostgreSQL 库和对账查询，包一层调度。

## 期望产出物

1. 一个每日调度脚本（cron 或 APScheduler）。
2. 一份运行日志。
3. 一段"从手工找 break 到脚本自动分类标记"的叙事。

## 成功标准（可自验）

- [ ] 调度能在指定时间自动跑全量对账、刷新 break 台账。
- [ ] 能讲清哪些步骤适合自动化、哪些必须留给人判断。
- [ ] 能说清这一层如何对接上游 IT 的数据管道（case §5 协作接点）。

## 教程

见 [../../tutorials/05-automation-scheduling-cn.md](../../tutorials/05-automation-scheduling-cn.md)。

## 踩坑 / 衍生

坑是把调度做成"生产级"（告警/容错/SLA），那超出 case scope（§4 明确 out of scope）。衍生：加一个"跑完发一封汇总邮件"，是很轻的自动化叙事加分。
