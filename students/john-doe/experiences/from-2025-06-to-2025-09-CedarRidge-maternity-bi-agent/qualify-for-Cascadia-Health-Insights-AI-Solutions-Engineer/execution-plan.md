# Gap Fill Plan, Cascadia Health Insights AI Solutions Engineer

This document builds on the previous gap analysis and converts the 9 identified gaps into 9 mini-POCs (each POC is a focused skill-learning project, not a pretend business project) plus a matching index of tutorial documents. The plan runs 12 weeks from March through May 2026 and is aligned to the mid-March Cascadia interview window. Every POC has its own practice directory at `pocs/poc-NN-xxx/` and a tutorial at `tutorials/NN-xxx.md`.

Before going further, the term "mini-POC" needs a precise definition for this document.

In this framework, a mini-POC specifically means a skill-learning practice project. The goal is to push a specific skill (a framework, a data format, a compliance requirement) from "I read the docs" to "I wrote it, I ran it, I hit the rough edges myself".

It does not need a contrived real-world business scenario, and it does not need to be embedded in a Cascadia customer business context. The corpus and data for each mini-POC can come from public sources like Wikipedia or Synthea synthetic data, as long as the material exercises the target skill.

This distinction matters especially for a student like John, who has already shipped a real BI project at Cedar Ridge. The point is to keep him from spending his spare time inventing another fake project that just rehearses skills he already has.

---

## 1. Execution Principles and Timeline

The whole gap fill plan is compressed into 12 weeks from early March to late May 2026. The goal is that by John's mid-March technical phone screen and early-April onsite with Cascadia, he can tell a "I built it, I hit the rough edges, I know what I'd change next time" story for every one of the 9 gaps, instead of stopping at "I read the docs".

The honest reality is that 9 gaps cannot all reach production-grade quality in 12 weeks. The 3 Core gaps (POC-01, POC-02, POC-03) need to reach the level of hands-on coding and articulate tradeoffs, but POC-04 (CDK) and POC-07 (compliance auditing) only need to ramp to a "credible interview level". John does not need to ship a real multi-account production rollout for those.

Time allocation follows three principles.

First, attack the high-entanglement POCs first, because a single POC unlocks several gaps, which gives the best return on time invested.

Second, attack the stack that the onsite system design round is most likely to probe (Strand Agents, RAG eval, Bedrock AgentCore) and complete that main line between Week 1 and Week 6.

Third, break the client-facing writing soft-skill POC into small pieces and stick them to the tail of every hard-skill POC. Each finished POC produces a 1 to 2 page client-facing style design doc on the way out, and by Week 12 this naturally accumulates 8 to 10 pages of writing samples.

John's current baseline is that the maternity BI agent project he just shipped at Cedar Ridge has already taken him through the full path of semantic layer, Snowflake, and Charge Nurse user interviews. So POC-05 (Semantic Layer YAML) and POC-08 (client-facing writing) have ready material he can reuse and rewrite, instead of starting from zero. The time budget on those two can be squeezed down to 2 or 3 days. The freed-up budget goes into the LLM agent stack (Strand, Bedrock, RAG) and AWS infra (CDK), which are where John sits farthest from production-grade and where the interviewer is most likely to dig.

The weekly budget is fixed at 12 to 15 hours (evenings plus weekends), giving 144 to 180 hours total from March to May. That budget roughly matches the workload of 9 POCs but leaves no buffer. If feedback from the April onsite signals that some direction needs remediation, the polish phase of the lowest-priority POC-09 (Python code quality) gets cut so the trunk survives.

Three more things need to be said up front, to keep John from being killed by perfectionism mid-execution.

First, every POC stops at demo-ready. No chasing 99 out of 100. If 70 to 80 out of 100 is enough to tell a story, move to the next POC. Cascadia interviewers care about how you think and how you make tradeoffs, not whether your RAG hit rate beats baseline by a couple of points.

Second, every finished POC immediately gets a 3 to 5 minute spoken walkthrough video recorded and uploaded to a private YouTube channel or stored locally. This is for quick review before later mock interviews, and it surfaces the "I thought I could explain it, turns out I cannot" blind spots.

Third, every weekend spend 30 minutes on a progress retro. The vehicle is a simple markdown journal recording "what I finished, where I got stuck, what I will change next week". This journal itself becomes a story library for the Cascadia onsite "comfort with ambiguity" question.

The timeline also needs to align with the real cadence of Cascadia's hiring process.

The Cascadia JD states at the bottom that the process is a 30 minute recruiter screen, a take-home Python and SQL exercise (3 hours, due within a week), a 60 minute technical phone screen, and a half-day onsite of 4 rounds. The whole thing typically runs 3 to 4 weeks.

Working backwards, submitting the resume in mid-March means hitting onsite around mid-April. So the hard milestones for this plan are: Strand Agent trunk done by Week 4, RAG eval baseline done by Week 6, AgentCore endpoint demoable by Week 8.

