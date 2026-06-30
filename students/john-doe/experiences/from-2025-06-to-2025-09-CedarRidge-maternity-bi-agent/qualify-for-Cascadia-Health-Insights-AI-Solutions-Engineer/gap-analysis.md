# Gap Analysis, Cedar Ridge Internship vs Cascadia AI Solutions Engineer Role

> This document diagnoses the gap between John Doe's current resume baseline (Cedar Ridge OB SQL reporting internship plus UW M.S. CS coursework in progress) and the Cascadia Health Insights AI Solutions Engineer (New Grad) JD. The goal is an honest audit that feeds the later fill plan and mini POC design. It deliberately does not deliver an optimistic verdict on "can he land this offer." It only describes the gap.

## 1. Relevance diagnosis

Slicing the Cedar Ridge internship against the Cascadia JD's required capability stack, the conclusion sorts into three buckets.

The directly relevant slice is small. SQL writing (including Snowflake table structure familiarity), exposure to healthcare operations context (OB ward, charge nurse, postpartum length of stay, high-risk maternal flags are all clinical operations topics that recur in Cascadia client scenarios), and the experience of aligning definitions with a senior analyst (Hannah's SQL review) are the three directly relevant pieces. SQL plus Snowflake hits a JD Required item, and the clinical operations metric context counts as indirect industry background outside of FHIR/HL7.

The indirectly relevant slice. The Excel reporting workflow means John has seen the downstream link of "what the requester actually does with the data after they get it," which gives him a small mental warm-up for future UAT (User Acceptance Testing) but is not an engineering skill. The "willing to ask questions" feedback signals weak evidence of ambiguity tolerance, but there is no falsifiable artifact behind it.

The not-relevant slice is large. LLM Agent, RAG, AWS Bedrock / AgentCore, CDK, semantic layer YAML design, HIPAA/TJC/SOC 2 audit trail documentation, and client-facing long-form documentation are six blocks that simply did not appear in the Cedar Ridge work. In those three months John never wrote a line of LLM API code, never touched an AWS deployment, and never wrote a single piece of external documentation.

Adding the three buckets together, overall relevance lands at **weak**. Cedar Ridge gave John the label of "healthcare SQL intern," which is still one full technical stack and one full client delivery cycle short of the "AI Solutions Engineer" the Cascadia JD asks for. Without elevation work, the signal this experience sends to a Cascadia interviewer is roughly "he can write SQL, he has seen Snowflake, he knows a little healthcare vocabulary," and nothing more. That is not enough to carry any single Strongly Preferred line from the JD.

---

## 2. Gap breakdown overview

The table below lays out all 9 gaps by severity and serves as the master index for the later fill plan.

| # | Gap | Severity | JD evidence | John's current state | What is missing | 3-month closability |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | LLM agent framework (Strand Agents) | Core | "hands-on experience with at least one LLM application framework. Strand Agents, LangChain, LlamaIndex..." | Zero exposure, ML/NLP class still in plan, not started | A from-scratch agent project plus evaluation data | Closable to "can explain clearly plus has a code sample" |
| 2 | AWS Bedrock AgentCore Runtime | Core | "Deploy and operate the agent infrastructure on AWS (Bedrock, AgentCore Runtime, Lambda, ECS...)" | Only course-level Lambda exposure plus Cloud Practitioner cert | Hands-on Bedrock model invoke plus AgentCore deployment path | Closable to a working demo, not production grade |
| 3 | RAG implementation plus evaluation harness | Core | "experience with at least one retrieval-augmented-generation (RAG) implementation including evaluation. We will probe how you measured RAG quality." | Zero exposure | A full end-to-end path from chunking to retrieval to eval metric | Closable to "can explain methodology plus has data" |
| 4 | AWS CDK (Python) | Core | "Familiarity with AWS CDK (Python) is a meaningful plus because every Cascadia deployment ships through CDK." | Only AWS console plus snippets of CloudFormation | One real CDK-deployed stack | Closable to entry level |
| 5 | Semantic layer YAML design | Important | "codify their internal metric definitions into the semantic layer YAML. Resolve definitional conflicts before they become production confusion." | Has written SQL but never designed a semantic layer | The engineering mindset to abstract 15 SQL queries into a metric definition set | Closable to a presentable YAML sample |
| 6 | FHIR / HL7 healthcare data formats | Important | "Preferred: prior exposure to healthcare data formats (FHIR, HL7) or comparable regulated data environments" | Zero exposure | Basic read/write of FHIR Patient / Encounter / Observation resources | Closable to "can explain and has a working demo" |
| 7 | HIPAA + TJC + SOC 2 audit trail | Important | "Author audit-trail and compliance documentation aligned to HIPAA, TJC hand-off communication standards..." | Zero exposure | An audit trail design doc plus code that actually emits audit logs | Closable to "knows the vocabulary plus has written one template" |
| 8 | Client-facing written communication | Important | "roughly ten to fifteen pages of client-facing documentation per quarter; this is not negotiable." | Zero exposure, only internal SQL reports | A 10-page-scale client-facing documentation sample | Closable to a presentable sample |
| 9 | Production-grade Python code quality | Nice-to-have | "strong Python proficiency including at least one production-grade or production-adjacent codebase. We will ask to discuss a code sample" | Course level plus 2 or 3 small projects | Engineering practices like type hints, tests, CI, modularization, logging | Closes alongside Gaps 1 through 4 |

---

## 3. Core gaps deep dive

The 4 gaps below are the hard technical threshold of the Cascadia JD. Missing any one of them blocks the technical phone screen.

### 3.1 Core Gap 1, LLM agent framework (Strand Agents)

The JD's "Education, Experience" section explicitly lists as Strongly Preferred: "hands-on experience with at least one LLM application framework. Strand Agents, LangChain, LlamaIndex, Haystack, or comparable. We use Strand Agents in production." Landscape 03-role §3 further notes that 35 to 45 percent of the week is spent writing code, and agent configuration sits at the center of that. In other words this is not a "nice to have seen it" bonus item, it is the carrier of about 40 percent of the daily workload.

Why this matters for this role specifically. Cascadia's Insight Assistant is itself an agent product, so an AI Solutions Engineer's daily work is not "using an agent," it is "customizing agent behavior for client scenarios." That requires the engineer to read framework source code level abstractions, distinguish a tool call from a plain prompt, and debug where a multi-step reasoning chain gets stuck. Having only seen a ChatGPT API call does not count.

John's current state is zero exposure. NLP sits in his M.S. CS plan but has not started, and the Cedar Ridge internship touched no LLM API. Even for LangChain, the most mainstream framework, he has no code sample he can discuss.

What is missing is a complete agent project: defining tools, managing prompts, handling multi-turn conversation state, and tracing the call chain. Since Cascadia runs Strand Agents in production, the demo project should pick either Strand or LangChain and go deep on one, not do half of each. The closure marker is John being able to spend 60 minutes in a technical phone screen explaining "why I chose this tool abstraction, what cases trip the agent, how I diagnose it." Supporting artifacts should include a runnable agent code repo, a conversation log covering at least 20 cases, and a design note explaining "why I gave this tool this schema."

### 3.2 Core Gap 2, AWS Bedrock AgentCore Runtime

The JD's Accountability section uses the verb phrase "Deploy and operate the agent infrastructure on AWS (Bedrock, AgentCore Runtime, Lambda, ECS, CloudWatch)." Landscape 03-role §3 also identifies CDK plus AWS deployment as another major coding block. AgentCore Runtime is the managed Bedrock service dedicated to running agents. It launched in late 2024, so the whole market is new to it, but Cascadia is already using it, which means interviewers will ask.

John currently has an AWS Cloud Practitioner cert and a little Lambda exposure from coursework, which only counts as "has heard of AWS." Bedrock and AgentCore he has not touched at all.

What is missing is a hands-on path of "invoke Claude / Nova on Bedrock plus deploy an agent via AgentCore plus read logs in CloudWatch." Closing to entry level means John can explain "what is different between Bedrock and calling the Anthropic API directly, what AgentCore saves you compared to running your own ECS." Full production-grade closure (fine-grained IAM, cross-account, KMS encryption, VPC endpoint) is unrealistic for a New Grad, not doable in 3 months, and not needed.

Worth noting: AgentCore Runtime only went GA in late 2024, so very few engineers in the market have hands-on experience with it. That is actually John's opportunity. Two weeks of reading official docs plus running a working demo can pull this item from "no clue" to "stronger than the average New Grad" in interviews. Early investment in this kind of emerging technology has the best return on effort.

### 3.3 Core Gap 3, RAG implementation plus evaluation harness

The JD is emphatic on this: "experience with at least one retrieval-augmented-generation (RAG) implementation including evaluation. We will probe how you measured RAG quality." Note the phrase "We will probe." It means the interviewer will actively follow up with "how do you measure retrieval recall, how do you measure generation faithfulness, where does the ground truth come from." Landscape 03-role §4 lists the evaluation harness as one of the four major deliverables.

John currently has zero exposure. Writing SQL reports never touches vector retrieval, embedding selection, or chunking strategy.

What is missing is an end-to-end RAG project including: corpus (ideally healthcare text, e.g. ACOG clinical guidelines), embedding model choice, chunking strategy, retrieval top-k, generation prompt, evaluation harness (covering retrieval recall@k, generation faithfulness, optionally LLM-as-judge). The closure marker is John being able to show an evaluation results table in an interview and explain "why moving chunk size from 512 to 256 raised recall but dropped faithfulness."

The evaluation harness piece is where Cascadia leans harder than a typical LLM application company. The JD's threatening phrasing "We will probe how you measured" means the interviewer will throw a number back at him: "how exactly did you compute that 0.78 recall, how big is the ground truth set, is there a held-out test set." John has to be prepared to answer with specific numbers on the spot.

### 3.4 Core Gap 4, AWS CDK (Python)

The JD line "every Cascadia deployment ships through CDK" carries more weight than a typical Preferred bullet. Landscape 03-role §3 also lists CDK as part of the daily code stack. Cascadia has many clients, many deployments, and every one of them spins up via a CDK template, which means a new hire is reading CDK code in week one.

John has only seen the AWS console and bits of CloudFormation YAML. He has never written CDK.

What is missing is a runnable CDK Python stack: define the IAM role required for a Bedrock call, a Lambda, an S3 bucket, and a CloudWatch log group, then actually deploy it to his own AWS account with `cdk deploy`. The closure marker is John explaining "at what abstraction layer CDK sits above CloudFormation, the difference between Construct vs Stack vs App." Depth is not required (no custom L3 construct, no CDK Pipelines). Entry level is enough.

Since CDK is in Python, and John's own Python also needs elevation (Gap 9), the CDK project naturally becomes the vehicle for Python engineering practices. If the fill plan writes the CDK project to production-grade Python standards (type hints, pytest, CI), it serves both purposes at once.

---

## 4. Important gaps deep dive

The 4 items below are JD Strongly Preferred and Preferred bullets that interviewers will probe. These gaps will not directly kill the resume, but they determine whether the interviewer reads John as "a new hire we can teach" or "a new hire who came totally unprepared."

### 4.1 Important Gap 5, Semantic layer YAML design

The JD's third Accountability bullet: "Partner with each client's senior clinical analyst to codify their internal metric definitions into the semantic layer YAML. Resolve definitional conflicts before they become production confusion." Landscape 03-role §3 points out that semantic YAML is one of the core deliverables.

John wrote 15 SQL queries at Cedar Ridge, each one corresponding to a metric (bed availability, nurse staffing, postpartum length of stay, etc.), but he never abstracted those metrics into a semantic layer definition consumable by an agent. In other words he did "querying" but not "definition engineering."

What is missing is the engineering instinct to abstract SQL into a metric definition: metric name, business definition (one sentence a charge nurse can understand), SQL expression, grain (per patient / encounter / day), filter, dependency, owner. The closure marker is John producing a YAML file covering the 15 Cedar Ridge SQL queries and being able to explain "if the charge nurse's LOS definition disagrees with the doctor's LOS definition, how would I encode that conflict in the YAML rather than burying it."

The special advantage of this item is that it strongly leverages the contextual edge of the Cedar Ridge experience. Other candidates doing a semantic layer project can only use synthetic data, while John can say "I wrote 15 SQL queries like this at Cedar Ridge, now I'm formalizing them." That is the most natural story arc available in the interview, and the fill plan should amplify this angle first.

### 4.2 Important Gap 6, FHIR / HL7 healthcare data formats

The JD lists as Preferred: "prior exposure to healthcare data formats (FHIR, HL7)." Landscape 01-industry §5 notes that the 2020 ONC interoperability rule mandated FHIR adoption, so the default data format across the industry is FHIR. When Cascadia's Insight Assistant processes clinical data, it most likely uses FHIR Resources.

At Cedar Ridge, John was looking at already-normalized OLAP tables in Snowflake. He never touched raw FHIR JSON.

What is missing is basic understanding of the structure of the core FHIR Resources (Patient, Encounter, Observation, Condition, MedicationRequest), plus awareness that HL7 v2 exists (without going deep, he should at least be able to say "I know HL7 v2 is pipe-delimited text mainly used by legacy systems, and FHIR is the RESTful plus JSON next generation"). The closure marker is John being able to say in a client-facing case study interview "for this query I would pull length-of-stay from the Encounter resource and compute age from Patient.birthDate."

In practice, Synthea (an open source FHIR synthetic data generator) can produce thousands of patient bundles in 5 minutes, and the FHIR learning curve is relatively gentle. This item should not be a bottleneck for the fill plan. One week is enough.

### 4.3 Important Gap 7, HIPAA + TJC + SOC 2 audit trail design

The JD's sixth Accountability bullet: "Author audit-trail and compliance documentation aligned to HIPAA, TJC hand-off communication standards, and client-specific governance requirements." Landscape 03-role §3 notes client documentation (including audit and HIPAA explanations) takes 10 to 15 percent of working time.

John currently has zero compliance exposure. The Cedar Ridge internship did not involve compliance design, and the NDA he signed was only about access rights.

What is missing is the design vocabulary of "what an audit trail should actually record in the context of an LLM agent": each agent call should log user identifier, timestamp, prompt, retrieved chunks, tool calls, final response, whether PHI was touched, and retention policy. TJC handoff standards (the compliance requirements around clinical handover communication) do not require depth from a New Grad, but he needs to know the vocabulary. The closure marker is John producing a 1 to 2 page audit trail design spec and being able to explain "why PHI redaction has to happen before the log write, not after."

The peculiarity of the compliance item is that depth is adjustable. A new hire does not need to know HIPAA statute text. He only needs to know the small portion an engineer can actually implement (log design, PHI redaction, retention, access control). The fill plan should not let John fall into the rabbit hole of reading HIPAA legal text. Cap it at 10 to 15 hours of investment.

### 4.4 Important Gap 8, Client-facing written communication

The JD writes this one as Required, not Preferred: "roughly ten to fifteen pages of client-facing documentation per quarter; this is not negotiable." Landscape 03-role §3 and §4 both list client docs as one of the four major deliverables.

So far the things John has written are: course assignment PDFs, SQL comments, and the internal Excel reports for Hannah. He has never written a long document for a non-technical reader (charge nurse, compliance officer).

What is missing is the ability to "translate technical decisions into language a clinical operations stakeholder can read." That includes structure (background, goal, plan, risk, sign-off), tone (neither overdeferential nor showing off), and visuals (mermaid flow diagrams, tables, a glossary of key terms). The closure marker is John having an 8 to 10 page sample post-deployment write-up in his portfolio, ready to show in an on-site case study.

One reminder: Cascadia's on-site case study round will most likely ask John to write a paragraph live for the client compliance officer, not just present a pre-written document. So a portfolio piece alone is not enough. He needs to turn client-facing writing into muscle memory. The fill plan should schedule him a 1 to 2 page client-oriented short piece each week as practice, not just produce one big doc and call it done.

---

## 5. Nice-to-have gaps deep dive

### 5.1 Nice-to-have Gap 9, Production-grade Python code quality

The JD says "strong Python proficiency including at least one production-grade or production-adjacent codebase. We will ask to discuss a code sample during the technical loop." This is nominally Required, but because it ships alongside the projects from Gaps 1 through 4, it sits in Nice-to-have (it does not need its own dedicated project).

John's Python today is at the course level plus 2 or 3 small projects. That means he has probably not systematically used type hints, pytest, CI, logging, modularization, dependency management (uv / poetry), or a Makefile.

What is missing is upgrading the Gap 1 through 4 projects from "it runs" to "I can show this to someone." The closure marker is at least one repo on John's GitHub that meets: clean README, full type-hint coverage, pytest with at least 3 tests, logging configured, CI passing, locked dependencies. These are the things a Cascadia interviewer will reflexively scan in the "discuss a code sample" round.

A trap to avoid: do not over-abstract just to look "engineered" (e.g., splitting a 200 line demo into 10 modules with assorted abstract base classes). Interviewers see through this kind of over-engineering immediately, and it actually loses points. A reasonable bar is "if a coworker came to review this repo, they could grok the directory layout in 10 minutes."

---

## 6. Cross-gap entanglement

The nine gaps are not isolated. Several strong couplings run between them, and the fill plan's project design should exploit those couplings for double duty, not run 9 separate projects.

LLM agent framework (Gap 1) and RAG (Gap 3) naturally bind together. Once an agent project hooks up a retrieval tool, the evaluation harness needs to cover both agent behavior and retrieval quality. These two gaps share a single project skeleton most cleanly.

Bedrock/AgentCore (Gap 2) and CDK (Gap 4) are two links in the same deployment chain. CDK code is exactly what spins up Bedrock / Lambda / IAM role infrastructure, so a single CDK stack covers both gaps.

FHIR (Gap 6) and production-grade Python (Gap 9) also couple. FHIR resources are deeply nested JSON, and parsing them cleanly, validating them, and converting them to dataclasses requires pydantic, type hints, and unit tests. In other words, picking FHIR as the RAG corpus source drives both Gap 6 and Gap 9 simultaneously.

Semantic layer YAML (Gap 5) and audit trail (Gap 7) have an indirect coupling. Both are different faces of "using YAML / JSON schema to formalize business rules." A single sample project that produces both a metric YAML and an audit log schema YAML shows "John knows how to capture business rules in declarative artifacts" as an engineering taste.

Client-facing documentation (Gap 8) is the final wrapper. Any one of the Gap 1 through 7 projects needs an external-facing write-up to close it out. Writing that explanation in client-facing style (background, plan, risk, compliance) closes Gap 8 in the same motion. That means Gap 8 does not need a standalone project, just an extra 1 to 2 days at the end of each other project to write a deliverable.

Putting the couplings together, the 9 gaps can actually converge into 2 to 3 organic mini POCs: a "Strand Agent plus RAG over FHIR plus eval harness" project (covering 1, 3, 6, 9), a "CDK plus Bedrock/AgentCore deployment" project (covering 2, 4), and a "Cedar Ridge 15 SQL abstracted into semantic YAML plus audit trail spec plus client-facing write-up" project (covering 5, 7, 8). That is the implicit suggestion for the fill plan.

Going one step further, the three POCs themselves string together into a coherent story. POC 1 is a "technical capability demo" (I can build a RAG agent from scratch), POC 2 is an "engineering delivery demo" (I can deploy the demo to the cloud), POC 3 is a "business capability demo" (I can engineer clinical metrics into formal definitions). Together they answer what the JD wants to see: the "technical plus engineering plus business" trinity. That is the natural narrative arc for talking through projects in the interview.

---

## 7. Diagnostic conclusion

Wrapping up the diagnosis from the first 6 sections.

John's Cedar Ridge experience today is **conditionally workable** for the Cascadia AI Solutions Engineer role, not unconditionally workable.

Why it is unconditionally not workable as-is: all 4 Core gaps are missing, which means even if a phone screen tosses him an open question like "tell me about an LLM project you've done," he has nothing to talk about. This is not a resume polish problem. It requires project output.

Why it is conditionally workable: the three foundations Cedar Ridge left are real. 1) SQL plus Snowflake hits a JD Required item, 2) the OB ward healthcare context is reusable industry background, 3) the "willing to ask questions" feedback is weak but pointed in the right direction. These three foundations, paired with 3 months of targeted elevation on the 9 gaps, can move the whole picture from "weak" to "submittable and phone-screen-ready."

