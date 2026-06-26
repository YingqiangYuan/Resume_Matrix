# Clinical Notes Summarization Platform

> Period: 2025-06 to 2025-09. Company: MedSync Health. Role: Software Engineer Intern, AI Team. Industry: Healthcare.

## 1. Context

MedSync Health is a 50-person clinical AI startup based in Boston. The product helps primary-care physicians cut down on after-hours charting (the paperwork doctors do at night to document each patient visit, which the industry estimates eats up two to three hours of a typical primary-care physician's day).

The way it does this is by turning raw EMR notes into clean, structured visit summaries that drop straight into the chart. EMR stands for Electronic Medical Record, the software systems hospitals use to store patient data, with Epic and Cerner being the two big vendors in the US.

The raw notes come in as FHIR-format JSON. FHIR is the healthcare industry's standard format for exchanging patient records, kind of like a structured JSON schema with prescribed fields for diagnoses, medications, vitals, and so on.

PHI, which comes up later in the story, stands for Protected Health Information (names, dates of birth, addresses, anything that can identify a real patient), and US law (HIPAA) requires that it be scrubbed before data leaves the clinical system.

When I joined, the team had a working prototype but it was clearly not production-ready. The prompts (the natural-language instructions we give the LLM, the Large Language Model that actually writes the summary) were brittle.

The LLM sometimes hallucinated medication dosages, and the ingestion pipeline ran as a single Python script that occasionally lost notes.

A pilot at a 60-physician primary-care group was slated to start in eight weeks, and the AI team lead asked me to own the rebuild end to end. The clock was the scary part. Pilots in healthcare are hard to reschedule because they involve coordinating clinical staff, IT, and compliance review on the customer side.

I reported to the Tech Lead and worked closely with one senior backend engineer, one clinical informatics consultant (a physician who acts as the bridge between the engineering team and how doctors actually think about a chart), and the Head of Clinical Operations who owned the pilot's success metrics.

---

## 2. What I Did

I rebuilt three layers of the system (ingestion, summarization, and serving) and on top of that I owned the data analytics work that told the team whether any of it was actually working.

For ingestion, I replaced the single-script pipeline with an event-driven setup. An AWS Lambda (a tiny piece of code that runs on demand without a server you manage) sits in front of an SQS queue (a message queue, basically a buffered to-do list of incoming work) that consumes FHIR Bundle JSON arriving via the customer's HL7 gateway.

Each note goes through a PHI de-identification step (Microsoft Presidio, an open-source PHI scrubber, plus a list of regex rules our clinical consultant wrote for the edge cases Presidio missed), gets normalized into a 14-table PostgreSQL schema I designed, and lands in S3 for cold storage. This part is plumbing, and once it ran for a week without dropping a note I stopped touching it.

For summarization, I designed a prompt template with structured output enforced by Pydantic models. Pydantic is a Python library that lets you declare the exact shape of data you expect and rejects anything that does not match.

The output schema covered chief complaint, history, assessment, plan, and a list of cited source spans so a physician could see which sentence in the original note backed each line of the summary.

I added a retry chain that re-prompts the LLM with the schema error when the first response fails validation, which is the trick that took our malformed-output rate from double digits down to near zero.

To catch hallucinations, I wrote an evaluation harness with eight metrics. Two of them, factual coverage and citation faithfulness, ran against a 300-note gold set our clinical consultant annotated by hand.

To improve grounding (grounding means anchoring the LLM's output in real reference material so it stops making things up), I built a small RAG layer. RAG stands for Retrieval-Augmented Generation, a pattern where you fetch relevant documents first and stuff them into the prompt so the LLM answers based on those documents instead of its own memory.

Our corpus was a set of internal clinical guidelines. The retriever runs k-NN (nearest-neighbor search) over OpenAI embeddings stored in pgvector (a PostgreSQL extension that lets you do vector search inside your regular database). The top three guidelines get injected into the prompt as reference material.

For serving, I built a FastAPI service exposing a single summarize endpoint, deployed on AWS ECS Fargate (a managed container service, meaning AWS runs the underlying machines for us) behind an Application Load Balancer. I added Prometheus metrics, structured JSON logging via structlog, and tracing via OpenTelemetry so we could see exactly where time was spent on each request.

The other half of my work, and honestly the half I learned the most from, was data analytics on top of all of this. Once the system was processing real notes, the question shifted from "does it run" to "is it actually helping any doctor." That is a question only data can answer.

The first thing I built was a physician adoption dashboard in Tableau. Tableau is the most common business-intelligence tool in industry. Think of it as a visual SQL editor that turns query results into charts and shared dashboards that non-engineers can read.

The dashboard pulled from our PostgreSQL database and showed, for each of the 60 pilot physicians, their adoption rate (how often they accepted the AI summary into the chart instead of ignoring it), their average time saved per case, and their satisfaction score from the weekly survey.

Once I had two weeks of data, the cohort breakdown jumped out. There were power users who accepted the summary on more than 80% of their cases, moderate users in the 30 to 60% range, and skeptics under 20%.

The clinical operations team used that cohort split to target follow-up training at the skeptics specifically, instead of sending a generic email to all 60 physicians. The Head of Clinical Operations told me later that targeted outreach is what pulled the week-six satisfaction number up.

The second analytics piece was an A/B test on summary length. Several physicians had complained in the early weeks that the summary was too long to scan in the few seconds they had between patients. We had a hunch that a shorter summary would land better, but a hunch is not evidence.

I designed an A/B test of two variants, a long version around 250 words and a short version around 100 words. I split the 30-physician test group into two arms using stratified randomization, a technique that makes sure both arms have similar proportions of specialty, seniority, and patient volume so the comparison is fair.

Over four weeks I tracked acceptance rate (whether the physician copied the summary into the chart vs. dismissed it). I ran a chi-squared test in Python's scipy.stats (a standard statistical test for comparing proportions between two groups).

The short version came out 14% higher in acceptance rate at p less than 0.05 (meaning there is less than a 5% chance that the difference was just random noise). On the strength of that result the team shipped the short variant company-wide.

The third piece was SQL exploration of summary quality by specialty. I wrote queries against the 250,000-note corpus to find which clinical specialties had the lowest summary quality scores. Pediatrics, OB/GYN, and Behavioral Health came out at the bottom.

I took the finding to the clinical consultant, who confirmed that those specialties have unusual note structures (pediatric vitals are age-relative, behavioral notes are heavily narrative and free-form, OB/GYN charts mix in obstetric history that does not look like a normal visit note) that our generic prompt was not handling well.

I prioritized prompt iteration on those three categories, tracked their weekly quality scores in a separate view, and by week seven all three were above our 75% quality threshold.

The fourth piece tied everything together. I built a one-page weekly KPI report covering PHI removal precision, summary quality scores, end-to-end latency, physician satisfaction, and the adoption-cohort breakdown. The CTO and the Head of Clinical Operations read it in the Monday leadership meeting.

By the end of the internship it had become the canonical source of truth for "how is the rollout going," and it replaced what had previously been a patchwork of Slack updates and one-off screenshots that no one fully trusted.

---

## 3. Outcomes

By the end of the internship the system was in production at the 60-physician pilot site.

- Processed 250,000 historical clinical notes during the backfill, then about 4,000 new notes per day in production.
- Reduced physician chart review time from a baseline of 12 minutes per case to roughly 4 minutes per case, measured by the EMR's user-action telemetry across 30 physicians in a two-week before-and-after comparison.
- 91% physician satisfaction in the pilot survey at week 6, up from 74% at week 2 after the targeted training rollout the adoption dashboard made possible.
- The summary-length A/B test produced a 14% lift in acceptance rate (p less than 0.05) for the short variant, which became the default.
- Prompt iteration on the three lowest-quality specialties (Pediatrics, OB/GYN, Behavioral Health) lifted all three above the 75% quality threshold by week 7.
- Cut malformed structured-output rate from 11% (prototype) to under 0.5% (production) by combining the schema retry chain with explicit examples in the prompt.
- PHI de-identification precision measured at 99.4% on the internal test set, with the two false-positive cases caught in clinician review.
- The weekly KPI report became the standing artifact in the Monday leadership meeting.

---

## 4. Tech Stack

Python, SQL, FastAPI, LangChain, OpenAI GPT-4 API, Anthropic Claude API, Microsoft Presidio, PostgreSQL with pgvector, Pydantic, pandas, scipy.stats, Tableau, AWS (Lambda, SQS, ECS Fargate, S3, RDS), Docker, Terraform, Prometheus, Grafana, OpenTelemetry, GitHub Actions.
