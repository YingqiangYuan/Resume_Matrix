# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> Tailored for Software Engineer roles at a financial-services company. Derived from `resume.md` by keeping Summary Variant C, the software/backend skill lines, and Bullet Sets 4 and 6. The finance experience leads with the AI emphasis variant because the target reader cares about that domain and the agent / compliance work.

---

## 1. Summary

M.S. Computer Science student building distributed backend services. Shipped a Go-based feed ranking microservice serving 4,500 requests per second at a consumer social product, plus the Phase 0 foundations of a production natural-language BI Agent on AWS Bedrock AgentCore at a fraud-detection SaaS. Strong with Go, Python, gRPC, Kubernetes, AWS CDK, and AWS.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Distributed Systems, Database Systems, Software Engineering, Machine Learning.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

Languages: Go, Python, SQL
Backend: gRPC, Protocol Buffers, FastAPI, REST, Next.js
Cloud and Infra: AWS CDK, AWS (Bedrock, AgentCore, EKS, ECS, Lambda, S3, RDS, ElastiCache, CloudWatch, Secrets Manager), Docker, Kubernetes, Helm
Data: Snowflake, PostgreSQL, Redis
Observability: CloudWatch, Prometheus, Grafana, OpenTelemetry, structured logging
AI and ML (working knowledge): Strand Agents, Bedrock Knowledge Base, OpenAI API, Claude on Bedrock, RAG, evaluation harnesses

---

## 4. Experience

### Pulse Social, Feed Ranking Microservice

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).

### NovaRisk AI, Fraud and AML BI Agent

Analytics Engineering Contractor. 2025-12 to 2026-01.

- Bootstrapped NovaRisk's internal natural-language BI Agent for the Fraud Ops and Compliance teams in Python using Strand Agents on AWS Bedrock AgentCore Runtime; built the Phase 0 foundations (Snowflake Query tool, Knowledge Retrieval tool, AgentCore App + MCP dual endpoint) that the FY26 production rollout was built on.
- Stood up the Amazon Bedrock Knowledge Base seeded with the senior fraud analyst's metric glossary, FinCEN and OFAC regulatory definitions, and the 20-template SQL corpus as the source of truth; the first 5 P0 queries reached 90% SQL accuracy on the engagement-end golden set.
- Wrote a multi-provider LLM abstraction (OpenAI and Gemini for demo, Claude on Bedrock for production) selectable by environment variable; the abstraction shipped to the production rollout unchanged.
- Designed the audit trail schema capturing every NL query, generated SQL, KB snippet ID, and result row count (excluding sensitive values) into both CloudWatch and a Snowflake audit table; this schema was the hard requirement that unblocked the project's SOC2 Type II and FinCEN compliance posture.