The conditions are listed below.

- He must deliver 2 to 3 mini POCs in 3 months (merged per the §6 coupling), not 9 isolated little exercises.
- He must have at least one 8 page client-facing sample doc in the portfolio, otherwise the Required written communication item does not pass.
- The Cedar Ridge experience itself has to be reframed from "I wrote 15 SQL queries" into "I abstracted 15 OB metrics into a semantic layer plus piloted a natural language BI agent plus deployed it to Bedrock." This reframing is what fill plan 02 will do, and this document does not expand on it.
- John must accept one fact: what is achievable in 3 months is "can explain clearly, has code samples," not "production grade." Interviewers distinguish between New Grads and people with 2 years of experience, and will not hold him to production grade. But among New Grads they will compare "whose project is more complete, who explains more clearly." Quality matters more than quantity.

If John does not accept these conditions (e.g., insufficient time, unwilling to do projects, hoping to bluff via resume packaging), the diagnosis falls back to **not workable**. He should then demote the Cascadia track to a stretch goal and put his main line on roles that do not require LLM experience.

---

## 8. Handoff notes for the fill plan

The items below are the constraints and priorities the author of execution-plan.md should carry into the next document.

Priority ordering: do Gaps 1 + 3 + 6 + 9 first (merged into one "RAG over FHIR agent" project), because that is the topic an 80 percent probability Cascadia phone screen will hit, and missing it fails directly. Next do Gaps 5 + 7 + 8 (merged into one Cedar Ridge elevation project), because that is the portfolio shown in the on-site case study round. Last do Gaps 2 + 4 (merged into a CDK deployment project), lowest priority but also lowest closure cost, demo-level achievable in 1 to 2 weeks.

