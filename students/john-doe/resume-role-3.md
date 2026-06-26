# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> Tailored for Software Engineer roles at a healthcare company. Derived from `resume.md` by keeping Summary Variant C, the software/backend skill lines, and Bullet Sets 2 and 6. The healthcare-AI experience leads with the AI emphasis variant because the target reader cares about that domain.

---

## 1. Summary

M.S. Computer Science student building distributed backend services. Shipped a Go-based feed ranking microservice serving 4,500 requests per second at a consumer social product, plus a production natural-language BI Agent on AWS Bedrock AgentCore at a 6-hospital maternity-care network. Strong with Go, Python, gRPC, Kubernetes, AWS CDK, and AWS.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Distributed Systems, Database Systems, Software Engineering.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

Languages: Go, Python, TypeScript, SQL
Backend: gRPC, Protocol Buffers, FastAPI, REST, Next.js
Cloud and Infra: AWS CDK, AWS (Bedrock, AgentCore, ECS, EKS, Lambda, S3, RDS, ElastiCache, CloudWatch, Secrets Manager), Docker, Kubernetes, Helm
Data: Snowflake, PostgreSQL, Redis
Observability: CloudWatch, Prometheus, Grafana, OpenTelemetry, structured logging
AI and ML (working knowledge): Strand Agents, Bedrock Knowledge Base, OpenAI API, RAG, evaluation harnesses

---

## 4. Experience

### Pulse Social, Feed Ranking Microservice

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).

### Cedar Ridge Women's Health, MaternaPulse BI Agent

Full-stack AI/Data Intern. 2025-06 to 2025-09.

- Built MaternaPulse, an internal natural-language BI Agent for the maternity wards of a 6-hospital healthcare network, in Python using Strand Agents on AWS Bedrock AgentCore Runtime; let Charge Nurses across 15 OB wards ask plain-English shift-handover, bed-availability, and high-risk-patient questions and get hybrid text + table + chart answers in under 6 seconds (p95).
- Designed the Knowledge Retrieval tool over Amazon Bedrock Knowledge Base on a ~50-doc internal OB glossary and metric corpus, so every answer cited canonical metric definitions; hit 100% citation rate on the 30-conversation golden set and held hallucination rate under 5%.
- Implemented a multi-provider LLM abstraction (OpenAI / Gemini for demo, Claude on Bedrock for production) selectable by environment variable; the same agent code ran across three providers with zero conditional branches, shipped unchanged to the CMIO demo.
- Wrote an evaluation harness against the senior clinical analyst's 20 SQL templates (the OB ward's internally-validated metric definitions) covering SQL accuracy, answer accuracy, hallucination rate, and p95 latency; reached 92% SQL accuracy and 87% answer accuracy on the golden set, clearing both the project's 90%/85% targets and the CMIO go/no-go gate.
