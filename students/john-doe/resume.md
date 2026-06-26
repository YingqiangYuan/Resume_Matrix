# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> This is the all-in-one master resume. It contains three Summary variants (one per target role family) and five Experience bullet sets (some projects appear twice with different emphases). This file is never submitted directly. Each derived resume (`resume-role-1.md` through `resume-role-4.md`) is produced by deleting irrelevant Summaries, Skills lines, and bullet sets from this file.

---

## 1. Summary

The three variants below describe the same person from three different angles. Each derived resume keeps exactly one.

### Variant A, for AI Engineer roles

M.S. Computer Science student building production AI systems. Hands-on experience designing an LLM-powered clinical summarization service at a healthcare AI startup and an ML-based fraud-detection model at a derivatives broker. Comfortable with prompt engineering, evaluation harnesses, RAG, XGBoost, and end-to-end deployment on AWS.

### Variant B, for Data Engineer roles

M.S. Computer Science student specializing in real-time data infrastructure. Built event-driven EMR ingestion pipelines processing 250K+ clinical records and Kafka-based streams handling 12K events per second across healthcare and fintech engagements. Strong with Python, Spark Structured Streaming, Kafka, Airflow, Snowflake, dbt, and AWS data services.

### Variant C, for Software Engineer roles

M.S. Computer Science student building distributed backend services. Shipped a Go-based feed ranking microservice serving 4,500 requests per second at a consumer social product, plus production AI and data systems across healthcare and fintech internships. Strong with Go, Python, gRPC, Kubernetes, and AWS.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Machine Learning, Distributed Systems, Database Systems, Natural Language Processing.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

The full master list. Each derived resume keeps only the lines relevant to that role family.

Languages: Python, Go, TypeScript, SQL, Java
AI and ML: PyTorch, scikit-learn, XGBoost, LangChain, OpenAI API, Anthropic API, prompt engineering, RAG, model evaluation
Data: Apache Kafka, Apache Spark Structured Streaming, Apache Airflow, dbt, Snowflake, PostgreSQL, Redis, Great Expectations
Cloud and Infra: AWS (ECS, EKS, Lambda, S3, RDS, MSK, ElastiCache), Docker, Kubernetes, Helm, Terraform
Backend: FastAPI, gRPC, Protocol Buffers, REST
Observability: Prometheus, Grafana, OpenTelemetry, structured logging

---

## 4. Experience

Each Experience entry below is one **bullet set**. Some projects appear twice with different emphases, because the same case file (in `experiences/`) can be framed for an AI angle or a Data angle or a Software angle.

### Bullet Set 1, MedSync Health, Clinical Notes Summarization Platform (AI emphasis)

Software Engineer Intern, AI Team. 2025-06 to 2025-09.

- Built an LLM-powered clinical notes summarization service using OpenAI GPT-4 and LangChain, processing 250K+ FHIR-formatted patient records and reducing physician chart review time from 12 to 4 minutes per case across a 60-physician pilot.
- Designed a prompt evaluation harness with eight quality metrics (including factual coverage and citation faithfulness against a 300-note clinician-annotated gold set); iterated on prompts to reach 91% physician satisfaction at week 6.
- Implemented structured output enforced by Pydantic schemas with a retry chain on schema-validation failure; cut malformed-output rate from 11% in the prototype to under 0.5% in production.
- Built a RAG layer over an internal clinical-guideline corpus with k-NN retrieval on OpenAI embeddings stored in pgvector; deployed the full stack as a FastAPI service on AWS ECS Fargate.

### Bullet Set 2, MedSync Health, Clinical Notes Summarization Platform (Data emphasis)

Software Engineer Intern, AI Team. 2025-06 to 2025-09.

- Built event-driven ingestion pipeline for FHIR-format EMR notes on AWS Lambda and SQS, processing 250K+ historical records during backfill and 4,000 new notes per day in production with zero data-loss incidents.
- Designed a 14-entity PostgreSQL schema for normalized clinical data with denormalized views for downstream analytics, supporting sub-second p95 query times for 60 physicians.
- Implemented HIPAA-compliant PHI de-identification combining Microsoft Presidio with custom regex rules from a clinical informatics consultant; achieved 99.4% precision on internal test set.
- Wired up Prometheus, Grafana, and OpenTelemetry observability across ingestion and serving layers; cut on-call triage time by 40% on data-quality incidents.

### Bullet Set 3, Forge Trading, Real-Time Fraud Detection Pipeline (AI emphasis)

Data Engineering Contractor. 2025-12 to 2026-01.

- Trained an XGBoost fraud-detection model on 4M+ labeled transactions across 30 engineered features (rolling velocity, geo-anomaly, merchant-risk score) and tuned with Bayesian search via Optuna; achieved 0.94 AUC on a held-out month.
- Shadow-deployed model against the live production transaction stream for two weeks; matched the rules-engine recall while cutting false-positive flag rate by 35% and catching $1.2M in confirmed fraud the rules engine missed.
- Implemented model-drift monitoring with PSI tracking on the top 10 features; surfaced two real covariate shifts during the engagement, enabling retraining ahead of any production degradation.
- Designed the feature scoring path so XGBoost inference ran under 200ms end to end against a Redis-backed feature store with six 30-second sliding-window aggregates.

### Bullet Set 4, Forge Trading, Real-Time Fraud Detection Pipeline (Data emphasis)

Data Engineering Contractor. 2025-12 to 2026-01.

- Built a real-time data pipeline on Kafka (AWS MSK) and Spark Structured Streaming processing 12,000 events per second at peak, up from a 5,000 events-per-second ceiling on the prior system.
- Engineered exactly-once delivery semantics using Kafka transactions and idempotent event-id-keyed sinks; brought data loss from a baseline of two events per week down to zero over a six-week production run.
- Designed a Snowflake feature store with six sliding-window aggregates refreshed every 30 seconds and pushed into Redis for low-latency reads; documented every feature as a dbt model for analyst self-service.
- Built Airflow DAGs for nightly retraining gated by 18 Great Expectations data contracts; blocked two bad retraining runs caused by an upstream merchant-feed schema change.

### Bullet Set 5, Pulse Social, Feed Ranking Microservice (Software emphasis)

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).

---

## 5. Notes

This master resume references three case files in `experiences/`. Each case file is the raw narrative of one project, and each bullet set above is one possible framing of that case for one role family.

Bullet Sets 1 and 2 are both drawn from [from-2025-06-to-2025-09-clinical-notes-summarization-platform.md](./experiences/from-2025-06-to-2025-09-clinical-notes-summarization-platform.md).

Bullet Sets 3 and 4 are both drawn from [from-2025-12-to-2026-01-realtime-fraud-detection-pipeline.md](./experiences/from-2025-12-to-2026-01-realtime-fraud-detection-pipeline.md).

Bullet Set 5 is drawn from [from-2026-06-to-2026-09-feed-ranking-microservice.md](./experiences/from-2026-06-to-2026-09-feed-ranking-microservice.md).
