# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> Tailored for Software Engineer roles at a financial-services company. Derived from `resume.md` by keeping Summary Variant C, the software/backend skill lines, and Bullet Sets 3 and 5. The finance experience leads with the AI emphasis variant because the target reader cares about that domain and the modeling work.

---

## 1. Summary

M.S. Computer Science student building distributed backend services. Shipped a Go-based feed ranking microservice serving 4,500 requests per second at a consumer social product, plus a real-time fraud-detection model and streaming pipeline at a derivatives broker. Strong with Go, Python, gRPC, Kubernetes, and AWS.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Distributed Systems, Database Systems, Software Engineering, Machine Learning.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

Languages: Go, Python, SQL
Backend: gRPC, Protocol Buffers, FastAPI, REST
Cloud and Infra: AWS (EKS, ECS, Lambda, S3, RDS, MSK, ElastiCache), Docker, Kubernetes, Helm, Terraform
Data: PostgreSQL, Redis, Apache Kafka, Apache Spark Structured Streaming
ML (working knowledge): XGBoost, scikit-learn, model monitoring
Observability: Prometheus, Grafana, OpenTelemetry, structured logging

---

## 4. Experience

### Pulse Social, Feed Ranking Microservice

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).

### Forge Trading, Real-Time Fraud Detection Pipeline

Data Analytics and ML Contractor. 2025-12 to 2026-01.

- Trained an XGBoost fraud-detection model on 4M+ labeled transactions across 30 engineered features (rolling velocity, geo-anomaly, merchant-risk score) and tuned with Bayesian search via Optuna; achieved 0.94 AUC on a held-out month.
- Shadow-deployed model against the live production transaction stream for two weeks; matched the rules-engine recall while cutting false-positive flag rate by 35% and catching $1.2M in confirmed fraud the rules engine missed.
- Implemented model-drift monitoring with PSI tracking on the top 10 features; surfaced two real covariate shifts during the engagement, enabling retraining ahead of any production degradation.
- Designed the feature scoring path so XGBoost inference ran under 200ms end to end against a Redis-backed feature store with six 30-second sliding-window aggregates.
