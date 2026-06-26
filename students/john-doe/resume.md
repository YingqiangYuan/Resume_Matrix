# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> This is the all-in-one master resume. It contains three Summary variants (one per target role family) and six Experience bullet sets (each project except the first appears twice with different emphases). Bullet Set 1 is the original "thin" framing of the same Cedar Ridge internship that Bullet Sets 2 and 3 elevate, kept here for teaching contrast only. This file is never submitted directly. Each derived resume (`resume-role-1.md` through `resume-role-4.md`) is produced by deleting irrelevant Summaries, Skills lines, and bullet sets from this file; Set 1 never appears in any derived resume.

---

## 1. Summary

The three variants below describe the same person from three different angles. Each derived resume keeps exactly one.

### Variant A, for AI Engineer roles

M.S. Computer Science student building production AI systems. Hands-on experience designing two natural-language BI Agents on AWS Bedrock AgentCore, covering maternity-ward operations at a 6-hospital healthcare network and fraud-ops self-serve analytics at a B2B fraud-detection SaaS. Comfortable with Strand Agents, Bedrock Knowledge Base, semantic layers over Snowflake, multi-provider LLM abstraction, prompt evaluation harnesses, and end-to-end AWS CDK deployment.

### Variant B, for Data Analyst roles

M.S. Computer Science student focused on Data Analytics for AI and risk products. Codified metric definitions into YAML semantic layers over Snowflake at a maternity-care network and a fraud-detection SaaS; designed UAT studies, retrospective gap analyses, and CloudWatch-driven adoption dashboards that moved pilot self-serve rates from 0 to 41% and resolved auditor-flagged definition inconsistencies. Strong with SQL, Python (pandas, scipy.stats, statsmodels), Snowflake, Looker, and Tableau.

### Variant C, for Software Engineer roles

M.S. Computer Science student building distributed backend services. Shipped a Go-based feed ranking microservice serving 4,500 requests per second at a consumer social product, plus production AI and data systems on AWS Bedrock AgentCore across healthcare and fintech engagements. Strong with Go, Python, gRPC, Kubernetes, AWS CDK, and AWS.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Machine Learning, Distributed Systems, Database Systems, Natural Language Processing.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

The full master list. Each derived resume keeps only the lines relevant to that role family.

Languages: Python, Go, TypeScript, SQL, Java
AI and ML: Strand Agents, AWS Bedrock AgentCore, AWS Bedrock Knowledge Base, OpenAI API, Anthropic API, Google Gemini API, LangChain, RAG, prompt engineering, evaluation harnesses
Data Analytics: SQL, Python (pandas, scipy.stats, statsmodels), Tableau, Looker, semantic layers, A/B testing, retrospective studies, KPI reporting
Data Infrastructure: Snowflake, dbt, PostgreSQL, Redis
Cloud and Infra: AWS CDK, AWS (Bedrock, AgentCore, ECS, EKS, Lambda, S3, RDS, CloudWatch, Secrets Manager), Docker, Kubernetes, Helm
Backend: FastAPI, gRPC, Protocol Buffers, REST, Next.js
Observability: CloudWatch, Prometheus, Grafana, OpenTelemetry, structured logging

---

## 4. Experience

Each Experience entry below is one **bullet set**. Some projects appear twice with different emphases, because the same case file (in `experiences/`) can be framed for an AI angle or a Data Analytics angle or a Software angle. Bullet Set 1 is the original "thin" framing of the same Cedar Ridge internship that Sets 2 and 3 elevate, included here only to make the elevation contrast visible. It does not appear in any derived resume.

### Bullet Set 1, Cedar Ridge Women's Health, Maternity SQL Reporting (BEFORE elevation, teaching artifact)

Analytics Intern. 2025-06 to 2025-09.

- Wrote 15 SQL queries against the maternity ward's Snowflake database to answer ad-hoc reporting requests from the clinical analytics team.
- Helped a senior analyst put together weekly Excel summaries on bed availability, staff scheduling, and postpartum length of stay for the OB ward manager.
- Reviewed each query with the senior analyst, revised based on her feedback on metric definitions and query performance.

### Bullet Set 2, Cedar Ridge Women's Health, MaternaPulse BI Agent (AI emphasis)

Full-stack AI/Data Intern. 2025-06 to 2025-09.

- Built MaternaPulse, an internal natural-language BI Agent for the maternity wards of a 6-hospital healthcare network, in Python using Strand Agents on AWS Bedrock AgentCore Runtime; let Charge Nurses across 15 OB wards ask plain-English shift-handover, bed-availability, and high-risk-patient questions and get hybrid text + table + chart answers in under 6 seconds (p95).
- Designed the Knowledge Retrieval tool over Amazon Bedrock Knowledge Base on a ~50-doc internal OB glossary and metric corpus, so every answer cited canonical metric definitions; hit 100% citation rate on the 30-conversation golden set and held hallucination rate under 5%.
- Implemented a multi-provider LLM abstraction (OpenAI / Gemini for demo, Claude on Bedrock for production) selectable by environment variable; the same agent code ran across three providers with zero conditional branches, shipped unchanged to the CMIO demo.
- Wrote an evaluation harness against the senior clinical analyst's 20 SQL templates (the OB ward's internally-validated metric definitions) covering SQL accuracy, answer accuracy, hallucination rate, and p95 latency; reached 92% SQL accuracy and 87% answer accuracy on the golden set, clearing both the project's 90%/85% targets and the CMIO go/no-go gate.

