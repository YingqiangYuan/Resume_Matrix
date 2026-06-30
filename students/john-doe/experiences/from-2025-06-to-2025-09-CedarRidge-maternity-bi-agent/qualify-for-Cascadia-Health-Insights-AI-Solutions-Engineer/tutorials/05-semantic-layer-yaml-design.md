# Tutorial 05: Semantic Layer YAML Design

> This is the companion tutorial for POC-05. Once you finish reading you will know why a semantic layer is not the shallow idea of "wrapping SQL in a layer of YAML," and which directions interviewers will probe in deeply on this topic.

## 1. Why a BI Agent Needs a Semantic Layer

Intuitively, a BI Agent looks like this. The user asks a question, the LLM generates SQL, the SQL runs and returns results. Why insert a semantic layer in the middle?

Because the same business question, "the average fare last month," can map to at least 5 reasonable but differently scoped SQL queries. Which time band? Tip included or not? Divided by orders or by passengers? Do canceled orders count? Do empty-vehicle trips count? At any company of meaningful scale, all those definitions have formal answers, scattered across analysts' Notion docs, Slack history, and old report appendices.

If the LLM reinvents those definitions on every turn, the numbers the agent produces will not line up with the company's other reports, and that will tank the tool's credibility overall. The job of the semantic layer is to externalize those definitions into machine-readable YAML so the LLM reads the YAML before generating SQL and produces SQL that matches the company's official definitions.

Cascadia's Insight Assistant invests heavily in this layer because every client is a hospital BI team, and a one-line definition mismatch will get them grilled by a compliance officer. That is also why "codify metric definitions into semantic layer" lands as the second responsibility in the JD.

## 2. Tradeoffs Across the Three Semantic Layer Shapes

The industry has three main semantic layer shapes. Understanding the tradeoffs across them is a frequent interview question.

**Cube shape**: represented by Cube.dev and AtScale. Build an OLAP cube on top of the data warehouse and preaggregate all possible dimension combinations. Pros, queries fly and dimensions can be swapped freely. Cons, cube definitions are complex, cardinality blows up fast, and this does not suit healthcare data where dimensions are many and long-tail sparse.

**Metric layer shape**: represented by dbt MetricFlow and Cube's newer mode. Only define metrics (aggregation expressions) and dimensions (grouping axes), do not preaggregate, compile to SQL at query time and run on demand. Pros, flexible, adding a new metric does not require rebuilding a cube. Cons, the compiler has to be robust, and performance becomes unpredictable on complex joins.

**Pure view shape**: build a layer of views directly inside the data warehouse, one view per metric. Pros, extremely simple, every BI tool works out of the box. Cons, metrics are hard to compose, dimension changes force view rewrites, and it does not fit agent usage.

Cascadia uses the **metric layer shape**, because a BI Agent needs exactly "flexibly compose metric plus dimension plus filter." POC-05 also uses this shape. If you get asked "why not cube" in an interview, the answer is that healthcare operations data has too severe a long tail across dimensions (the cross product of every ward, every shift, and every specialty blows up cardinality immediately), so a cube does not pay off.

## 3. Core Fields of the YAML Schema

A minimum viable semantic layer YAML contains three kinds of object, metric, dimension, and source.

**Metric** is an aggregation expression. Every metric needs at least name, expression, type, and source. Example:

```yaml
metrics:
  - name: total_trips
    expression: count(trip_id)
    type: counter
    source: taxi_trips
    description: How many taxi trips occurred within a time period
```

A more elaborate case is the ratio type, with numerator and denominator expressed separately:

```yaml
metrics:
  - name: tip_rate
    type: ratio
    numerator: sum(tip_amount)
    denominator: sum(fare_amount)
    source: taxi_trips
    zero_denominator: null  # return null instead of erroring when the denominator is 0
```

**Dimension** is a grouping axis, similar to a SQL GROUP BY field. Common types are time, categorical, and numeric bucket:

```yaml
dimensions:
  - name: pickup_hour
    expression: extract(hour from tpep_pickup_datetime)
    type: time
    source: taxi_trips
```

**Source** describes the underlying table. Source belongs here so a metric can span tables, not to configure database connections (that is runtime's concern):

```yaml
sources:
  - name: taxi_trips
    table: public.yellow_tripdata
    primary_key: trip_id
```

## 4. Three Common Traps in Design Decisions

**Trap 1, pushing definitional choices into metric names**. Beginners often write two metrics, `total_trips_weekday` and `total_trips_weekend`. The right move is to write only one `total_trips` and turn weekday vs weekend into a filter on a dimension. The former blows up the metric count, the latter preserves composability.

**Trap 2, hard-coded dates inside the YAML**. A metric like `metric: revenue_q3_2025` is dead on arrival. The right move is for the metric to be `revenue` and let the query time filter control the time range.

**Trap 3, dimensions as a flat list**. `dimensions: [pickup_hour, pickup_day, pickup_week, pickup_month]` looks innocent but the agent has no way to know these are different granularities of the same time dimension. The right move is to build a hierarchy:

```yaml
dimensions:
  - name: pickup_time
    type: time
    granularities: [hour, day, week, month, quarter, year]
    base_column: tpep_pickup_datetime
```

## 5. Three Topics an Interviewer Will Dig Into

**Topic 1, how to handle metric definition conflicts**. The classic version is the client's Senior Analyst telling you their definition of `readmission rate` differs from yours. The interviewer is not listening for "I would just change it to theirs." They are listening for "I would add a `definition_source: Hannah Liu 2025-08 review` field in the YAML to make the source of the decision explicit, so anyone who later wants to change the metric can trace why it was defined this way."

**Topic 2, how the semantic layer gets used by the LLM**. Two approaches. (a) Chunk the YAML and feed it into the RAG corpus, so the LLM retrieves it when generating SQL. (b) Serialize the YAML into part of the system prompt. Production typically uses both, POC-05 sticks with (a) (which conveniently provides material for POC-03).

**Topic 3, how far metrics can compose**. Can you write `metric: monthly_revenue_growth = (this_month_revenue - last_month_revenue) / last_month_revenue`? Yes, but it tends to complicate the metric compiler. Cascadia's internal policy is to only allow linear composition of the form metric A op metric B, no nested window functions. Articulating that boundary clearly in an interview scores points.

## 6. Further Reading

- The metric chapter in the dbt MetricFlow documentation is the clearest metric layer spec in the industry right now.
- Cube.dev's data modeling tutorial, focus on the cube vs metric comparison chapter.
- Looker's LookML design documents. LookML is a cube shape, but its discussion of dimension and measure is illuminating for beginners.

End of tutorial. Go back to [POC-05](../pocs/poc-05-semantic-yaml/README.md) and start building.
