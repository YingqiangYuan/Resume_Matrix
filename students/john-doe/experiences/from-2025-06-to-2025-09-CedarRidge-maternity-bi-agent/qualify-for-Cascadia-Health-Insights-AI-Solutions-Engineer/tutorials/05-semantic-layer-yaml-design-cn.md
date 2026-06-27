# Tutorial 05: Semantic Layer YAML Design

> 这是 POC-05 的配套教程。读完这份你会知道为什么 semantic layer 不是"在 SQL 外面包一层 YAML"那么肤浅，以及面试里这个话题会被往哪几个方向深挖。

## 1. 为什么 BI Agent 需要语义层

直觉上，BI Agent 看起来应该是：用户问问题、LLM 生成 SQL、跑 SQL 出结果。中间为什么要插一层 semantic layer？

因为同一个业务问题，"上个月的平均车费"，可以对应至少 5 种合理但口径不同的 SQL：算的是哪个时段？包不包含小费？除以的是订单数还是乘客数？取消单算不算？空车订单算不算？任何一家有规模的公司里，这些口径都会有正式定义，散落在分析师的 Notion 文档、Slack 历史记录、过去的报告附录里。

如果 LLM 每次都重新发明这套口径，agent 给出的数字就跟公司内部其他报表对不齐，进而把整个工具的可信度搞垮。语义层的作用是把这些口径外化成可机读的 YAML，让 LLM 在生成 SQL 之前先去读 YAML 里的定义，按公司定的口径生成 SQL。

Cascadia 的 Insight Assistant 在这一层投入很大，因为客户都是医院 BI 团队，口径错一行就要被合规官追问。这也是 JD 里 "codify metric definitions into semantic layer" 排在第二条 responsibility 的原因。

## 2. 三种 semantic layer 形态的取舍

业界 semantic layer 主要有三种形态。理解这三种的取舍是面试常考题。

**Cube 形态**：以 Cube.dev 和 AtScale 为代表。在数据仓库之上建一层 OLAP cube，预聚合所有可能的维度组合。优点：查询飞快，dimension 可任意切换。缺点：cube 定义复杂，cardinality 一高就爆炸，不适合医疗数据这种维度多、长尾稀疏的场景。

**Metric layer 形态**：以 dbt MetricFlow 和 Cube 的 newer mode 为代表。只定义 metric（聚合表达式）和 dimension（分组维度），不预聚合，每次查询时编译成 SQL 实时跑。优点：灵活，新增 metric 不需要重建 cube。缺点：编译器要写得健壮，遇到复杂 join 时性能不可控。

**纯 view 形态**：直接在数据仓库里建一层 view，每个 metric 一个 view。优点：极简，所有 BI 工具开箱即用。缺点：metric 之间组合困难，dimension 变换要重写 view，不适合给 agent 用。

Cascadia 用的是 **metric layer 形态**，因为 BI Agent 需要的就是"灵活组合 metric + dimension + filter"的能力。POC-05 也用这种形态。面试时如果被问"为什么不用 cube"，答案是医疗运营数据的 dimension 长尾太严重（每个 ward + 每个 shift + 每个 specialty 的乘积一上来就爆 cardinality），cube 不划算。

## 3. YAML schema 的核心字段

一个最小可用的语义层 YAML 包含三类对象：metric、dimension、source。

**Metric** 是聚合表达式。每个 metric 至少要有 name、expression、type、source。例子：

```yaml
metrics:
  - name: total_trips
    expression: count(trip_id)
    type: counter
    source: taxi_trips
    description: 一段时间内有多少次出租车出行
```

复杂一点的是 ratio 类型，分子分母分别表达：

```yaml
metrics:
  - name: tip_rate
    type: ratio
    numerator: sum(tip_amount)
    denominator: sum(fare_amount)
    source: taxi_trips
    zero_denominator: null  # 分母为 0 时返回 null 不报错
```

**Dimension** 是分组维度，类似 SQL 的 GROUP BY 字段。常见的有 time / categorical / numeric bucket 三类：

```yaml
dimensions:
  - name: pickup_hour
    expression: extract(hour from tpep_pickup_datetime)
    type: time
    source: taxi_trips
```

**Source** 描述底层表。这里写 source 是为了让一个 metric 能跨表，不是为了配置数据库连接（那是 runtime 关心的事）：

```yaml
sources:
  - name: taxi_trips
    table: public.yellow_tripdata
    primary_key: trip_id
```

## 4. 设计决策的 3 个常见陷阱

**陷阱 1：把口径选择推给 metric 名字**。新手常写 `total_trips_weekday` 和 `total_trips_weekend` 两个 metric。正确做法是只写一个 `total_trips`，把 weekday / weekend 做成 dimension 上的 filter。前者会让 metric 数量爆炸，后者保持组合性。

**陷阱 2：在 YAML 里写 hard-coded 日期**。`metric: revenue_q3_2025` 这种 metric 一上来就死。正确做法是 metric 是 `revenue`，时间范围用 query 时的 filter 控制。

**陷阱 3：dimension 用 flat 列表**。`dimensions: [pickup_hour, pickup_day, pickup_week, pickup_month]` 看着没问题，但 agent 不知道这是同一个时间维度的不同 granularity。正确做法是建 hierarchy：

```yaml
dimensions:
  - name: pickup_time
    type: time
    granularities: [hour, day, week, month, quarter, year]
    base_column: tpep_pickup_datetime
```

## 5. 面试会被深挖的 3 个话题

**话题 1：metric 口径冲突怎么处理**。最经典的情况是客户的 Senior Analyst 告诉你 `readmission rate` 的定义跟你设计的不一样。面试官想听的不是"我就改成他的"，而是"我会在 YAML 里加 `definition_source: Hannah Liu 2025-08 review` 字段，把决策来源显式化，未来谁要改这个 metric 都能查到为什么是这个定义"。

**话题 2：semantic layer 怎么给 LLM 用**。两条思路：(a) 把 YAML chunk 化后塞进 RAG corpus，让 LLM 在生成 SQL 时检索；(b) 把 YAML 序列化成 system prompt 的一部分。生产环境通常两者都用，POC-05 用 (a) 就行（顺便给 POC-03 提供素材）。

**话题 3：metric 之间能组合到什么程度**。能不能写 `metric: monthly_revenue_growth = (this_month_revenue - last_month_revenue) / last_month_revenue`？答案是：能，但通常会让 metric 编译器变复杂。Cascadia 内部的处理是只允许 metric A op metric B 这种线性组合，不允许嵌套窗口函数。面试时讲清楚这条边界是加分项。

## 6. 进一步阅读

- dbt MetricFlow 文档里的 metric 章节，是目前业界最清晰的 metric layer 规范
- Cube.dev 的 data modeling 教程，重点看 cube vs metric 的对比章节
- Looker 的 LookML 设计文档，虽然 LookML 是 cube 形态，但里面对 dimension 和 measure 的讨论对新手有启发

教程到此结束。回到 [POC-05](../pocs/poc-05-semantic-yaml/README-cn.md) 开始动手。
