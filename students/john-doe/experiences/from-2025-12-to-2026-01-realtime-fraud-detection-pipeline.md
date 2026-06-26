# Real-Time Fraud Detection Pipeline

> Period: 2025-12 to 2026-01. Company: Forge Trading. Role: Data Engineering Contractor. Industry: Financial Services.

## 1. Context

Forge Trading is a mid-size derivatives broker. The compliance team was running a legacy rules engine for transaction fraud detection: roughly 80 hand-coded if-else rules that flagged about 0.8% of all transactions. Of those flags, around 78% turned out to be false positives, which meant the compliance team spent significant time clearing benign trades.

The team wanted to pilot an ML-based system that could supplement (not replace) the rules engine and reduce that false-positive load. The Head of Data brought me on for a six-week winter contract to build the streaming infrastructure and an initial model. I worked alongside two senior data engineers and a quantitative analyst.

The scope was deliberately narrow: build the pipeline, train a baseline model, run it in shadow mode against the production transaction stream, and produce a report on whether the approach was worth scaling.

---

## 2. What I Did

The pipeline ingests every transaction event from Forge's internal order-management system into a Kafka topic on AWS MSK. I built a Spark Structured Streaming job that reads the topic, joins each event against six rolling feature aggregates (transaction velocity over 1, 5, and 30 minutes, average notional, geo-anomaly score, and merchant-risk score), and scores the result with an XGBoost model.

I designed the feature store on Snowflake. Each aggregate is recomputed every 30 seconds by a separate Spark job and pushed back to a low-latency Redis cache that the streaming scorer reads from. I documented all six feature definitions as dbt models so downstream analysts could understand exactly what each number meant.

I engineered exactly-once delivery semantics using Kafka transactions on the producer side and idempotent sinks (event-id-keyed upserts) on the consumer side. This mattered because under stress the legacy pipeline had been losing one to two events per week.

For the model, I worked with the quantitative analyst to engineer roughly 30 features. I trained an XGBoost model on six months of labeled historical transactions (about 4 million records) and tuned it with Bayesian search via Optuna. The chosen model achieved 0.94 AUC on a held-out month.

To monitor drift, I wired up PSI (population stability index) tracking on the top ten features. We had three alert events during the shadow run, and in two of them the analyst confirmed real upstream covariate shift, so we retrained ahead of any degradation in production.

For orchestration I used Airflow, with nightly retraining DAGs gated by Great Expectations data-quality contracts. The 18 contracts blocked two bad retraining runs caused by a schema change in an upstream merchant feed.

---

## 3. Outcomes

The system ran in shadow mode against the live production transaction stream for two weeks. Outcomes (reported in the final write-up to the Head of Compliance):

- Processed 12,000 events per second at peak, with the prior pipeline maxing out around 5,000.
- Achieved 0.94 AUC on holdout. Recall at the operating point matched the rules engine (so no fraud was missed in shadow) while false-positive flag rate dropped by 35%.
- Caught $1.2M in confirmed fraudulent transactions over the two-week shadow window that the rules engine missed.
- Zero data-loss incidents over the six-week production run, down from a baseline of about two per week.
- Two bad retraining runs blocked by Great Expectations contracts.

The team approved scaling to active scoring after I left.

---

## 4. Tech Stack

Python, Apache Kafka (AWS MSK), Apache Spark Structured Streaming, XGBoost, Optuna, Snowflake, dbt, Redis, Apache Airflow, Great Expectations, AWS (MSK, EKS, S3, IAM), Docker, Terraform, Prometheus, Grafana.