Time budget constraint: John is a UW M.S. CS student in progress, and over the same 3 months has to keep the course schedule running (ML and NLP are in plan, and Spring 2026 semester will eat a portion of the time). The fill plan cannot assume 8 hours a day. Plan against a realistic 15 to 20 hours per week.

Technology stack lock: Strand Agents is Cascadia's production framework, so pick Strand, not LangChain. Even though LangChain has more material, pick Strand. For models on Bedrock, pick Claude (Cascadia most likely runs that family), not Nova or Llama.

Budget constraint: AWS Bedrock calls are not cheap, and AgentCore Runtime is also usage-billed. The fill plan should design projects to keep token usage under USD 100. CDK deployment is essentially free as long as you remember `cdk destroy`.

Things not to do: do not pad with a production-grade HIPAA implementation (a New Grad cannot do it and does not need to), do not pick FHIR scenarios that are too advanced (Bulk FHIR, SMART on FHIR OAuth integration), do not try to imitate real PHI data (high compliance risk, use a synthetic FHIR dataset like Synthea instead).

Documentation is the end state, not the process. At the end of each mini POC, produce a client-facing write-up (8 to 10 pages). That document itself is the portfolio. Do not wait until all three POCs are finished to batch-write the docs at the end, because memory will have faded by then.

A last word for the fill plan author: this document locks the diagnosis at "weak but workable." If during fill plan execution it becomes clear that John's actual time investment is not enough, or some Core gap is not closing as expected, go back and edit the §7 condition list rather than letting the fill plan run on empty. The feedback loop between diagnosis and execution has to stay open.
