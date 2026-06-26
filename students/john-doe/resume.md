# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> This is the all-in-one master resume. It contains three Summary variants (one per target role family) and five Experience bullet sets (some projects appear twice with different emphases). This file is never submitted directly. Each derived resume (`resume-role-1.md` through `resume-role-4.md`) is produced by deleting irrelevant Summaries, Skills lines, and bullet sets from this file.

---

## 1. Summary

The three variants below describe the same person from three different angles. Each derived resume keeps exactly one.

### Variant A, for AI Engineer roles

M.S. Computer Science student building production AI systems. Hands-on experience designing an LLM-powered clinical summarization service at a healthcare AI startup and an ML-based fraud-detection model at a derivatives broker. Comfortable with prompt engineering, evaluation harnesses, RAG, XGBoost, and end-to-end deployment on AWS.

### Variant B, for Data Analyst roles

M.S. Computer Science student focused on Data Analytics for AI and risk products. Built a Tableau adoption dashboard and ran a statistically-rigorous A/B test that drove a 14-point acceptance lift at a healthcare AI startup; designed a retrospective study at a derivatives broker that quantified a 35% false-positive reduction with tight confidence intervals and unlocked production scaling. Strong with SQL, Python (pandas, scipy.stats, statsmodels), Tableau, Looker, and Snowflake.

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
Data Analytics: SQL, Python (pandas, scipy.stats, statsmodels), Tableau, Looker, A/B testing, retrospective studies, KPI reporting
Data Infrastructure: Apache Kafka, Apache Spark Structured Streaming, Apache Airflow, dbt, Snowflake, PostgreSQL, Redis
Cloud and Infra: AWS (ECS, EKS, Lambda, S3, RDS, MSK, ElastiCache), Docker, Kubernetes, Helm, Terraform
Backend: FastAPI, gRPC, Protocol Buffers, REST
Observability: Prometheus, Grafana, OpenTelemetry, structured logging

---

## 4. Experience

Each Experience entry below is one **bullet set**. Some projects appear twice with different emphases, because the same case file (in `experiences/`) can be framed for an AI angle or a Data Analytics angle or a Software angle.

### Bullet Set 1, MedSync Health, Clinical Notes Summarization Platform (AI emphasis)

Software Engineer Intern, AI Team. 2025-06 to 2025-09.

- Built an LLM-powered clinical notes summarization service using OpenAI GPT-4 and LangChain, processing 250K+ FHIR-formatted patient records and reducing physician chart review time from 12 to 4 minutes per case across a 60-physician pilot.
- Designed a prompt evaluation harness with eight quality metrics (including factual coverage and citation faithfulness against a 300-note clinician-annotated gold set); iterated on prompts to reach 91% physician satisfaction at week 6.
- Implemented structured output enforced by Pydantic schemas with a retry chain on schema-validation failure; cut malformed-output rate from 11% in the prototype to under 0.5% in production.
- Built a RAG layer over an internal clinical-guideline corpus with k-NN retrieval on OpenAI embeddings stored in pgvector; deployed the full stack as a FastAPI service on AWS ECS Fargate.

### Bullet Set 2, MedSync Health, Clinical Notes Summarization Platform (Data Analytics emphasis)

Software Engineer Intern, AI Team. 2025-06 to 2025-09.

- Built physician adoption dashboard in Tableau pulling from PostgreSQL across the 60-physician pilot; surfaced three distinct adoption cohorts (power users above 80%, moderates 30 to 60%, skeptics under 20%) that drove targeted retraining and lifted week-6 satisfaction from 74% to 91%.
- Designed and ran a 4-week A/B test on summary length using stratified randomization across 30 physicians; chi-squared test in Python's scipy.stats showed the short variant (~100 words) won by 14 percentage points in acceptance rate at p<0.05, and the team shipped it company-wide.
- Wrote SQL exploration queries against the 250K-note corpus to identify three lowest-quality specialties (Pediatrics, OB/GYN, Behavioral Health); prioritized prompt iteration on those categories and brought all three above the 75% quality threshold by week 7.
- Built a one-page weekly clinical-ops KPI report (PHI precision, summary quality, end-to-end latency, satisfaction, adoption cohorts) consumed by the CTO and Head of Clinical Operations in the Monday leadership meeting; replaced a fragmented set of Slack updates as the canonical pilot health signal.

### Bullet Set 3, Forge Trading, Real-Time Fraud Detection Pipeline (AI emphasis)

Data Analytics and ML Contractor. 2025-12 to 2026-01.

- Trained an XGBoost fraud-detection model on 4M+ labeled transactions across 30 engineered features (rolling velocity, geo-anomaly, merchant-risk score) and tuned with Bayesian search via Optuna; achieved 0.94 AUC on a held-out month.
- Shadow-deployed model against the live production transaction stream for two weeks; matched the rules-engine recall while cutting false-positive flag rate by 35% and catching $1.2M in confirmed fraud the rules engine missed.
- Implemented model-drift monitoring with PSI tracking on the top 10 features; surfaced two real covariate shifts during the engagement, enabling retraining ahead of any production degradation.
- Designed the feature scoring path so XGBoost inference ran under 200ms end to end against a Redis-backed feature store with six 30-second sliding-window aggregates.

### Bullet Set 4, Forge Trading, Real-Time Fraud Detection Pipeline (Data Analytics emphasis)

Data Analytics and ML Contractor. 2025-12 to 2026-01.

- Analyzed 6 months of compliance flag history in SQL on Snowflake; identified three false-positive patterns (cross-border weekend travel, holiday retail spikes, miscategorized merchants) accounting for ~80% of analyst clearance hours; delivered a one-page memo with a target list of rules to retire.
- Built a Looker dashboard tracking daily flag volume, false-positive rate, and analyst clearance time by category; refreshed hourly, replaced a 2-day Excel weekly report, became the compliance team's daily standup reference.
- Designed retrospective study comparing the new ML model against the rules engine on 4M historical transactions; computed a 35% false-positive reduction with 95% confidence intervals using Python (statsmodels); presented to the executive review committee, became the main basis for green-lighting active scoring.
- Wrote SQL exploration notebooks for the analyst team showing which signal combinations correlated with confirmed fraud (e.g., geo-anomaly combined with merchant-risk score above thresholds was 12x more likely to be real fraud); incorporated into the manual review playbook.

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