Those three milestones happen to be the most likely follow-up topics in the system design round of the Cascadia onsite. Missing any of them leaves John without an anchor at the onsite stage.

Another reality to acknowledge is that John cannot stop his existing day job at Cedar Ridge. The 9 POCs are evening and weekend work on the side.

That means the plan has essentially zero slack and demands tight discipline around protecting the weekend 4-hour and weekday 2-hour blocks.

Any week that loses those blocks more than twice triggers a retrospective to figure out whether the cause is work conflict or motivation. If it is motivation, the recommendation is to prioritize fast-output, fast-feedback small tasks like POC-08 (writing a client-facing one-pager) to rebuild rhythm.

There is also an implicit principle in budget allocation: the combined budget for the 4 Core POCs (48 days) must occupy the main slots of the first 8 weeks. It cannot slide backward.

The reason is that the 4 Core POCs are the stack the Cascadia interviewer is most likely to dig into. If they slip past Week 9 and the interviewer asks for an extra round or accelerates the timeline at the end of Week 8, John is stuck with "the main course is still in the oven".

The 5 Important and Nice-to-have POCs can be pushed to Week 9 or later, because even if they are not fully cooked, John can fall back on "I have these on my next 30-day plan" during interviews. Core POCs do not have that fallback.

The plan also needs to be explicit about what it does not cover, so John does not mistake this document for the complete onsite prep. Resume rewriting, targeted Cascadia interview question drilling, recruiter conversation scripts, and salary negotiation prep are out of scope. Those four belong to a separate workflow in the final 2 weeks before the onsite. Mock interview question selection and STAR-format behavioral story prep are also handled in a separate document. This plan focuses on exactly one thing: turning the 9 gaps into demonstrable artifacts.

---

## 2. POC Priority Matrix

The table below ranks the 9 POCs across three dimensions: impact on Cascadia interview success, entanglement with other gaps, and time cost. The entanglement column is the dimension John personally weights highest. It captures how many other gaps a given POC covers as a side effect. For example, finishing POC-03 (RAG plus eval) mostly resolves half of POC-01 (Strand Agents) and POC-02 (Bedrock AgentCore) as well, because in Cascadia's Insight Assistant architecture these three are three segments of the same data flow.

| POC | Gap Name | Severity | Interview Impact | Entanglement (which gaps it unlocks) | Estimate | Overall Rank |
| --- | --- | --- | --- | --- | --- | --- |
| POC-03 | RAG + evaluation harness | Red | Very High | Unlocks the retrieval semantics shared by POC-01 / POC-02 / POC-05 | 18 days | 1 |
| POC-01 | Strand Agents | Red | Very High | Unlocks the agent trunk for POC-02 / POC-03 | 12 days | 2 |
| POC-02 | AWS Bedrock AgentCore Runtime | Red | High | Unlocks the infra template for POC-04 | 10 days | 3 |
| POC-04 | AWS CDK Python | Red | High | Unlocks the deployment path for POC-02 / POC-07 | 8 days | 4 |
| POC-05 | Semantic Layer YAML | Yellow | High | Unlocks the semantic retrieval corpus for POC-03 | 5 days | 5 |
| POC-08 | Client-facing writing | Yellow | High | One page of writing produced at the tail of every POC | 4 days | 6 |
| POC-06 | FHIR / HL7 | Yellow | Medium | Unlocks the PHI field identification for POC-07 | 6 days | 7 |
| POC-07 | HIPAA + TJC + SOC 2 audit | Yellow | Medium | Unlocks the audit trail design for POC-02 | 5 days | 8 |
| POC-09 | Production-grade Python code quality | Orange | Medium-Low | Acts horizontally across every POC's code repo | 4 days | 9 |

Two lines through this matrix are worth calling out. First, the POC-03 to POC-01 to POC-02 chain is the actual skeleton of Cascadia's agentic analytics platform. Doing all three together lets you share one corpus, one eval set, and one agent main program, which is why they are concentrated in Week 1 through Week 6. Second, the POC-04 to POC-02 to POC-07 chain: the CDK stack simultaneously carries the AgentCore Runtime deployment and the audit log landing, so writing one stack validates knowledge of three areas.

POC-08 and POC-09 are horizontal capabilities. They do not get standalone weeks. They ride on the tail of every hard POC. The benefit is that the writing samples and the code quality samples are byproducts of real projects, not "I wrote this just so I had something to show" artifacts.

When reading this table, one more thing matters. The "interview impact" column is not "general market recognition". It is "impact under this specific Cascadia JD text and this specific onsite flow".

For example, POC-07 (compliance audit) would almost never come up in a generic AI Engineer interview, but the Cascadia onsite includes a client-facing case study segment, and that case will almost certainly touch on PHI handling or audit design. So it gets ranked Medium in this table, not Low.

