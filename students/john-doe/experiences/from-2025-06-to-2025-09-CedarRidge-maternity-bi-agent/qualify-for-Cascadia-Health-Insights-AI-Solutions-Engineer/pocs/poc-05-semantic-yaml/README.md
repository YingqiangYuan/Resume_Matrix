# POC-05, Describing the NYC Taxi Dataset with a Semantic Layer YAML

> This is the fifth of 9 POCs in the gap-fill plan. It maps to the gap: Important Semantic layer YAML design. Companion tutorial: [../../tutorials/05-semantic-layer-yaml-design.md](../../tutorials/05-semantic-layer-yaml-design.md).

## 1. What This POC Practices

This POC drills the ability to translate a business question into a YAML-encoded semantic layer schema. The semantic layer is the definitions map sitting between the BI Agent and the data warehouse. When a user asks "what was the average fare last month," the agent does not write SQL straight from the question. It first looks up a metric called `average_fare` in the semantic layer to see how it is defined, and then generates SQL on that basis. Cascadia's Insight Assistant uses the same architecture, so this skill maps directly onto the JD responsibility that says "codify metric definitions into semantic layer YAML."

John wrote 15 SQL reports at Cedar Ridge and has reasonable intuition for metric definitions, but he has never systematically externalized those definitions into YAML. This POC is about making that intuition explicit.

## 2. Inputs and Starting Point

- A public dataset, a one month sample of NYC TLC Yellow Taxi Trip Records (around 3 million rows of CSV, downloadable from NYC Open Data). Load it into a local PostgreSQL instance and build a `taxi_trips` table.
- A list of 10 business questions written from the client's point of view (write them yourself). Examples: daily average order count, average fare by time of day, weekend versus weekday tip rate, top 5 pickup zones, share of trips longer than an hour, and so on.
- Python 3.12 plus PostgreSQL 16 plus a minimal semantic-to-SQL compiler (write 200 lines yourself, no heavyweight framework like dbt MetricFlow).

## 3. Expected Deliverables

- A `metrics.yaml` defining 5 to 7 metrics (one each of count, average, percentile, and ratio) plus 3 to 5 dimensions (time, space, payment method).
- A `compile.py` that takes a metric name plus dimensions plus filters and emits runnable SQL.
- A validation script. At least 8 of the 10 business questions can be answered by composing the semantic layer, and the generated SQL produces results that match handwritten reference SQL (within 0.1% precision difference).
- A one page design decision retro. Why pick the metric layer shape over the cube shape? Why use dimension hierarchies instead of flat dimensions?

## 4. Acceptance Criteria

- The YAML file itself passes a custom schema validator (using Pydantic or jsonschema).
- At least one metric is a ratio type (with explicit numerator and denominator), proving the semantic layer can express composite definitions.
- 8 or more of the 10 business questions can be composed from the semantic layer.
- No metric's generated SQL exceeds 30 lines. If it does, the YAML is too coarse and the logic was not pushed down properly.

## 5. How It Maps to the Cascadia JD

The JD says "Partner with each client's senior clinical analyst to codify their internal metric definitions into the semantic layer. Resolve definitional conflicts before they become production confusion." This line gets probed hard in interviews. "If the client's Senior Analyst tells you their definition of `readmission rate` differs from the one you designed, how do you handle it?"

After POC-05, John can give a concrete example. In his NYC Taxi semantic layer he ran into the same shape of issue, should `peak_hour_fare` be bounded by passenger pickup time or dropoff time? He added a `time_basis: pickup_time` field in the YAML so the decision is explicit and visible to any downstream consumer. That kind of concrete conflict-resolution move is exactly what a Hiring Manager wants to hear.

## 6. Time Estimate

- Day 1, download the data, load it into PostgreSQL, run the 10 reference SQL queries, and confirm the data is clean.
- Day 2 to 3, design the YAML schema and define 5 to 7 metrics plus 3 to 5 dimensions.
- Day 4, write `compile.py` using the simplest possible string concatenation, get 5 questions running end to end.
- Day 5, push the remaining questions through, write the design decision retro, and do a final cleanup pass.

Total of 5 days at 1.5 hours per day, 7.5 hours in all. It is the lightest of the 9 POCs by time, because John's Cedar Ridge SQL reporting experience provides the base layer.

## 7. Connections to Other POCs

- The YAML from this POC feeds POC-03's (RAG plus eval harness) generation prompt as grounding. Concretely, chunk the metric definitions from the YAML and stuff them into the RAG corpus so the agent cites a metric by name when generating SQL instead of inventing aggregations from thin air.
- The one page design decision retro produced here can be reused directly as one of the writing samples for POC-08 (client-facing writing).

## 8. Known Pitfalls (heads-up in advance)

- In the NYC TLC dataset, the `tpep_pickup_datetime` field has timezone inconsistencies in the early 2009 to 2014 data. Normalize everything to UTC during preprocessing.
- Do not try to stuff concepts that need an external calendar (like `holiday`) into the YAML. Build a separate `dim_calendar` table instead, it stays cleaner.
- Ratio type metrics (numerator over denominator) frequently produce silent zero-denominator bugs. When validating the YAML schema, enforce an explicit `zero_denominator: error | null | ignore` policy declaration on every ratio metric.
