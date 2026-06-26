# John Doe

Seattle, WA · john.doe@example.com · linkedin.com/in/john-doe · github.com/john-doe

> Tailored for Data Analyst roles. Derived from `resume.md` by keeping Summary Variant B, the Data Analytics skill lines, and Bullet Sets 2, 4, and 5.

---

## 1. Summary

M.S. Computer Science student focused on Data Analytics for AI and risk products. Built a Tableau adoption dashboard and ran a statistically-rigorous A/B test that drove a 14-point acceptance lift at a healthcare AI startup; designed a retrospective study at a derivatives broker that quantified a 35% false-positive reduction with tight confidence intervals and unlocked production scaling. Strong with SQL, Python (pandas, scipy.stats, statsmodels), Tableau, Looker, and Snowflake.

---

## 2. Education

M.S. Computer Science, University of Washington, expected December 2026.
Relevant coursework: Machine Learning, Database Systems, Statistical Learning.

B.S. Computer Science, University of California San Diego, 2024. GPA 3.8 / 4.0.

---

## 3. Skills

Languages: Python, SQL
Data Analytics: SQL, Python (pandas, scipy.stats, statsmodels), Tableau, Looker, A/B testing, retrospective studies, KPI reporting
Data Warehousing: Snowflake, PostgreSQL
ML (working knowledge): XGBoost, scikit-learn, model evaluation, PSI drift monitoring
Tools: Jupyter, dbt, Git
AI / LLM (working knowledge): LangChain, OpenAI API, RAG

---

## 4. Experience

### MedSync Health, Clinical Notes Summarization Platform

Software Engineer Intern, AI Team. 2025-06 to 2025-09.

- Built physician adoption dashboard in Tableau pulling from PostgreSQL across the 60-physician pilot; surfaced three distinct adoption cohorts (power users above 80%, moderates 30 to 60%, skeptics under 20%) that drove targeted retraining and lifted week-6 satisfaction from 74% to 91%.
- Designed and ran a 4-week A/B test on summary length using stratified randomization across 30 physicians; chi-squared test in Python's scipy.stats showed the short variant (~100 words) won by 14 percentage points in acceptance rate at p<0.05, and the team shipped it company-wide.
- Wrote SQL exploration queries against the 250K-note corpus to identify three lowest-quality specialties (Pediatrics, OB/GYN, Behavioral Health); prioritized prompt iteration on those categories and brought all three above the 75% quality threshold by week 7.
- Built a one-page weekly clinical-ops KPI report (PHI precision, summary quality, end-to-end latency, satisfaction, adoption cohorts) consumed by the CTO and Head of Clinical Operations in the Monday leadership meeting; replaced a fragmented set of Slack updates as the canonical pilot health signal.

### Forge Trading, Real-Time Fraud Detection Pipeline

Data Analytics and ML Contractor. 2025-12 to 2026-01.

- Analyzed 6 months of compliance flag history in SQL on Snowflake; identified three false-positive patterns (cross-border weekend travel, holiday retail spikes, miscategorized merchants) accounting for ~80% of analyst clearance hours; delivered a one-page memo with a target list of rules to retire.
- Built a Looker dashboard tracking daily flag volume, false-positive rate, and analyst clearance time by category; refreshed hourly, replaced a 2-day Excel weekly report, became the compliance team's daily standup reference.
- Designed retrospective study comparing the new ML model against the rules engine on 4M historical transactions; computed a 35% false-positive reduction with 95% confidence intervals using Python (statsmodels); presented to the executive review committee, became the main basis for green-lighting active scoring.
- Wrote SQL exploration notebooks for the analyst team showing which signal combinations correlated with confirmed fraud (e.g., geo-anomaly combined with merchant-risk score above thresholds was 12x more likely to be real fraud); incorporated into the manual review playbook.

### Pulse Social, Feed Ranking Microservice

Software Engineer Intern, Backend Team. 2026-06 to 2026-09.

- Built a Go-based feed ranking microservice serving 4,500 requests per second at peak with p99 latency under 60ms end to end (replaced a Python monolith path sitting at 220ms p99).
- Designed a three-stage composable architecture (candidate generation, ranking, post-processing) with a minimal model interface; enabled the data-science team to ship two new ranking models during the internship without backend involvement.
- Designed a gRPC API with three RPCs (GetFeed, RecordImpression, HealthCheck) consumed by three client services; deployed to Kubernetes via Helm with HPA, Prometheus metrics, and OpenTelemetry tracing per stage.
- Wrote a k6 load-test suite replaying two weeks of production traffic at 1.5x peak and ran two weeks of shadow deployment before any user rollout; shipped to 25% of users in 8 weeks with +6% session length lift in the A/B test (p<0.01).
