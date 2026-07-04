# POC-01：用建一个两表对账查询集来学 SQL（PostgreSQL）

> 严重度：🔴 Core ｜ 估时：15 天 ｜ 对应 tutorial：[../../tutorials/01-sql-reconciliation-cn.md](../../tutorials/01-sql-reconciliation-cn.md)

## 一句话 pitch

用建一组"内部账 vs 托管行账"的对账 SQL 查询，来把 SQL 从零学到能现场写、能讲取舍。

## 支撑的 case 决策

case 决策 1/2/3/6/7：对账逻辑用 SQL 写在 PostgreSQL 上、用 FULL OUTER JOIN 找单侧 break、用窗口函数做 aging、用 SQL 规则打分。这个 POC 就是 case 对账引擎的本体。

## 输入与 setup（全部公开/自建，零付费）

- Docker 起一个本地 PostgreSQL 实例。
- 一个 Python 合成数据生成器：造内部持仓/托管行持仓、内部成交/券商确认、现金两侧，按已知规则注入 break，并把注入记录写进一张"真值台账"。
- 无需任何真实数据或云资源。

## 期望产出物

1. 三套对账 SQL 查询：持仓对账（FULL OUTER JOIN）、现金对账（差额+容差）、交易对账（T+1 过滤 + 分批成交聚合）。
2. 合成数据生成器 + 建表 DDL + 真值台账。
3. 一张持久化 break 台账 + aging 计算查询（窗口函数 + 跨天匹配）。
4. 一份注释文档：每条查询在找哪类 break、为什么用这个 JOIN、NULL 意味着什么。
5. 一份 HackerRank/Codility 中等 SQL 题的限时练习记录。

## 成功标准（可自验，面试官"你做过 X 吗"直接映射)

- [ ] 能现场手写 INNER / LEFT / FULL OUTER JOIN，并说清各自何时用。
- [ ] 能写 `GROUP BY + HAVING` 聚合出每账户风险敞口。
- [ ] 能用窗口函数 `ROW_NUMBER() OVER (PARTITION BY ...)` 给每账户 break 排名。
- [ ] 能用一句 SQL 找出"内部有、托管行没有"的记录（一个 break）。
- [ ] 能说清 WHERE 与 HAVING 的区别、LEFT JOIN 后 NULL 的含义。
- [ ] 能写出 T+1 过滤条件 `WHERE settle_date <= as_of_date` 并解释为什么。
- [ ] 在中等 SQL 题上不查文档、限时做对。

## 教程

见 [../../tutorials/01-sql-reconciliation-cn.md](../../tutorials/01-sql-reconciliation-cn.md)。

## 踩坑 / 衍生

最常见的坑是习惯性用 `INNER JOIN`，把"只有一边有"的记录悄悄过滤掉，而那正是要找的 break。守住 case 决策 2：找单侧存在的 break 必须 FULL OUTER JOIN。边界严格锁在 JOIN/GROUP BY/窗口函数/日期函数/CTE，**不碰索引调优、分区、复制这些 DBA 噪音**（JD 要的是写查询做对账，不是数据库管理员）。衍生：单日对账跑通后，加跨天 aging 台账，直接产出 case 阶段三的核心。