Similarly POC-08 (client-facing writing) is usually a soft bonus in most technical roles, but because Cascadia explicitly writes "15 pages of client-facing documentation per quarter" into the must-haves, this table lifts it to High. This JD-text-by-text grounding is what separates this matrix from a generic learning roadmap.

The entanglement column can also be read as a dependency graph.

POC-01 is the agent trunk, POC-02 is the runtime layer that deploys the agent to AWS, POC-03 is the retrieval and evaluation layer inside the agent. The three share the same Wikipedia corpus and the same Q-A golden set, so they are physically coupled.

POC-04 is the deployment vehicle for POC-02. The CDK stack written for POC-04 is the infra-as-code description of POC-02. They cannot be separated.

POC-05 is the "advanced version" of POC-03, swapping plain text retrieval for structured semantic retrieval. The YAML phrasing needs to stay consistent with the generation prompt in POC-03.

POC-06 and POC-07 form a "healthcare data compliance chain". POC-06 solves how to parse the data, POC-07 solves how to leave an audit trail after parsing. Telling the story of the two together is more compelling than telling either one alone.

Finally POC-08 and POC-09 sit as a "membrane covering all nodes" in the dependency graph. POC-08 is the output artifact of every hard POC, POC-09 is the code shell of every hard POC. Thinking of POC-08 and POC-09 as an "output layer" rather than standalone nodes makes it clear why they should not get their own weeks. This dependency structure is not just convenient for scheduling. It is also a "this is my learning architecture" diagram that John can draw straight onto the whiteboard in the onsite system design round. Drawing this diagram is itself a bonus story.

---

## 3. Core POCs

Below are detailed definitions of the 4 Core POCs. The Core criterion is: appears with high frequency in the JD, almost certain to come up in the system design interview round, and John currently has zero hands-on experience with it. These 4 POCs consume about 60% of the total budget over 12 weeks (about 28 days out of 48), and are the real backbone of the plan.

Every Core POC is written under a uniform structure: the first item is the skill definition, the second is the input and setup, the third is the artifact, the fourth is the success criteria, the fifth is the connection to the Cascadia JD, the sixth is the time budget, the seventh is the tutorial index, the eighth is gotchas or follow-on exercises.

This structure is identical to the Important POCs below for easy side-by-side comparison. If the Cascadia onsite asks John to "pick one project and go deep", he can quickly assemble a 3-minute narrative skeleton from these 8 items.

---

### POC-01: Build a tiny Strand Agent over a Wikipedia corpus

- The core skill this POC practices is the basic programming model of the Strand Agents framework: how to define a tool, how to register it into the agent loop, and how to manage multi-turn conversation state. Strand is the framework Cascadia uses in production. The JD explicitly states "we use Strand Agents in production", so this stack is non-optional.
- The input dataset is 50 Wikipedia articles about PNW healthcare geography (Seattle, Portland, Spokane, etc.) exported to markdown and stored in local SQLite. This corpus is chosen because the content is stable, the size is small, and it can be reused for the RAG evaluation in POC-03.
- The expected artifact is roughly 100 lines of Python plus a README. The agent should be able to answer questions like "how many tertiary-care hospitals are in Seattle" grounded in the corpus. The script needs a clear separation between the tool definition layer, the agent runtime layer, and the CLI entry layer, which makes future extension easier.
- The success criteria are: the agent correctly routes to the search_corpus tool, the agent preserves context across multi-turn conversation, and the agent handles the "corpus has no answer" fallback case. All three are required, because these are exactly the three agent-engineering questions Cascadia interviewers ask most often.
- Connection to the JD: the line "comfortable with one LLM application framework" is strongly preferred, and Strand is the framework of choice. This POC addresses that requirement directly.
- Time estimate is 12 days, broken down as: 3 days reading Strand docs and examples, 4 days writing a first working version, 3 days iterating on tool design and prompts, 2 days writing the design doc and recording a 5-minute walkthrough video.
- Corresponding tutorial: `tutorials/01-strand-agents-quickstart.md`. The tutorial walks John from installing Strand, defining the first tool, adding memory, and running the agent as a CLI, across those four stages.
- Gotcha to flag: Strand's tool registration is decorator-based, and on a first attempt it is easy to write the type hints incorrectly, which breaks schema inference. The recommendation is to write the tool function signatures so they pass mypy strict mode first, then register them with the agent. This lived experience is a very concrete detail anchor when telling a debugging story at onsite.

---

### POC-02: Stand up a minimal Bedrock AgentCore Runtime deployment

