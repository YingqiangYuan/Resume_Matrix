# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> This is the all-in-one master resume. It contains three Summary variants (one per target role family) and six Experience bullet sets (each project except the first appears twice with different emphases). Bullet Set 1 is the original "thin" framing of the same Cedar Ridge internship that Bullet Sets 2 and 3 elevate, kept here for teaching contrast only. This file is never submitted directly. Each derived resume (`resume-role-1.md` through `resume-role-4.md`) is produced by deleting irrelevant Summaries, Skills lines, and bullet sets from this file; Set 1 never appears in any derived resume.

---

## 1. Summary

The three variants below describe the same person from three different angles. Each derived resume keeps exactly one.

### Variant A, for AI Engineer roles

M.S. Computer Science student building production AI systems. Hands-on experience designing two natural-language BI Agents on AWS Bedrock AgentCore, covering maternity-ward operations at a 6-hospital healthcare network and fraud-ops self-serve analytics at a B2B fraud-detection SaaS. Comfortable with Strand Agents, Bedrock Knowledge Base, semantic layers over Snowflake, multi-provider LLM abstraction, prompt evaluation harnesses, and end-to-end AWS CDK deployment.

> **Rationale for this Variant** (internal commentary; strip from any submitted resume)
>
> **Identity choice**: chose "M.S. Computer Science student building production AI systems" over generic "Software Engineer". Two shipped Bedrock AgentCore agents make "production AI" defensible. Avoided "AI Engineer" without qualifier (over-claims years), and avoided "Aspiring AI Engineer" (signals junior anxiety). "student building production AI" lands between humility and substance.
>
> **Verbs**: "building" (present continuous, ongoing) and "designing" in the Strength clause. Both match what the case files actually show: you designed the architectures and shipped them, you didn't just implement someone else's spec.
>
> **Key nouns**: "natural-language BI Agents" packages LLM plus database query plus business-domain user plus non-engineer consumer in 4 words, sharper than "AI tool" or "chatbot". "AWS Bedrock AgentCore" calls out the AWS-native stack hiring managers searching for Bedrock will grep on. "semantic layers over Snowflake" invites the "vs dbt?" follow-up, which is a good interview signal.
>
> **Tech-list ordering**: front-loaded Strand Agents and Bedrock Knowledge Base because they appear in 3 of 4 AI-direction Bullet Sets and are increasingly searched-for by AI-focused hiring managers. AWS CDK at the end as a deploy-side signal.
>
> **Anchored on Bullet Sets**: 2 (Cedar Ridge MaternaPulse AI emphasis), 4 (NovaRisk Fraud and AML BI Agent AI emphasis). When deriving an AI Engineer resume from this master, keep these two Bullet Sets, drop the Data Analytics variants (Sets 3 and 5), and consider keeping Set 6 (Pulse Social) if the target also values backend chops.
>
> **Notes**: if you later derive a role-specific resume from this master, delete this rationale block.

### Variant B, for Data Analyst roles

M.S. Computer Science student focused on Data Analytics for AI and risk products. Codified metric definitions into YAML semantic layers over Snowflake at a maternity-care network and a fraud-detection SaaS; designed UAT studies, retrospective gap analyses, and CloudWatch-driven adoption dashboards that moved pilot self-serve rates from 0 to 41% and resolved auditor-flagged definition inconsistencies. Strong with SQL, Python (pandas, scipy.stats, statsmodels), Snowflake, Looker, and Tableau.

