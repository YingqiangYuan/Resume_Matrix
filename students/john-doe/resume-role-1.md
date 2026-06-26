# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> Tailored for AI Engineer roles. Derived from `resume.md` by keeping Summary Variant A, the AI/ML skill lines, and Bullet Sets 1, 3, and 5.

---

## 1. Summary

M.S. Computer Science student building production AI systems. Hands-on experience designing an LLM-powered clinical summarization service at a healthcare AI startup and an ML-based fraud-detection model at a derivatives broker. Comfortable with prompt engineering, evaluation harnesses, RAG, XGBoost, and end-to-end deployment on AWS.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Machine Learning, Natural Language Processing, Distributed Systems.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

Languages: Python, Go, SQL
AI and ML: PyTorch, scikit-learn, XGBoost, LangChain, OpenAI API, Anthropic API, prompt engineering, RAG, model evaluation
Data: Apache Kafka, Apache Spark Structured Streaming, PostgreSQL, Redis
Cloud and Infra: AWS (ECS, EKS, Lambda, S3, RDS), Docker, Kubernetes, Terraform
Backend: FastAPI, gRPC

---

## 4. Experience

### MedSync Health, Clinical Notes Summarization Platform

Software Engineer Intern, AI Team. 2025-06 to 2025-09.

- Built an LLM-powered clinical notes summarization service using OpenAI GPT-4 and LangChain, processing 250K+ FHIR-formatted patient records and reducing physician chart review time from 12 to 4 minutes per case across a 60-physician pilot.
- Designed a prompt evaluation harness with eight quality metrics (including factual coverage and citation faithfulness against a 300-note clinician-annotated gold set); iterated on prompts to reach 91% physician satisfaction at week 6.
- Implemented structured output enforced by Pydantic schemas with a retry chain on schema-validation failure; cut malformed-output rate from 11% in the prototype to under 0.5% in production.
- Built a RAG layer over an internal clinical-guideline corpus with k-NN retrieval on OpenAI embeddings stored in pgvector; deployed the full stack as a FastAPI service on AWS ECS Fargate.

### Forge Trading, Real-Time Fraud Detection Pipeline

Data Engineering Contractor. 2025-12 to 2026-01.

- Trained an XGBoost fraud-detection model on 4M+ labeled transactions across 30 engineered features (rolling velocity, geo-anomaly, merchant-risk score) and tuned with Bayesian search via Optuna; achieved 0.94 AUC on a held-out month.
- Shadow-deployed model against the live production transaction stream for two weeks; matched the rules-engine recall while cutting false-positive flag rate by 35% and catching $1.2M in confirmed fraud the rules engine missed.
- Implemented model-drift monitoring with PSI tracking on the top 10 features; surfaced two real covariate shifts during the engagement, enabling retraining ahead of any production degradation.
- Designed the feature scoring path so XGBoost inference ran under 200ms end to end against a Redis-backed feature store with six 30-second sliding-window aggregates.

### Pulse Social, Feed Ranking Microservice

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).
