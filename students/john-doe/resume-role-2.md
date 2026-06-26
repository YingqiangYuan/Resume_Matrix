# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> Tailored for Data Analyst roles. Derived from `resume.md` by keeping Summary Variant B, the Data Analytics skill lines, and Bullet Sets 3, 5, and 6.

---

## 1. Summary

M.S. Computer Science student focused on Data Analytics for AI and risk products. Codified metric definitions into YAML semantic layers over Snowflake at a maternity-care network and a fraud-detection SaaS; designed UAT studies, retrospective gap analyses, and CloudWatch-driven adoption dashboards that moved pilot self-serve rates from 0 to 41% and resolved auditor-flagged definition inconsistencies. Strong with SQL, Python (pandas, scipy.stats, statsmodels), Snowflake, Looker, and Tableau.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Machine Learning, Database Systems, Statistical Learning.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

Languages: Python, SQL
Data Analytics: SQL, Python (pandas, scipy.stats, statsmodels), Tableau, Looker, semantic layers (YAML), A/B testing, retrospective studies, UAT design, KPI reporting
Data Warehousing: Snowflake, PostgreSQL
ML (working knowledge): evaluation harnesses, model accuracy/answer accuracy/hallucination metrics
Tools: Jupyter, dbt, Git
AI / LLM (working knowledge): Strand Agents, Bedrock Knowledge Base, OpenAI API, RAG

---

## 4. Experience

### Cedar Ridge Women's Health, MaternaPulse BI Agent

Full-stack AI/Data Intern. 2025-06 to 2025-09.

- Codified the maternity ward's operational metric definitions (active census, postpartum LOS, room availability with cleaning state, high-risk BP trend) into a YAML semantic layer over Snowflake; replaced an undocumented mix of Slack-screenshot SQL and Notion notes that had produced inconsistent definitions across prior reports.
- Designed and ran a UAT study with 2 Charge Nurses on 30 golden conversations covering Shift Handover, Room Availability, and High-Risk Alerts; tracked thumbs-up rate weekly, surfaced 8 wording and information-density adjustments, and lifted UAT acceptance from 64% to 92% over three iterations.
- Built an adoption dashboard on the agent's CloudWatch query log and Snowflake audit table tracking pilot ward self-serve query rate, p95 latency, and ad-hoc ticket displacement; the pilot ward's self-serve query rate climbed from 0 to 41% in 8 weeks, beating the project's 40% target.
- Authored the HIPAA + TJC-compliant audit trail spec covering every NL query, generated SQL, KB snippet ID, and result row count (excluding PHI values) into both CloudWatch (90-day retention) and a Snowflake audit table (7-year retention); cleared the Q3 internal compliance dry-run on the first pass.

### NovaRisk AI, Fraud and AML BI Agent

Analytics Engineering Contractor. 2025-12 to 2026-01.

- Codified NovaRisk's fraud-ops metric definitions (false-positive rate, SAR conversion rate, structuring pattern hit rate, model-vs-rule precision) into a YAML semantic layer over Snowflake; resolved 3 conflicting definitions of false-positive rate that an external auditor had flagged across different quarterly reports.
- Analyzed 6 months of the senior fraud analyst's ad-hoc query backlog (~40 requests per week, ~22 analyst-hours per week) in SQL on Snowflake; categorized into 8 business-problem clusters and 20 representative golden SQL templates that became the project's evaluation gold set.
- Designed a retrospective gap analysis comparing the analyst's hand-written SQL against the semantic-layer-derived SQL across 20 templates; surfaced 4 cases where the hand-written version had drifted from the canonical definition, and brought all 20 back to a single source of truth.
- Built the evaluation harness measuring SQL accuracy, answer accuracy (with ±2% tolerance), hallucination rate, and citation coverage on the 20 golden queries; the framework was reused unchanged in subsequent phases to drive the agent past the 85% SQL accuracy and 80% answer accuracy gates needed for SOC2 readiness.

### Pulse Social, Feed Ranking Microservice

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).