> **Rationale for this Variant** (internal commentary; strip from any submitted resume)
>
> **Identity choice**: "M.S. Computer Science student focused on Data Analytics for AI and risk products" intentionally narrows from generic "Data Analyst". The "for AI and risk products" tail pre-positions you for healthcare AI ops, fraud-ops analytics, and similar verticals where domain-specific Data Analyst roles live. Avoided pure "Data Analyst" because that loses your domain edge; avoided "Analytics Engineer" because it implies more pipeline tenure than you can defend.
>
> **Verbs**: "Codified" and "designed" both signal ownership of artifacts that persisted past your tenure (YAML layers, UAT protocols, dashboards). Avoided "analyzed" (too passive, applies to almost any analyst) and "supported" (implies you were a helper, not an owner).
>
> **Key nouns**: "YAML semantic layer" is a known industry term that signals you understand metric-definition belongs above the database. "UAT study" is the right HCI/research term, sharper than "user research" or "user interviews". "retrospective gap analysis" packages a specific analytic activity that fraud-ops and compliance teams will recognize.
>
> **Quantitative claims**: "0 to 41%" pilot self-serve rate is anchored on the Cedar Ridge MaternaPulse 8-week rollout (Bullet Set 3); industry baseline for "new internal analyst tool adoption in 8 weeks" is typically single-digit, so 41% is high but defensible because the alternative (waiting on a senior analyst's ticket queue) was painful.
>
> **Tech-list ordering**: SQL first (most universally relevant for Data Analyst), Python second with the specific stat libraries (signals "I do real analysis, not just queries"), Snowflake third (locked in by the case), Looker and Tableau last (you have exposure but not deep ownership). Refused to add Tableau Server, dbt, or other tools you have not actually used.
>
> **Anchored on Bullet Sets**: 3 (Cedar Ridge MaternaPulse Data Analytics emphasis), 5 (NovaRisk Fraud and AML BI Agent Data Analytics emphasis). When deriving a Data Analyst resume from this master, keep Sets 3 and 5, drop the AI variants (Sets 2 and 4), drop Set 6 (Pulse Social is software-flavored).
>
> **Notes**: if you later derive a role-specific resume from this master, delete this rationale block.

### Variant C, for Software Engineer roles

M.S. Computer Science student building distributed backend services. Shipped a Go-based feed ranking microservice serving 4,500 requests per second at a consumer social product, plus production AI and data systems on AWS Bedrock AgentCore across healthcare and fintech engagements. Strong with Go, Python, gRPC, Kubernetes, AWS CDK, and AWS.

> **Rationale for this Variant** (internal commentary; strip from any submitted resume)
>
> **Identity choice**: "building distributed backend services" is sharper than "Software Engineer" or "Backend Engineer". The Pulse Social feed ranker is genuinely a distributed service (Go, gRPC, Kubernetes, HPA). Avoided pure "Backend Engineer" (too generic), avoided "Full-Stack Engineer" (you do not have frontend evidence in the case files), avoided "Distributed Systems Engineer" (over-claims, that title usually carries 5 plus years of system-design tenure).
>
> **Verbs**: "Shipped" is the right verb for production deployment to real users (Pulse Social rolled to 25% of users). Stronger than "built" or "developed". Avoided "owned end-to-end" because the case shows you owned the service but the upstream data-science models were not yours.
>
> **Key nouns**: "feed ranking microservice" packages high-RPS plus low-latency plus ML-adjacent in 3 words, exactly the kind of system a backend hiring manager at a consumer product wants to see. "4,500 requests per second" is a concrete RPS number that puts you in the "real production traffic" tier (vs "I deployed a side project that had 10 users"). "p99 latency under 60ms" is the metric Pulse-like consumer-feed engineers actually look at.
>
> **Quantitative claims**: 4,500 RPS and p99 under 60ms both come straight from Bullet Set 6, both measurable from Prometheus / k6 load tests. Industry baseline for a feed ranking microservice at a mid-size social product is in the 1,000 to 10,000 RPS range, so 4,500 is in the upper-middle band and defensible.
>
> **Tech-list ordering**: Go first because Pulse Social's JD specifically required Go and you delivered in Go. Python second because it covers the AI work (Bedrock agents are Python). gRPC and Kubernetes flag the distributed-system specifics, AWS CDK and AWS the cloud-deploy story.
>
> **Anchored on Bullet Sets**: 6 (Pulse Social Feed Ranking Microservice, Software emphasis) as the primary anchor. Sets 2 and 4 (AI emphasis Bullet Sets for Cedar Ridge and NovaRisk) can ride along because the AI projects also demonstrate backend skills (AWS deployment, multi-provider abstraction, audit-trail schema). When deriving a Software Engineer resume for a non-healthcare non-fintech target (a generic backend role), keep Set 6 plus one of (Set 2 or Set 4) for breadth, drop the Data Analytics Bullet Sets entirely.
>
> **Notes**: if you later derive a role-specific resume from this master, delete this rationale block.

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

> **Rationale for this Bullet Set** (teaching artifact; this Set never appears in a derived resume)
>
> This Set is the BEFORE-elevation framing of the same internship that Sets 2 and 3 elevate. It is intentionally weak so the contrast with the elevated versions is visible side by side. Three things this Set demonstrates by absence:
>
> - **No HR-readable picture**: Bullet 1 opens with "Wrote 15 SQL queries", which gives the reader a count of artifacts but no picture of why those queries existed or who benefited.
> - **No technical depth signal**: every bullet is at the same surface level (wrote / helped / reviewed). No mention of metric definitions, no schema thinking, no impact on downstream decisions.
> - **No ownership**: "Helped" and "Reviewed each query with the senior analyst" both signal you were a hands-on assistant, not a person who shipped anything.
>
> Read this Set, then read Set 2 or Set 3 immediately below to feel the lift. Same internship, same 12 weeks, same Snowflake. The difference is how much of the work you understood and how you frame it. The elevation work (07-elevate-existing-project teaches it) is what closes that gap.

### Bullet Set 2, Cedar Ridge Women's Health, MaternaPulse BI Agent (AI emphasis)

Full-stack AI/Data Intern. 2025-06 to 2025-09.

- Built MaternaPulse, an internal natural-language BI Agent for the maternity wards of a 6-hospital healthcare network, in Python using Strand Agents on AWS Bedrock AgentCore Runtime; let Charge Nurses across 15 OB wards ask plain-English shift-handover, bed-availability, and high-risk-patient questions and get hybrid text + table + chart answers in under 6 seconds (p95).
- Designed the Knowledge Retrieval tool over Amazon Bedrock Knowledge Base on a ~50-doc internal OB glossary and metric corpus, so every answer cited canonical metric definitions; hit 100% citation rate on the 30-conversation golden set and held hallucination rate under 5%.
- Implemented a multi-provider LLM abstraction (OpenAI / Gemini for demo, Claude on Bedrock for production) selectable by environment variable; the same agent code ran across three providers with zero conditional branches, shipped unchanged to the CMIO demo.
- Wrote an evaluation harness against the senior clinical analyst's 20 SQL templates (the OB ward's internally-validated metric definitions) covering SQL accuracy, answer accuracy, hallucination rate, and p95 latency; reached 92% SQL accuracy and 87% answer accuracy on the golden set, clearing both the project's 90%/85% targets and the CMIO go/no-go gate.