- The core skill of this POC is hands-on deployment experience with AWS Bedrock AgentCore Runtime: how to register an agent definition with AgentCore, how to configure session management, and how to read agent traces from CloudWatch. AgentCore only went GA in late 2024, so people with real hands-on experience are very rare in the market.
- Input setup: reuse the Strand agent definition from POC-01, package it into an AgentCore-compatible runtime format, and deploy it to a personal AWS sandbox account. The corpus and tools stay the same. The focus is on swapping out the runtime layer.
- The expected artifact is an AgentCore endpoint invokable from the AWS CLI, a collection of session log screenshots, and a roughly one-page tradeoff write-up of "AgentCore vs self-deployed Lambda agent". That tradeoff doc is directly usable in the system design interview.
- The success criteria are: the endpoint responds stably, session state can be correctly recovered within 30 minutes, and traces are visible in CloudWatch. Cascadia's operations isolate AgentCore sessions per client, so session isolation has to be done correctly.
- Connection to the JD: the deployment stack listed in the JD is "Bedrock, AgentCore Runtime, Lambda, ECS, CloudWatch". AgentCore is in the second position, and it is the scarcest hands-on experience on the current market.
- Time estimate is 10 days: 2 days reading AWS docs and limit notes, 3 days building the basic deployment, 3 days getting sessions and traces working, 2 days writing the comparison doc and cost estimate.
- Corresponding tutorial: `tutorials/02-bedrock-agentcore-runtime.md`. The tutorial covers the AgentCore conceptual model, IAM configuration, session lifecycle, and common error codes.
- Risk to flag: AgentCore calls the Bedrock model billed per token. The sandbox account needs a budget alarm. Otherwise one looped-call bug can rack up tens of dollars in billing within hours. This lived experience also doubles as a cost-aware engineering story.

---

### POC-03: Build a RAG pipeline with a measurable evaluation harness

- The core skill of this POC is end-to-end implementation of a RAG system plus a quantitative evaluation harness. The point is not sophistication of retrieval. The point is that the eval harness exists at all: can you produce hit rate, MRR, and faithfulness numbers, and can you explain the definition behind each number.
- Input setup: use the 50 Wikipedia articles from POC-01, plus a hand-constructed 50-item Q-A golden set (each item annotated with the expected supporting passage ID). Embeddings use OpenAI text-embedding-3-small or Bedrock Titan. The vector store is local ChromaDB.
- The expected artifacts are three Python modules (retrieval, generation, evaluation), an eval report markdown table comparing three setups (baseline, chunk size tuning, reranker added), and a one-page application plan titled "how I would apply this eval to a Cascadia client corpus".
- The success criteria are three: retrieval hit@3 improves by 10 percentage points or more over baseline, John can articulate the definition and limits of the faithfulness metric, and John can list 5 meaningful directions for extending the eval set. Cascadia's RAG interview questions are very likely to orbit around eval, so eval matters more than retrieval itself.
- Connection to the JD: the line "experience with at least one retrieval-augmented-generation (RAG) implementation including evaluation. We will probe how you measured RAG quality" is almost a direct definition of POC-03.
- Time estimate is 18 days: 2 days reading RAG eval literature (RAGAS, Ares, etc.), 4 days writing retrieval and generation, 5 days constructing the Q-A set and the eval harness, 4 days running the three-way comparison experiments, 3 days writing the eval report.
- Corresponding tutorial: `tutorials/03-rag-with-eval-harness.md`. The tutorial focuses on how to construct an eval set, which metrics are worth chasing, and which metrics are noise.
- An easily overlooked detail is the "negative example" coverage in the eval set. Deliberately construct 10 questions whose answers are not in the corpus, and see whether the agent stably outputs "I don't know". In Cascadia's clinical setting the cost of hallucination is extremely high, and interviewers commonly press on this point.

---

### POC-04: Provision a Lambda + S3 + IAM stack via AWS CDK Python

- The core skill of this POC is how to write AWS CDK Python: how to compose resources with Constructs, how to configure IAM Policy, and how to run a stack through the full cdk deploy / cdk destroy lifecycle. Every Cascadia deployment goes through CDK templates, so no CDK experience equals not being able to start work.
- Input setup: start a CDK Python project from scratch. The target resources are one Lambda (Python runtime), one S3 bucket, one DynamoDB table, and the corresponding IAM Role and Policy. The Lambda reads from S3 and writes to DynamoDB. Keep the logic as simple as possible.
- The expected artifacts are `app.py`, `stacks/main_stack.py`, and `tests/test_stack.py` (using the CDK assertion module), a CloudFormation template produced by `cdk synth`, and a README explaining how to deploy and destroy.
- The success criteria are: cdk deploy succeeds on the first try, cdk destroy leaves no orphan resources, and test_stack contains at least 3 assertions (IAM Policy contains no wildcard, S3 bucket encryption is on, Lambda timeout does not exceed 30 seconds).
- Connection to the JD: the line "Familiarity with AWS CDK (Python) is a meaningful plus because every Cascadia deployment ships through CDK" is a meaningful plus but also a day-to-day operational requirement.
- Time estimate is 8 days: 1 day reading CDK Python docs, 3 days writing the stack and tests, 2 days getting the deployment working, 2 days iterating on IAM least-privilege and writing the README.
- Corresponding tutorial: `tutorials/04-aws-cdk-python-quickstart.md`. The tutorial walks from cdk init through cdk deploy, with focus on IAM least-privilege and the testing pattern.
- Follow-on exercise: once the first stack is working, split it into 2 stacks (one network layer, one application layer) with cross-stack references. This is the most common topology in Cascadia production deployments, and the probability of being asked about it is higher than for a single stack.

