# POC-02：用建一份交易生命周期 domain 资料来学结算对账语言

> 严重度：🟡 Important ｜ 估时：5 天 ｜ 对应 tutorial：[../../tutorials/02-trade-lifecycle-domain-cn.md](../../tutorials/02-trade-lifecycle-domain-cn.md)

## 一句话 pitch

用整理交易生命周期术语 + 把 T+1 落地成 POC-01 的过滤逻辑，来学 post-trade operations 的行业语言。

## 支撑的 case 决策

case 决策 4：交易对账做 T+1 结算日过滤。domain 理解直接变成 `WHERE settle_date <= as_of_date` 这句 SQL。

## 输入与 setup

- gap 分析引用的 `03-role` 调研 + 公开的 post-trade 科普材料。
- 与 POC-01 的合成数据设计交织：把理解落到 `settle_date` 字段和过滤逻辑上。

## 期望产出物

1. 一页交易生命周期术语表：execution / clearance / settlement / break / T+1 / custodian / fail to deliver。
2. 一张结算时间线图（T 日成交 → T+1 交收）。
3. 一个 break 分类举例（内部记 100 股、托管行记 90 股 = 一个 position break）。

## 成功标准（可自验）

- [ ] 能 60 秒讲清一笔交易从成交到交收的链路。
- [ ] 能定义 break 并举一个具体例子。
- [ ] 能解释 T+1 为什么压紧对账时间窗、为什么逼着流程自动化。
- [ ] 能说清 agency-side（基金行政）与 principal（Virtu 做市）对账语境的差异。
- [ ] 备好"为什么是 ops 不是 quant"的真诚答案（行为面必问）。

## 教程

见 [../../tutorials/02-trade-lifecycle-domain-cn.md](../../tutorials/02-trade-lifecycle-domain-cn.md)。

## 踩坑 / 衍生

坑是学成"背名词"而不能落地。每个术语必须接到 POC-01 的一段 SQL 或一个字段上，否则面试一追"这在你项目里长什么样"就空。JD 明写 "No finance experience is necessary"，所以这不是 OA 闸，而是 onsite 和行为面的可信度。衍生：把术语表写成英文，顺带喂 POC-07 的 writing sample。