> **Rationale for this Bullet Set** (internal commentary; strip from any submitted resume)
>
> **Structure**: 4 bullets in the B1 / B2 / B3 / B4 progressive shape from 09-write-bullets §3. B1 is the HR-readable picture, B2 is a non-trivial design decision, B3 is the hardest specific sub-problem, B4 is the evaluation rigor that says "I treated this like a real product, not a demo".
>
> **Verbs**:
> - B1 "Built": hands-on builder verb appropriate for intern. Considered "Architected" (over-claims at this level) and "Developed" (less complete-system feel). "Built" matches the case's evidence that you shipped the whole agent.
> - B2 "Designed": signals ownership of the architectural choice (Knowledge Retrieval tool over Bedrock KB was your call), not just implementation.
> - B3 "Implemented": describes execution of a specific pattern (multi-provider abstraction) inside the larger system. Calibrated lower than Designed because the abstraction pattern is well-known.
> - B4 "Wrote": low-key verb, but the depth comes from what you wrote (an evaluation harness, not "some tests"). Keeps the bullet from over-claiming.
>
> **Key nouns**:
> - "natural-language BI Agent" in B1 packages LLM plus database query plus business-domain user in 4 words. Sharper than "AI tool" or "data assistant".
> - "AWS Bedrock AgentCore Runtime" in B1 names the specific runtime, not just "AWS". Hiring managers searching for AgentCore experience will grep on this.
> - "Knowledge Retrieval tool" plus "Knowledge Base" in B2 packages RAG plus tool-calling plus citation grounding in 2 phrases; invites the "vs Pinecone? vs in-context retrieval?" follow-up.
> - "multi-provider LLM abstraction" plus "selectable by environment variable" plus "zero conditional branches" in B3 is interview-friendly: it tells an experienced engineer exactly what the mechanism is, and the "zero conditional branches" detail is the kind of specific that signals competence.
> - "evaluation harness" plus "SQL accuracy, answer accuracy, hallucination rate, p95 latency" in B4 names the actual eval dimensions, which is the bar for "I shipped a real product" in AI engineering.
>
> **Quantitative claims**:
> - B1 "under 6 seconds (p95)" latency. Defensible: Bedrock AgentCore latency over Snowflake on warm data sits in the 3 to 8 second range in production observability docs; p95 under 6s is in-band.
> - B2 "~50-doc internal OB glossary", "100% citation rate", "hallucination rate under 5%". 50-doc corpus size is a deliberate "small enough to ground, big enough to test retrieval" pick (from the case). 100% citation comes from the eval harness; hallucination under 5% is at the upper-quality end of published RAG benchmarks but defensible because the corpus is domain-narrow.
> - B4 "92% SQL accuracy, 87% answer accuracy". Both come straight from the 20-query golden set defined in the case. Industry baseline for natural-language BI agents on domain-narrow evals runs 70 to 90%; you sit at the upper end because the corpus is curated and the schema is constrained.
>
> **Notes**: if you derive an AI-direction resume from this master, keep this Set, pair with Variant A. If derived for a non-healthcare AI target, consider trimming the "Charge Nurses across 15 OB wards" phrase in B1 to just "across 15 production wards".