### Bullet Set 3, Cedar Ridge Women's Health, MaternaPulse BI Agent (Data Analytics emphasis)

Full-stack AI/Data Intern. 2025-06 to 2025-09.

- Codified the maternity ward's operational metric definitions (active census, postpartum LOS, room availability with cleaning state, high-risk BP trend) into a YAML semantic layer over Snowflake; replaced an undocumented mix of Slack-screenshot SQL and Notion notes that had produced inconsistent definitions across prior reports.
- Designed and ran a UAT study with 2 Charge Nurses on 30 golden conversations covering Shift Handover, Room Availability, and High-Risk Alerts; tracked thumbs-up rate weekly, surfaced 8 wording and information-density adjustments, and lifted UAT acceptance from 64% to 92% over three iterations.
- Built an adoption dashboard on the agent's CloudWatch query log and Snowflake audit table tracking pilot ward self-serve query rate, p95 latency, and ad-hoc ticket displacement; the pilot ward's self-serve query rate climbed from 0 to 41% in 8 weeks, beating the project's 40% target.
- Authored the HIPAA + TJC-compliant audit trail spec covering every NL query, generated SQL, KB snippet ID, and result row count (excluding PHI values) into both CloudWatch (90-day retention) and a Snowflake audit table (7-year retention); cleared the Q3 internal compliance dry-run on the first pass.

### Bullet Set 4, NovaRisk AI, Fraud and AML BI Agent (AI emphasis)

Analytics Engineering Contractor. 2025-12 to 2026-01.

- Bootstrapped NovaRisk's internal natural-language BI Agent for the Fraud Ops and Compliance teams in Python using Strand Agents on AWS Bedrock AgentCore Runtime; built the Phase 0 foundations (Snowflake Query tool, Knowledge Retrieval tool, AgentCore App + MCP dual endpoint) that the FY26 production rollout was built on.
- Stood up the Amazon Bedrock Knowledge Base seeded with the senior fraud analyst's metric glossary, FinCEN and OFAC regulatory definitions, and the 20-template SQL corpus as the source of truth; the first 5 P0 queries reached 90% SQL accuracy on the engagement-end golden set.
- Wrote a multi-provider LLM abstraction (OpenAI and Gemini for demo, Claude on Bedrock for production) selectable by environment variable; the abstraction shipped to the production rollout unchanged.
- Designed the audit trail schema capturing every NL query, generated SQL, KB snippet ID, and result row count (excluding sensitive values) into both CloudWatch and a Snowflake audit table; this schema was the hard requirement that unblocked the project's SOC2 Type II and FinCEN compliance posture.

### Bullet Set 5, NovaRisk AI, Fraud and AML BI Agent (Data Analytics emphasis)

Analytics Engineering Contractor. 2025-12 to 2026-01.

- Codified NovaRisk's fraud-ops metric definitions (false-positive rate, SAR conversion rate, structuring pattern hit rate, model-vs-rule precision) into a YAML semantic layer over Snowflake; resolved 3 conflicting definitions of false-positive rate that an external auditor had flagged across different quarterly reports.
- Analyzed 6 months of the senior fraud analyst's ad-hoc query backlog (~40 requests per week, ~22 analyst-hours per week) in SQL on Snowflake; categorized into 8 business-problem clusters and 20 representative golden SQL templates that became the project's evaluation gold set.
- Designed a retrospective gap analysis comparing the analyst's hand-written SQL against the semantic-layer-derived SQL across 20 templates; surfaced 4 cases where the hand-written version had drifted from the canonical definition, and brought all 20 back to a single source of truth.
- Built the evaluation harness measuring SQL accuracy, answer accuracy (with ±2% tolerance), hallucination rate, and citation coverage on the 20 golden queries; the framework was reused unchanged in subsequent phases to drive the agent past the 85% SQL accuracy and 80% answer accuracy gates needed for SOC2 readiness.

### Bullet Set 6, Pulse Social, Feed Ranking Microservice (Software emphasis)

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).

---

## 5. Notes

This master resume references three case files (one of which is a folder of project design docs) in `experiences/`. Each case file is the raw narrative or design package of one project, and each bullet set above is one possible framing of that case for one role family.

Bullet Set 1 is drawn from [from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting.md](./experiences/from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting.md), which is the "before elevation" framing of the Cedar Ridge internship.

Bullet Sets 2 and 3 are both drawn from [from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/](./experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/), which is the elevated project design package for the same internship.

Bullet Sets 4 and 5 are both drawn from [from-2025-12-to-2026-01-NovaRisk-fraud-aml-bi-agent/](./experiences/from-2025-12-to-2026-01-NovaRisk-fraud-aml-bi-agent/).

Bullet Set 6 is drawn from [from-2026-06-to-2026-09-feed-ranking-microservice.md](./experiences/from-2026-06-to-2026-09-feed-ranking-microservice.md).