---

## 4. Important POCs

The Important tier contains 4 POCs, matching the strongly preferred and preferred sections of the Cascadia JD.

Their common feature is that doing them well meaningfully adds points but skipping them does not auto-fail you. So their combined budget is held under 20 days, averaging 5 days each.

Within this tier, POC-05 and POC-08 have ready material from the Cedar Ridge project to reuse, so they will be easier to execute than POC-06 and POC-07.

A reminder: in the onsite, the Important tier POCs have a much higher "probability of being asked" than "depth of probing".

The interviewer uses this tier as an entry point. For example, they may open with "have you done FHIR?". If John answers "yes, I parsed Patient, Encounter, and Observation resources", the interviewer follows the thread deeper. If he answers "no", they switch topic.

So the goal of this tier is "each POC supports a 30-second opening intro plus a 2-minute deep-dive story", not getting every one of them to expert level.

---

### POC-05: Design a Semantic Layer YAML over a Snowflake-shaped schema

- The core skill of this POC is YAML authoring and metadata organization for a semantic layer. The focus is not Snowflake syntax itself. It is how to formalize a clinical operations metric (e.g. average length of stay) into the entity, measure, and dimension triplet. This is the core daily action at Cascadia.
- Input setup: use Synthea-generated synthetic hospital data, import to local PostgreSQL (no need to actually stand up Snowflake, the schema shape is the same), and write semantic YAML around 5 clinical operations metrics: ALOS, bed occupancy, readmission rate, handoff completion, order turnaround time.
- The expected artifacts are a 200 to 300 line semantic.yaml, an accompanying "metric definition glossary" (each metric paired with a one-paragraph English definition), and a comparison table titled "how this differs from the definitions we used in the Cedar Ridge project".
- The success criteria are: all 5 metrics can be expressed by a SQL expression in YAML, the grain and filter conditions of each metric are explicitly declared, and John can articulate "why use a semantic layer instead of a view".
- Connection to the JD: the line "Partner with each client's senior clinical analyst to codify their internal metric definitions into the semantic layer YAML" describes the core daily verb of the role.
- Time estimate is 5 days, because Cedar Ridge has ready material to reuse. The main work is rewriting, cleaning up, and adding the comparison table.
- Corresponding tutorial: `tutorials/05-semantic-layer-yaml-design.md`. The tutorial covers tradeoffs across three approaches: cube form, metric layer form, and pure view form.
- Reuse note: the maternity metric YAML written for the Cedar Ridge project can be lifted directly as a 6th metric tacked on the end, serving as a bridge between "on-the-job project material" and "practice project material". When telling stories in the interview, this gives John a way to connect his two pieces of experience.

---

### POC-06: Parse FHIR Bundle JSON from Synthea data into a normalized table

- The core skill of this POC is hands-on understanding of the FHIR resource model: how to read the JSON structure of the Patient, Encounter, and Observation resources, how to flatten nested fields into relational tables, and how to handle reference fields (e.g. "Patient/abc-123").
- Input setup: use Synthea to generate FHIR Bundle JSON files for 100 synthetic patients. Write a Python script that extracts the three resources into three PostgreSQL tables while preserving reference relationships.
- The expected artifacts are the extraction script, the DDL for the three table schemas, and a short comparison write-up titled "FHIR vs the custom schema we used at Cedar Ridge". This short write-up is directly usable when telling healthcare data stories in the interview.
- The success criteria are: all 100 Bundles parse without error, reference fields associate correctly, and John can articulate the difference between valueQuantity, valueCodeableConcept, and valueString in FHIR.
- Connection to the JD: "prior exposure to healthcare data formats (FHIR, HL7)" is at the preferred level, not must-have, but failing to talk about FHIR will make the healthcare experience look thin.
- Time estimate is 6 days, including reading the key chapters of the FHIR R4 spec, writing the parsing script, and hitting the reference-type gotchas.
- Corresponding tutorial: `tutorials/06-fhir-bundle-parsing.md`. The tutorial focuses on the 3 core resources in R4 rather than covering all 145 resource types.
- Scope-control reminder: FHIR has an enormous number of resources, and newcomers most easily fall into the trap of "I will read the entire spec before writing any code". This POC strictly limits to 3 resources. In the interview, it is better to say "I only did Patient, Encounter, and Observation, but I can explain them clearly" than to fake coverage of everything.

---

### POC-07: Draft an audit trail design aligned to HIPAA + TJC + SOC 2