### Bullet Set 3, Cedar Ridge Women's Health, MaternaPulse BI Agent (Data Analytics emphasis)

Full-stack AI/Data Intern. 2025-06 to 2025-09.

- Codified the maternity ward's operational metric definitions (active census, postpartum LOS, room availability with cleaning state, high-risk BP trend) into a YAML semantic layer over Snowflake; replaced an undocumented mix of Slack-screenshot SQL and Notion notes that had produced inconsistent definitions across prior reports.
- Designed and ran a UAT study with 2 Charge Nurses on 30 golden conversations covering Shift Handover, Room Availability, and High-Risk Alerts; tracked thumbs-up rate weekly, surfaced 8 wording and information-density adjustments, and lifted UAT acceptance from 64% to 92% over three iterations.
- Built an adoption dashboard on the agent's CloudWatch query log and Snowflake audit table tracking pilot ward self-serve query rate, p95 latency, and ad-hoc ticket displacement; the pilot ward's self-serve query rate climbed from 0 to 41% in 8 weeks, beating the project's 40% target.
- Authored the HIPAA + TJC-compliant audit trail spec covering every NL query, generated SQL, KB snippet ID, and result row count (excluding PHI values) into both CloudWatch (90-day retention) and a Snowflake audit table (7-year retention); cleared the Q3 internal compliance dry-run on the first pass.

