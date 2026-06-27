# POC-05: 用 Semantic Layer YAML 描述 NYC Taxi 数据集

> 这是 gap-fill plan 里 9 个 POC 中的第五个。对应 Gap：🟡 Semantic layer YAML 设计。对应教程：[../../tutorials/05-semantic-layer-yaml-design-cn.md](../../tutorials/05-semantic-layer-yaml-design-cn.md)。

## 1. 这个 POC 在练什么

练习把"业务问题"翻译成"YAML 写的语义层 schema"的能力。语义层是 BI Agent 和数据仓库之间的口径地图：用户问"上个月的平均车费"，agent 不直接生成 SQL，而是先去语义层找叫 `average_fare` 的 metric 看它怎么定义，再据此生成 SQL。Cascadia 的 Insight Assistant 是同样的架构，所以这个能力直接对得上 JD 里"codify metric definitions into semantic layer YAML"那条 responsibility。

John 在 CedarRidge 写过 15 条 SQL，对 metric 口径有一定直觉，但从来没系统化地把口径外化成 YAML。这个 POC 就是把那种直觉显式化。

## 2. 输入与起点

- 公开数据集：NYC TLC Yellow Taxi Trip Records 的一个月样本（约 300 万行 CSV，可从 NYC Open Data 下载）。装载到本地 PostgreSQL，建一张 `taxi_trips` 表。
- 一份从客户视角写的 10 条业务问题清单（自己写）。例子：每天平均订单数、各时段平均车费、周末 vs 平日的小费率、热门上车区域 Top 5、车程超过 1 小时的订单占比、等等。
- Python 3.12 + PostgreSQL 16 + 一个最小化的 semantic-to-SQL 编译器（自己写 200 行，不引入像 dbt MetricFlow 这种重量级框架）。

## 3. 期望产出

- 一份 `metrics.yaml`，定义 5 到 7 个 metric（计数、平均、分位数、比率各覆盖一种）和 3 到 5 个 dimension（时间、空间、支付方式）。
- 一个 `compile.py`，输入 metric 名 + dimension + filter，输出可执行的 SQL。
- 一份验证脚本：10 条业务问题里至少 8 条能用语义层组合出来，跑出的 SQL 结果和手写参考 SQL 一致（精度差异 < 0.1%）。
- 一份 1 页的设计决策回放：为什么选 metric layer 形态而不是 cube 形态？为什么 dimension 用 hierarchy 而不是 flat？

## 4. 验收标准

- YAML 文件本身能通过自定义的 schema 校验（用 Pydantic 或 jsonschema）。
- 至少有一个 metric 是 ratio 类型（带分子分母），用来证明语义层能表达复合定义。
- 10 条业务问题里 ≥ 8 条用语义层可组合表达。
- 任何 metric 的 SQL 生成结果都不超过 30 行（如果超过说明 YAML 写得太粗，逻辑没有合理下沉）。

## 5. 跟 Cascadia JD 怎么对得上

JD 里 "Partner with each client's senior clinical analyst to codify their internal metric definitions into the semantic layer. Resolve definitional conflicts before they become production confusion"。面试里这条会被深挖："如果客户的 Senior Analyst 告诉你 `readmission rate` 的定义跟你设计的不一样，你怎么处理？"

POC-05 走完后 John 可以举例说：在 NYC Taxi 这套语义层里，他遇到过类似情况，`peak_hour_fare` 这个 metric 到底用乘客上车时间还是下车时间界定？他在 YAML 里加了 `time_basis: pickup_time` 字段把这个决策显式化，让任何下游消费者都能看到。这种处理冲突的具体动作是 hiring manager 想听的。

## 6. 时间估算

- Day 1：下载数据装载到 PostgreSQL，跑通 10 条参考 SQL，确认数据干净。
- Day 2-3：设计 YAML schema、定义 5-7 个 metric 和 3-5 个 dimension。
- Day 4：写 `compile.py`，用最简单的字符串拼接出 SQL，跑通 5 条问题。
- Day 5：把剩下的问题打通，写设计决策回放，做最终一遍清理。

总计 5 天，每天 1.5 小时，共 7.5 小时。是 9 个 POC 里最省时间的，因为 John 有 CedarRidge SQL 报表经验作为底座。

## 7. 跟其他 POC 的连接

- 这个 POC 的 YAML 会被 POC-03（RAG + eval harness）的 generation prompt 用作 grounding。具体做法：把 YAML 的 metric 定义 chunk 化后塞进 RAG 的 corpus，让 agent 在生成 SQL 时引用 metric 名而不是凭空写聚合。
- 这个 POC 完成后写的 1 页设计决策回放可以直接复用为 POC-08（客户向写作）的样本之一。

## 8. 已知坑点（提前预警）

- NYC TLC 数据集里 `tpep_pickup_datetime` 字段在 2009-2014 早期数据里有时区不一致的问题，预处理时统一到 UTC。
- 不要试图把 `holiday` 这种需要外部 calendar 的概念塞进 YAML，单独建一张 `dim_calendar` 表更干净。
- ratio 类型 metric（分子/分母）容易出现"分母为 0 不报错"的 bug，YAML schema 校验时加一条强制要求显式声明 `zero_denominator: error | null | ignore` 的处理策略。
