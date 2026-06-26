# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> Tailored for Data Engineer roles. Derived from `resume.md` by keeping Summary Variant B, the data/infra skill lines, and Bullet Sets 2, 4, and 5.

---

## 1. Summary

M.S. Computer Science student specializing in real-time data infrastructure. Built event-driven EMR ingestion pipelines processing 250K+ clinical records and Kafka-based streams handling 12K events per second across healthcare and fintech engagements. Strong with Python, Spark Structured Streaming, Kafka, Airflow, Snowflake, dbt, and AWS data services.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Distributed Systems, Database Systems, Machine Learning.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

Languages: Python, Go, SQL
Data: Apache Kafka, Apache Spark Structured Streaming, Apache Airflow, dbt, Snowflake, PostgreSQL, Redis, Great Expectations
Cloud and Infra: AWS (MSK, EKS, Lambda, S3, RDS, ElastiCache), Docker, Kubernetes, Terraform
Backend: FastAPI, gRPC
Observability: Prometheus, Grafana, OpenTelemetry, structured logging

---

## 4. Experience

### MedSync Health, Clinical Notes Summarization Platform

Software Engineer Intern, AI Team. 2025-06 to 2025-09.

- Built event-driven ingestion pipeline for FHIR-format EMR notes on AWS Lambda and SQS, processing 250K+ historical records during backfill and 4,000 new notes per day in production with zero data-loss incidents.
- Designed a 14-entity PostgreSQL schema for normalized clinical data with denormalized views for downstream analytics, supporting sub-second p95 query times for 60 physicians.
- Implemented HIPAA-compliant PHI de-identification combining Microsoft Presidio with custom regex rules from a clinical informatics consultant; achieved 99.4% precision on internal test set.
- Wired up Prometheus, Grafana, and OpenTelemetry observability across ingestion and serving layers; cut on-call triage time by 40% on data-quality incidents.

### Forge Trading, Real-Time Fraud Detection Pipeline

Data Engineering Contractor. 2025-12 to 2026-01.

- Built a real-time data pipeline on Kafka (AWS MSK) and Spark Structured Streaming processing 12,000 events per second at peak, up from a 5,000 events-per-second ceiling on the prior system.
- Engineered exactly-once delivery semantics using Kafka transactions and idempotent event-id-keyed sinks; brought data loss from a baseline of two events per week down to zero over a six-week production run.
- Designed a Snowflake feature store with six sliding-window aggregates refreshed every 30 seconds and pushed into Redis for low-latency reads; documented every feature as a dbt model for analyst self-service.
- Built Airflow DAGs for nightly retraining gated by 18 Great Expectations data contracts; blocked two bad retraining runs caused by an upstream merchant-feed schema change.

### Pulse Social, Feed Ranking Microservice

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).
