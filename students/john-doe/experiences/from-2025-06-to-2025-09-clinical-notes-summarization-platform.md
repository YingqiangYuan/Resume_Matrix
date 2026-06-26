# Clinical Notes Summarization Platform

> Period: 2025-06 to 2025-09. Company: MedSync Health. Role: Software Engineer Intern, AI Team. Industry: Healthcare.

## 1. Context

MedSync Health is a 50-person clinical AI startup based in Boston. The product helps primary-care physicians cut down on after-hours charting by turning raw EMR notes (FHIR-format JSON from Epic and Cerner) into clean, structured visit summaries.

When I joined, the team had a working prototype but it was clearly not production-ready. The prompts were brittle, the LLM sometimes hallucinated medication dosages, and the ingestion pipeline ran as a single Python script that occasionally lost notes. The pilot at a 60-physician primary-care group was slated to start in eight weeks and the AI team lead asked me to own the rebuild end to end.

I reported to the Tech Lead and worked closely with one senior backend engineer and one clinical informatics consultant.

---

## 2. What I Did

I rebuilt three layers of the system: ingestion, summarization, and serving.

For ingestion, I replaced the single-script pipeline with an event-driven setup. An AWS Lambda fronts an SQS queue that consumes FHIR Bundle JSON arriving via the customer's HL7 gateway. Each note goes through a PHI de-identification step (Microsoft Presidio plus a list of regex rules our clinical consultant wrote), gets normalized into a 14-entity PostgreSQL schema I designed, and lands in S3 for cold storage.

For summarization, I designed a prompt template with structured output enforced by Pydantic models. The output schema covered chief complaint, history, assessment, plan, and a list of cited source spans. I added a retry chain that re-prompts the LLM with the schema error when the first response fails validation. To catch hallucinations, I wrote an evaluation harness with eight metrics. Two of them, factual coverage and citation faithfulness, ran against a 300-note gold set our clinical consultant annotated by hand.

To improve grounding, I built a small RAG layer over a corpus of internal clinical guidelines. The retriever runs k-NN over OpenAI embeddings stored in pgvector. The top three guidelines get injected into the prompt as reference material.

For serving, I built a FastAPI service exposing a single summarize endpoint, deployed on AWS ECS Fargate behind an Application Load Balancer. I added Prometheus metrics, structured JSON logging via structlog, and tracing via OpenTelemetry.

---

## 3. Outcomes

By the end of the internship the system was in production at the 60-physician pilot site.

- Processed 250,000 historical clinical notes during the backfill, then about 4,000 new notes per day in production.
- Reduced physician chart review time from a baseline of 12 minutes per case to roughly 4 minutes per case, measured by the EMR's user-action telemetry across 30 physicians in a two-week before-and-after comparison.
- 91% physician satisfaction in the pilot survey at week 6. The main complaint was summary length, and we shipped a shorter-summary variant in response.
- Cut malformed structured-output rate from 11% (prototype) to under 0.5% (production) by combining the schema retry chain with explicit examples in the prompt.
- PHI de-identification precision measured at 99.4% on the internal test set, with the two false-positive cases caught in clinician review.

---

## 4. Tech Stack

Python, FastAPI, LangChain, OpenAI GPT-4 API, Anthropic Claude API, Microsoft Presidio, PostgreSQL with pgvector, Pydantic, AWS (Lambda, SQS, ECS Fargate, S3, RDS), Docker, Terraform, Prometheus, Grafana, OpenTelemetry, GitHub Actions.