- The core skill of this POC is not writing code. It is writing a design doc such that "if a Cascadia client's compliance officer came in to review it, they would understand it". The focus is on mapping HIPAA Privacy Rule, TJC handoff standards, and SOC 2 CC7 requirements into specific log fields and retention policies.
- Input setup: take the Bedrock AgentCore deployment from POC-02 as the base, pretend this is a customer production environment, and write an "audit trail design doc" that lists what fields each log line records, where they are stored, how long they are retained, and who can access them.
- The expected artifact is a 3 to 4 page design doc covering a field table, a retention table, an access control table, and an incident response playbook section. Every reference must explicitly cite which clause of HIPAA 164.312 or which item of the TJC handoff standard.
- The success criteria are: the key clauses of all 3 compliance frameworks map to concrete design choices, John can articulate how PHI fields should be redacted in logs, and John can identify the "what if the audit log itself goes missing" fallback.
- Connection to the JD: the line "Author audit-trail and compliance documentation aligned to HIPAA, TJC hand-off communication standards" is almost a direct counterpart of POC-07.
- Time estimate is 5 days, mostly spent reading HIPAA / TJC / SOC 2 source excerpts, writing the document, and doing a dry run with ChatGPT playing the role of "compliance officer".
- Corresponding tutorial: `tutorials/07-audit-trail-hipaa-tjc-soc2.md`. The tutorial gives a minimum viable audit trail field table in checklist form.
- Interview usage: this document becomes a ready case study for the onsite client-facing case study segment. It can be performed as "if you were the Cascadia compliance liaison engineer, how would you respond to a customer compliance officer's review". Practicing the conversation from the artifact backwards is far more powerful than reciting compliance theory in the abstract.

---

### POC-08: Produce a client-facing one-pager for each prior POC

- The core skill of this POC is translating technical work into "a 1-page document a non-technical clinical operations leader can read". The Cascadia JD explicitly requires producing 10 to 15 pages of client-facing documentation per quarter. This is a hard requirement.
- Input setup: tack a 1-page client-facing one-pager onto the tail of each POC from POC-01 through POC-07. Force zero jargon, and force a paragraph titled "why this matters to the Charge Nurse".
- The expected artifacts are 7 markdown one-pagers (each about 250 to 300 words) with uniform formatting (problem, approach, output, next step). Bundled into a portfolio PDF and submitted as a writing sample during onsite.
- The success criteria are: every one-pager is readable in 90 seconds by someone who does not know LLMs, none contain terms like "agent loop" or "vector embedding", and each has an actionable next step.
- Connection to the JD: the line "strong written communication skills... roughly ten to fifteen pages of client-facing documentation per quarter; this is not negotiable" is a must-have.
- Time estimate is 4 days, spread out as a half day at the tail of each hard POC.
- Corresponding tutorial: `tutorials/08-client-facing-writing-templates.md`. The tutorial provides 4 one-pager templates and 1 anti-example.
- Self-check method: after writing each one, read it aloud and record yourself. On playback, if you hear any sentence that "stutters" or "winds", go back and rewrite that paragraph. This method comes from the "narrative memo" read-aloud practice at Amazon and surfaces issues faster than silent proofreading.

---

## 5. Nice-to-have POC

The Nice-to-have tier contains only POC-09, matching the "production-grade or production-adjacent codebase" line in the JD.

It is placed in nice-to-have rather than important not because it does not matter, but because this work cannot be done in isolation. It is essentially "cleaning up the previous 8 POCs", and depends on those POCs being completed first.

Week 11 to Week 12 is the natural ripening window for this work.

Another reason for the downgrade is that in the interview setting, "production-grade Python" is more often evaluated through a code sample than through questions.

Cascadia's take-home Python and SQL round will ask John to submit code, and that is where this capability actually gets graded.

So the goal of POC-09 is "make every take-home deliverable look presentable", not "produce a standalone showcase point".

---

### POC-09: Apply production-grade Python hygiene across the POC repo

- The core skill of this POC is consolidating the loose scripts of the previous 8 POCs into one repository and wrapping them in the "looks like production code" shell of uv package management, ruff / mypy static checking, pytest unit tests, and GitHub Actions CI. The focus is not on fixing every line of code. The focus is bringing the repo into a state where it can be thrown out as a code sample.
- Input setup: a monorepo where the subdirectories are the 9 POCs. The root contains `pyproject.toml`, `.pre-commit-config.yaml`, and `.github/workflows/ci.yml`.
- The expected artifacts are a green CI badge, unit tests with at least 40% coverage, zero-warning ruff code, and a top-level README that threads the storyline through the 9 POCs.
- The success criteria are: when the repo link is sent to the Cascadia recruiter, the first impression is not "weekend hack scripts", and the interviewer can open any subdirectory and find a sensible structure.
- Connection to the JD: the line "strong Python proficiency including at least one production-grade or production-adjacent codebase. We will ask to discuss a code sample during the technical loop" turns this monorepo itself into the code sample.
- Time estimate is 4 days, concentrated in Week 11 to Week 12, because earlier POCs are still in flux and tightening CI too early is not worth it.
- Corresponding tutorial: `tutorials/09-python-production-hygiene.md`. The tutorial provides minimal configuration templates for the uv + ruff + mypy + pytest + actions stack.
- For threading the README story, the recommendation is to follow the Anthropic / OpenAI cookbook style: each subdirectory gets a 2 to 3 line "why this exists" header so a recruiter clicking through can grasp the portfolio's arc in 30 seconds.