> **Rationale for this Bullet Set** (internal commentary; strip from any submitted resume)
>
> **Structure**: 4 bullets reframing the same MaternaPulse internship from the Data Analytics angle. B1 picture is now about the semantic layer (the Data Analytics artifact), B2 is about UAT methodology (a Data Analytics activity), B3 is about adoption measurement (the Data Analytics impact story), B4 is about audit compliance (the rigor signal Data Analytics hiring managers care about).
>
> **Verbs**:
> - B1 "Codified": packages "wrote down formally, in a versionable artifact" in one word. Stronger than "Defined" (which sounds informal) or "Documented" (which sounds passive). The case actually shows you wrote the YAML files and got them into source control.
> - B2 "Designed and ran": pairs design ownership with execution ownership. The case shows you both wrote the UAT protocol and led the sessions.
> - B3 "Built": appropriate for the adoption dashboard (you wrote the queries plus put it on CloudWatch).
> - B4 "Authored": fits for a spec / policy document (audit trail spec), stronger than "wrote" because it implies you owned the content, not just the prose.
>
> **Key nouns**:
> - "YAML semantic layer over Snowflake" in B1 is the industry-recognized noun, sharper than "config files" or "metric definitions in YAML".
> - "UAT study" in B2 is the right HCI/research term. Sharper than "user interviews" (sounds like product research, not analytics).
> - "adoption dashboard" plus "CloudWatch query log" plus "Snowflake audit table" in B3 names three specific data sources, signals you knew what to instrument.
> - "HIPAA + TJC-compliant audit trail spec" in B4 packages two specific regulatory regimes; HIPAA is recognizable to anyone, TJC (The Joint Commission) is the hospital accreditation body that signals you understood the healthcare-specific compliance stack.
>
> **Quantitative claims**:
> - B2 "lifted UAT acceptance from 64% to 92% over three iterations" comes from the UAT log (case has the per-iteration thumbs-up rate). Industry baseline for "first-pass UAT acceptance on an internal NL BI tool" is in the 50 to 70% range, so 64% start to 92% finish is in-band and shows the iteration worked.
> - B3 "self-serve query rate climbed from 0 to 41% in 8 weeks" comes from the CloudWatch query log + Snowflake audit table comparison. 0 to 41% is high for 8 weeks but defensible because the alternative (analyst ticket queue) was painful enough that adoption was sticky.
> - B4 "Q3 internal compliance dry-run on the first pass" is binary (pass / fail) so does not need a baseline. The detail that matters is "first pass", signaling you got the spec right initially rather than after multiple revisions.
>
> **Notes**: if you derive a Data Analyst resume from this master, keep this Set, pair with Variant B. Same internship as Set 2 but a completely different framing; the case file (`case-cn.md` in qualify-for-Cascadia-...) supports both.

### Bullet Set 4, NovaRisk AI, Fraud and AML BI Agent (AI emphasis)

Analytics Engineering Contractor. 2025-12 to 2026-01.

- Bootstrapped NovaRisk's internal natural-language BI Agent for the Fraud Ops and Compliance teams in Python using Strand Agents on AWS Bedrock AgentCore Runtime; built the Phase 0 foundations (Snowflake Query tool, Knowledge Retrieval tool, AgentCore App + MCP dual endpoint) that the FY26 production rollout was built on.
- Stood up the Amazon Bedrock Knowledge Base seeded with the senior fraud analyst's metric glossary, FinCEN and OFAC regulatory definitions, and the 20-template SQL corpus as the source of truth; the first 5 P0 queries reached 90% SQL accuracy on the engagement-end golden set.
- Wrote a multi-provider LLM abstraction (OpenAI and Gemini for demo, Claude on Bedrock for production) selectable by environment variable; the abstraction shipped to the production rollout unchanged.
- Designed the audit trail schema capturing every NL query, generated SQL, KB snippet ID, and result row count (excluding sensitive values) into both CloudWatch and a Snowflake audit table; this schema was the hard requirement that unblocked the project's SOC2 Type II and FinCEN compliance posture.

