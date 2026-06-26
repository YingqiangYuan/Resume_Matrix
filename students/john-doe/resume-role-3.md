# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> Tailored for Software Engineer roles at a healthcare company. Derived from `resume.md` by keeping Summary Variant C, the software/backend skill lines, and Bullet Sets 1 and 5. The healthcare-AI experience leads with the AI emphasis variant because the target reader cares about that domain.

---

## 1. Summary

M.S. Computer Science student building distributed backend services. Shipped a Go-based feed ranking microservice serving 4,500 requests per second at a consumer social product, plus a production LLM-powered clinical summarization service at a healthcare AI startup. Strong with Go, Python, gRPC, Kubernetes, and AWS.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Distributed Systems, Database Systems, Software Engineering.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

Languages: Go, Python, TypeScript, SQL
Backend: gRPC, Protocol Buffers, FastAPI, REST
Cloud and Infra: AWS (ECS, EKS, Lambda, S3, RDS, ElastiCache), Docker, Kubernetes, Helm, Terraform
Data: PostgreSQL, Redis, Apache Kafka
Observability: Prometheus, Grafana, OpenTelemetry, structured logging
AI and ML (working knowledge): LangChain, OpenAI API, RAG

---

## 4. Experience

### Pulse Social, Feed Ranking Microservice

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).

### MedSync Health, Clinical Notes Summarization Platform

Software Engineer Intern, AI Team. 2025-06 to 2025-09.

- Built an LLM-powered clinical notes summarization service using OpenAI GPT-4 and LangChain, processing 250K+ FHIR-formatted patient records and reducing physician chart review time from 12 to 4 minutes per case across a 60-physician pilot.
- Designed a prompt evaluation harness with eight quality metrics (including factual coverage and citation faithfulness against a 300-note clinician-annotated gold set); iterated on prompts to reach 91% physician satisfaction at week 6.
- Implemented structured output enforced by Pydantic schemas with a retry chain on schema-validation failure; cut malformed-output rate from 11% in the prototype to under 0.5% in production.
- Built a RAG layer over an internal clinical-guideline corpus with k-NN retrieval on OpenAI embeddings stored in pgvector; deployed the full stack as a FastAPI service on AWS ECS Fargate.