---

## 6. Tutorial Index

Below is the index of tutorial documents corresponding to the 9 POCs. The tutorials themselves are standalone documents and are not expanded inside this plan. Only the archive location is listed. POC scaffolding code lives under the `pocs/` subdirectory, with one folder per POC.

| POC | Gap Name | Tutorial Filename | POC Scaffold Path |
| --- | --- | --- | --- |
| POC-01 | Strand Agents | `tutorials/01-strand-agents-quickstart.md` | `pocs/poc-01-strand-agents/` |
| POC-02 | AWS Bedrock AgentCore Runtime | `tutorials/02-bedrock-agentcore-runtime.md` | `pocs/poc-02-bedrock-agentcore/` |
| POC-03 | RAG + evaluation harness | `tutorials/03-rag-with-eval-harness.md` | `pocs/poc-03-rag-eval/` |
| POC-04 | AWS CDK Python | `tutorials/04-aws-cdk-python-quickstart.md` | `pocs/poc-04-cdk-python/` |
| POC-05 | Semantic Layer YAML | `tutorials/05-semantic-layer-yaml-design.md` | `pocs/poc-05-semantic-yaml/` |
| POC-06 | FHIR / HL7 | `tutorials/06-fhir-bundle-parsing.md` | `pocs/poc-06-fhir-parsing/` |
| POC-07 | HIPAA + TJC + SOC 2 audit | `tutorials/07-audit-trail-hipaa-tjc-soc2.md` | `pocs/poc-07-audit-trail/` |
| POC-08 | Client-facing writing | `tutorials/08-client-facing-writing-templates.md` | `pocs/poc-08-one-pagers/` |
| POC-09 | Production-grade Python code quality | `tutorials/09-python-production-hygiene.md` | `pocs/poc-09-repo-hygiene/` |

Authoring of the tutorial documents is outside the scope of this skill. This plan only locks in the filenames and archive locations to keep later tutorial authors and POC implementers consistent in their references.

Naming convention: tutorial filenames use a "NN-topic-keyword" double-hyphenated phrase. Topic names prefer English technical terms to align with official documentation. POC scaffold directories follow the same "poc-NN-topic" format to avoid the compatibility issues of non-English paths across toolchains.

Each POC subdirectory should have a `README.md` (explaining what the POC is doing), `src/` or `app/` (actual code), `eval/` or `tests/` (evaluation or tests), and `docs/` (client-facing one-pagers and other documentation artifacts).

---

## 7. Execution Cadence and Milestones

Below is the week-by-week layout for the 12 weeks (March 2 to May 24, 2026). The overall rhythm follows "trunk first, branches next, horizontal capabilities tacked on the tail". Mock interviews happen in Week 4, Week 8, and Week 12 to force the progress so far into story form.

| Week | Date Range | Primary POC | Secondary POC (tail) | Milestone |
| --- | --- | --- | --- | --- |
| Week 1 | 03-02 to 03-08 | POC-01 Day 1-5 | None | First version of Strand agent runs |
| Week 2 | 03-09 to 03-15 | POC-01 Day 6-12 | POC-08 write first one-pager | Strand POC complete |
| Week 3 | 03-16 to 03-22 | POC-03 Day 1-7 | POC-08 second one-pager | 50-item Q-A golden set in place |
| Week 4 | 03-23 to 03-29 | POC-03 Day 8-14 | Mock interview #1 | RAG eval baseline report produced |
| Week 5 | 03-30 to 04-05 | POC-03 Day 15-18 + POC-02 Day 1-3 | POC-08 third one-pager | RAG eval three-way comparison complete |
| Week 6 | 04-06 to 04-12 | POC-02 Day 4-10 | POC-08 fourth one-pager | AgentCore endpoint live |
| Week 7 | 04-13 to 04-19 | POC-04 Day 1-5 | POC-05 Day 1-2 | CDK stack v1 deploy succeeds |
| Week 8 | 04-20 to 04-26 | POC-04 Day 6-8 + POC-05 Day 3-5 | Mock interview #2 | semantic.yaml complete |
| Week 9 | 04-27 to 05-03 | POC-06 Day 1-6 | POC-08 fifth one-pager | FHIR parsing script complete |
| Week 10 | 05-04 to 05-10 | POC-07 Day 1-5 | POC-08 sixth one-pager | Audit trail design doc complete |
| Week 11 | 05-11 to 05-17 | POC-09 Day 1-2 | POC-08 seventh one-pager | Monorepo CI green, ruff zero warnings |
| Week 12 | 05-18 to 05-24 | POC-09 Day 3-4 | Mock interview #3 | Portfolio PDF and demo video |