> **Rationale for this Bullet Set** (internal commentary; strip from any submitted resume)
>
> **Structure**: 4 bullets, B1 / B2 / B3 / B4 progression. B1 frames a "Phase 0 foundation" picture (intentional, you were a 6-week contractor not a year-long owner). B2 surfaces the design decision around the source of truth (KB seeded with metric glossary plus regulatory definitions). B3 picks up the multi-provider abstraction pattern from Set 2 (you reused it, the reuse is the story). B4 is the audit trail schema that gated compliance, the hard-impact bullet.
>
> **Verbs**:
> - B1 "Bootstrapped": exactly the right verb for a 6-week Phase 0 contractor engagement. Stronger than "Helped start" but weaker than "Built end-to-end" (which would over-claim for a 6-week stint). Signals "I laid the foundations that later phases built on".
> - B2 "Stood up": engineering verb for "deployed and configured to a useful state". Stronger than "Set up" because it implies operational readiness.
> - B3 "Wrote": low-key verb because the abstraction pattern is the same one from Set 2, just reused; no over-claiming.
> - B4 "Designed": ownership of the schema design, which matches the case (you wrote the schema, the SOC2 audit signed off on it).
>
> **Key nouns**:
> - "Phase 0 foundations" in B1 is honest about the scope (6 weeks, not full delivery) while making clear that the foundations were on the critical path of the FY26 rollout. Better than calling it "an MVP" (sounds amateurish) or "the production agent" (over-claims).
> - "metric glossary, FinCEN and OFAC regulatory definitions, and the 20-template SQL corpus as the source of truth" in B2 names three concrete content types that went into the KB. Hiring managers in fraud-ops will recognize FinCEN (Financial Crimes Enforcement Network) and OFAC (Office of Foreign Assets Control) immediately.
> - "SOC2 Type II and FinCEN compliance posture" in B4 is the killer phrase. SOC2 Type II is the enterprise compliance bar for B2B SaaS; FinCEN is the federal regulator for fraud reporting. Either alone is meaningful, both together signals "this person worked on a real regulated product".
>
> **Quantitative claims**:
> - B2 "90% SQL accuracy on the engagement-end golden set" comes from the 5 P0 queries evaluated at end of the 6-week contract. Sample size is small (5) which is acknowledged by "engagement-end" qualifier; defensible because the bar was P0 only.
> - B4 schema "unblocked SOC2 Type II and FinCEN compliance posture" is binary (compliance team accepted or rejected the schema). No baseline needed.
>
> **Notes**: if you derive an AI Engineer resume targeting fintech or compliance-heavy verticals (Stripe, Plaid, Marqeta, anything fraud-ops), keep this Set alongside Set 2. The healthcare AI emphasis (Set 2) plus fintech AI emphasis (this Set) together signal "production AI in regulated industries", which is a sharper positioning than "production AI" alone.

### Bullet Set 5, NovaRisk AI, Fraud and AML BI Agent (Data Analytics emphasis)

Analytics Engineering Contractor. 2025-12 to 2026-01.

- Codified NovaRisk's fraud-ops metric definitions (false-positive rate, SAR conversion rate, structuring pattern hit rate, model-vs-rule precision) into a YAML semantic layer over Snowflake; resolved 3 conflicting definitions of false-positive rate that an external auditor had flagged across different quarterly reports.
- Analyzed 6 months of the senior fraud analyst's ad-hoc query backlog (~40 requests per week, ~22 analyst-hours per week) in SQL on Snowflake; categorized into 8 business-problem clusters and 20 representative golden SQL templates that became the project's evaluation gold set.
- Designed a retrospective gap analysis comparing the analyst's hand-written SQL against the semantic-layer-derived SQL across 20 templates; surfaced 4 cases where the hand-written version had drifted from the canonical definition, and brought all 20 back to a single source of truth.
- Built the evaluation harness measuring SQL accuracy, answer accuracy (with ±2% tolerance), hallucination rate, and citation coverage on the 20 golden queries; the framework was reused unchanged in subsequent phases to drive the agent past the 85% SQL accuracy and 80% answer accuracy gates needed for SOC2 readiness.

> **Rationale for this Bullet Set** (internal commentary; strip from any submitted resume)
>
> **Structure**: 4 bullets, B1 / B2 / B3 / B4 progression mirroring Set 3 but for the fraud-ops case. B1 picture is the semantic layer (the Data Analytics artifact); B2 is the historical SQL backlog analysis (a Data Analytics activity that demonstrates analytical chops); B3 is the retrospective gap analysis (the harder Data Analytics work that shows you understood drift, not just queries); B4 is the eval framework (the rigor signal).
>
> **Verbs**:
> - B1 "Codified": same verb as Set 3 B1 for consistency across the two Data Analytics Bullet Sets. Signals you took informal definitions and made them versionable.
> - B2 "Analyzed": the right verb for a 6-month backlog review. Avoided "investigated" (too forensic) and "studied" (too academic).
> - B3 "Designed": ownership of the methodology (retrospective gap analysis is a specific analytic activity you chose), not just execution.
> - B4 "Built": appropriate for the eval framework as an artifact.
>
> **Key nouns**:
> - "fraud-ops metric definitions" plus "false-positive rate, SAR conversion rate, structuring pattern hit rate, model-vs-rule precision" in B1 names four specific fraud-ops metrics. Reader who works in fraud-ops will immediately recognize these as the metrics that actually matter; reader who does not will at least see the depth.
> - "6 months of the senior fraud analyst's ad-hoc query backlog" in B2 quantifies the data scope you analyzed. "~40 requests per week, ~22 analyst-hours per week" gives the reader a sense of "this is a real bottleneck", not "I analyzed 5 tickets".
> - "retrospective gap analysis comparing the analyst's hand-written SQL against the semantic-layer-derived SQL" in B3 is the killer methodology phrase. It signals you understood that "the analyst's queries and the semantic layer's queries can drift" and you measured the drift.
> - "SQL accuracy, answer accuracy (with ±2% tolerance), hallucination rate, and citation coverage" in B4 lists the four eval dimensions explicitly; the "±2% tolerance" detail is the kind of specific that signals "I designed a real eval, not a yes/no spot check".
>
> **Quantitative claims**:
> - B1 "3 conflicting definitions of false-positive rate that an external auditor had flagged" comes from the case (this is what triggered the Phase 0 engagement). Auditor framing makes it externally validated, not your subjective judgment.
> - B2 "~40 requests per week, ~22 analyst-hours per week" comes from the 6-month backlog analysis. ~40 and ~22 are presented as estimates (the ~ matters); precise to the right level for a 6-month sample.
> - B2 "8 business-problem clusters and 20 representative golden SQL templates" comes from the case design. The "8 clusters distilled to 20 templates" ratio implies real consolidation work; defensible because the case has the clustering criteria.
> - B3 "4 cases where the hand-written version had drifted from the canonical definition" is the bug-finding result, specific count out of 20 templates.
> - B4 "85% SQL accuracy and 80% answer accuracy gates needed for SOC2 readiness" specifies the bar; the harness reuse "unchanged in subsequent phases" is impact-by-longevity.
>
> **Notes**: if you derive a Data Analyst resume targeting fintech, fraud-ops, or compliance-heavy analytics roles, keep this Set alongside Set 3. Pair with Variant B. The Cedar Ridge healthcare Data Analyst story (Set 3) plus the NovaRisk fraud-ops Data Analyst story (this Set) together signal "Data Analyst who has worked across two regulated verticals", which is a sharper positioning than "Data Analyst with healthcare experience" alone.

### Bullet Set 6, Pulse Social, Feed Ranking Microservice (Software emphasis)

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).