The Week 4 mock interview focuses on RAG engineering and agent loop design, matching the progress on POC-01 and POC-03. The Week 8 mock interview focuses on AWS infra and semantic layer, matching POC-02 / POC-04 / POC-05. The Week 12 mock interview is a full loop simulation, covering system design, client-facing case study, and code review across three segments, matching the accumulation of all POCs.

The cadence has two failure modes to watch out for.

The first is that Week 3 to Week 5, the RAG eval concentration window, tends to overrun, because constructing the eval set is slower than expected. A progress checkpoint must be done at the end of Week 3. If the Q-A golden set has not reached 50 items, cut it down to 30 to protect the schedule.

The second is Week 7's CDK. Because this is a stack John has never touched, reserve half a day specifically for AWS account permissions and bootstrap. These look like they should not block but always do.

The setup of the three mock interviews also needs more detail.

For the Week 4 interview, find a friend with LLM application experience to play a senior engineer. The focus is "what does your Strand agent look like, how do you debug the agent loop, why did you choose these tools".

For the Week 8 interview, find a friend with AWS experience to play a platform engineer. The focus is "AgentCore vs self-deployed Lambda tradeoffs, CDK stack split logic, cost control approach".

For the Week 12 interview, do a full loop simulation. Ideally find two friends to separately play the technical interviewer and the client-facing interviewer. The technical interviewer does a whiteboard system design problem. The client-facing interviewer gives an ambiguous clinical scenario and watches whether John can, in 20 minutes, ask 3 key clarifying questions, draw 1 rough diagram, and produce 1 spoken proposal.

In addition to the mock interviews, embed two external "small checks".

The first is at the end of Week 5: send the POC-03 eval report to a senior engineer with RAG experience for a 30 minute review. The point is to use external eyes to surface blind spots, not to get a "good or bad" rating.

The second is at the end of Week 10: send the POC-07 audit trail design doc to someone with a healthcare compliance background for the same 30 minute review.

The feedback from these two reviews should be recorded verbatim in the weekly journal. It is the most direct source material for answering "how do you incorporate feedback" type questions at onsite.

The 12 weeks also include two interim summaries done in Week 6 and Week 11, called "midpoint status report" and "final portfolio review" respectively.

The midpoint report is a 1-page markdown listing completed POCs, current blockers, and remaining budget allocation. It is mainly for John himself, forcing a step back to evaluate whether the direction is correct.

The final portfolio review is a 5-page PDF covering the artifact index for all 9 POCs, key metrics, and the 5 most important screenshots. This PDF gets sent to the recruiter as supplementary material before the onsite, and a printed copy goes with John to the onsite itself.

Neither summary is new work. Both are "closing actions" on prior work. The Week 6 midpoint report has a hidden function: it forces John to "pause sprinting and look up" at that time point. That action would not happen without an external trigger, but without it the POCs lose cohesion. The Week 11 portfolio review is the final binding of onsite materials. The one-pagers need to be converted to PDF, code demos need to be recorded as GIFs, and architecture sketches need to be transcribed into a proper diagramming tool. Each takes 2 to 3 hours and none can be skipped.

The target state at the end of the 12 weeks is: all 4 Core POCs have runnable code plus a 1-page one-pager plus a 5-minute walkthrough video. All 5 Important / Nice-to-have POCs have at least a demonstrable artifact. The entire monorepo is publicly accessible on GitHub. The portfolio PDF contains 7 client-facing one-pagers.

This state is sufficient to support the Cascadia onsite of 4 rounds, including the two most critical segments (system design and client-facing case study).

If execution drifts significantly, the trigger conditions and contingency plans are as follows.

Scenario one: at the end of Week 5, the POC-03 eval baseline has not run yet. Immediately shrink POC-03 to "retrieval hit@k only, no generation faithfulness", and shift the saved time to POC-02. The onsite will ask "how did you deploy your RAG", not "was your faithfulness 0.83 or 0.85".

Scenario two: at the end of Week 8, POC-04's CDK has not deployed successfully. Downgrade to "write the stack code plus cdk synth to produce a CloudFormation template, do not actually deploy". Cascadia has internal standard CDK templates, and the interviewer cares about the writing pattern, not deployment success.

Scenario three: after Week 10, some interview feedback says "we care most about X" and X is not in this plan. Cut the polish phase of POC-09 and shift the 4 days to a crash course on X.

Beyond the 12 weeks, late May to June needs 1 to 2 weeks reserved for targeted interview-pattern reinforcement: system design whiteboard practice, take-home Python pattern drills, SQL window function review. These do not enter the POC plan but are added as fixed "exam week" actions on John's personal calendar. The ultimate success criterion of the whole gap fill plan is measured by exactly one thing: after the Cascadia onsite, can John land the offer, or even if he does not, can he clearly state in the debrief "which question I did not answer well, and how I would fix it next time". That matters more than getting all 9 POCs to 100 out of 100.