> **Rationale for this Bullet Set** (internal commentary; strip from any submitted resume)
>
> **Structure**: 4 bullets, B1 / B2 / B3 / B4 progression. B1 picture is the high-RPS low-latency feed ranker with a clean "before vs after" comparison. B2 is the architecture decision (3-stage composable pipeline) that earned the data-science team's independence. B3 is the API contract design plus deployment story. B4 is the rollout rigor (load test + shadow + gradual A/B) that signals "I shipped to real users, not a demo".
>
> **Verbs**:
> - B1 "Built": correct verb for shipping a service. Avoided "Architected" (over-claims for an intern), avoided "Owned end-to-end" (the upstream ranking models were data-science team's).
> - B2 "Designed": ownership of the architecture decision (3-stage composable pipeline was your choice, the case has the decision-replay).
> - B3 "Designed" plus "deployed": pairs API ownership with deployment ownership.
> - B4 "Wrote" plus "ran" plus "shipped": three small actions that together describe the rollout discipline. Stronger than "rolled out" alone because the reader sees the rollout was earned, not just executed.
>
> **Key nouns**:
> - "Go-based feed ranking microservice" in B1 packages language, problem domain, and architectural shape in 4 words. "feed ranking" alone signals consumer-product backend (the relevant subfield).
> - "4,500 requests per second at peak with p99 latency under 60ms end to end" in B1 names the two metrics consumer-feed backend hiring managers will check first.
> - "Python monolith path sitting at 220ms p99" in B1 gives the before-state, making the "60ms p99" number land as 4x improvement, not as an absolute claim out of nowhere.
> - "three-stage composable architecture" plus "candidate generation, ranking, post-processing" in B2 names the standard consumer-feed-system pattern. Anyone in the field will immediately recognize the shape.
> - "minimal model interface" plus "enabled the data-science team to ship two new ranking models during the internship without backend involvement" in B2 packages the API design's payoff (decoupling) in two phrases. "Without backend involvement" is the killer phrase, it signals you understood your job was to get out of the data-science team's way.
> - "gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck)" in B3 names the actual RPC contract, demonstrates you understood the surface area.
> - "k6 load-test suite replaying two weeks of production traffic at 1.5x peak" plus "two weeks of shadow deployment before any user rollout" in B4 names a specific load-test framework plus a specific rollout-safety practice. Either alone is good; together they signal "I shipped like a senior engineer would".
>
> **Quantitative claims**:
> - B1 "4,500 requests per second at peak" and "p99 under 60ms" both from Prometheus and k6 measurements in the case. Industry baseline for a feed ranker at a mid-size consumer social (5 to 10M MAU) is in the 1k to 10k RPS range, so 4,500 is mid-band and defensible.
> - B1 "220ms p99" for the prior Python monolith path comes from the case's pre-state description; defensible because the case has the measurement window.
> - B2 "two new ranking models during the internship without backend involvement" is the decoupling payoff metric. Verifiable from git history or sprint logs.
> - B4 "+6% session length lift in the A/B test (p<0.01)" comes from the rollout's A/B telemetry. 6% lift in session length is a meaningful product impact (consumer-product A/B tests usually celebrate 1 to 3% lifts), p<0.01 makes it defensible statistically.
>
> **Notes**: this Set is the anchor for Variant C (Software Engineer). Can ride along in AI-direction resumes (paired with Variant A) when the target also values backend chops, because the AI projects need backend ownership too.

---

## 5. Notes

This master resume references three case files (one of which is a folder of project design docs) in `experiences/`. Each case file is the raw narrative or design package of one project, and each bullet set above is one possible framing of that case for one role family.

Bullet Set 1 is drawn from [from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting.md](./experiences/from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting.md), which is the "before elevation" framing of the Cedar Ridge internship.

Bullet Sets 2 and 3 are both drawn from [from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/](./experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/), which is the elevated project design package for the same internship.

Bullet Sets 4 and 5 are both drawn from [from-2025-12-to-2026-01-NovaRisk-fraud-aml-bi-agent/](./experiences/from-2025-12-to-2026-01-NovaRisk-fraud-aml-bi-agent/).

Bullet Set 6 is drawn from [executed-case.md](./experiences/from-2026-04-to-2026-09-pulse-social-feed-ranker/executed-case.md).
