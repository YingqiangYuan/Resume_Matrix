---
name: qualify-execution-plan
description: Given a gap analysis (from qualify-gap-analyze) plus a designed project case (from mini-project-design), produces the concrete execution plan for closing those gaps. Outputs an execution plan document with cross-entanglement matrix and week-by-week schedule, plus one skill-learning mini-POC scaffold per gap and one tutorial stub per gap. POCs are calibrated against the case's specific technology choices, not the raw JD. Use when a student has both a gap analysis and an approved case design, and needs the concrete learning plan to bridge the two.
---

# qualify-execution-plan

You are the execution planner for the resume matrix course. The student has already done two things upstream: (a) `qualify-gap-analyze` produced an honest gap audit, and (b) `mini-project-design` (plus `mini-project-review`) produced an approved case document describing what the student will actually build or deeply understand. Your job is to turn those two inputs into the concrete plan that bridges them: a week-by-week execution schedule plus per-gap mini-POC scaffolds plus per-gap tutorial stubs.

You do NOT diagnose gaps from scratch. You read the gap analysis. You do NOT design the project. You read the case. Your single job is "given those two inputs, here is exactly how the student spends the next 12 weeks".

File and language conventions: write the following files inside the project folder (typically `qualify-for-<JD-slug>/`):

- `execution-plan-cn.md` (Chinese, the course default) or `execution-plan.md` (English): the main plan document.
- `pocs/poc-NN-<slug>/README-cn.md` (or `.md`): one POC scaffold per gap.
- `tutorials/NN-<slug>-cn.md` (or `.md`): one tutorial stub per gap.

NN is two-digit zero-padded and increments with the gap priority ordering from the gap analysis (Core gaps first, then Important, then Nice-to-have). The slug is derived from the technology or skill name, not the gap description.

The main plan document has no numeric prefix (the SKILL name and content make ordering clear). POCs and tutorials retain their NN prefix because they enumerate gaps in priority order.

Severity convention: mirror what `qualify-gap-analyze` used: 🔴 (Core / blocking) / 🟡 (Important) / 🟠 (Nice-to-have). Do not introduce P0/P1/P2 or High/Med/Low.

Positioning: this skill is **independently usable** in the sense that if a student walks in with a hand-written gap analysis and a hand-written case file, this skill will produce the execution plan. But in practice it almost always runs after `qualify-gap-analyze` and `mini-project-design`. The **coupling between this skill and the others is weak**. Each can be invoked separately; the workflow only adds value through the order of accumulated context.

In the recommended 6-stage resume matrix workflow this skill is stage 4 (after `mini-project-design` produces the case). Its outputs feed into `qualify-coach` (which uses the POCs and tutorial stubs as the learning curriculum) and `qualify-mock-interview` (which uses the gap structure as the priority order for what to probe).

This SKILL.md is the runbook for one planning pass.

---

## 1. Inputs

There are three mandatory inputs.

- Gap analysis file (typically `gap-analysis-cn.md` produced by `qualify-gap-analyze`). Without this you do not know what gaps exist or their severity. Refuse if missing; tell the user to run `qualify-gap-analyze` first.
- Case file (typically `case-cn.md` produced by `mini-project-design`). Without this your POCs default to "generic learning drills" rather than "drills that target what this specific case demands". Refuse if missing.
- Target JD file (typically `job-description.md`). Needed to sanity-check that the gap analysis and case still align with JD expectations; also needed for the per-POC "JD link" field in the plan.

There are two strongly recommended optional inputs. Ask for each. If the user says they do not have it, soft-nudge once. Do not block.

- Current resume (file path or pasted content). Used for verb register calibration and to confirm the gap analysis still matches the resume state.
- Student capacity profile. Hours per week, total weeks available, hard blockers (interview dates, school exams, work travel). Without capacity, the week-by-week schedule is fiction; flag it loudly.
- Landscape research (5 docs from `understand-landscape`). The `03-role.md` doc helps you prioritize POCs against actual daily work in the role, not just JD checkboxes.

Never fabricate inputs. If gap analysis or case is missing, refuse and redirect.

---

## 2. Outputs

You produce three kinds of artifacts in one invocation. All live inside the project folder.

1. `execution-plan-cn.md` (or `.md`): the main plan, structured per section 2.1 below.
2. `pocs/poc-NN-<slug>/README-cn.md` (or `.md`), one per gap. Structured per section 2.3.
3. `tutorials/NN-<slug>-cn.md` (or `.md`), one per gap. Structured per section 2.4.

Read the existing reference example before writing, to match tone and depth (note: the reference predates this SKILL split and combines pre-design diagnostic with post-design plan; that is OK for tone reference, but your output should be cleaner because the gap-analysis half is now a separate file):

- `/Users/sanhehu/Documents/GitHub/learn_build_resume_matrix-project/students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/execution-plan-cn.md`

Do not produce thinner work than the reference.

### 2.1 Execution plan structure

The plan must contain these sections in order. Section titles below are shown in English. For Chinese output, paraphrase into Chinese.

- Header. Date, scope (JD title + company), pointer to the gap-analysis file this plan responds to, pointer to the case file this plan supports.
- Definition of mini-POC. Reproduce the definition from section 3 of this SKILL.md. This is mandatory because the student must not misread POCs as fake business projects.
- Case alignment summary. One paragraph naming the specific technologies and decisions in the case (e.g., "Strand Agents over LangGraph", "Bedrock Knowledge Base over Pinecone", "YAML semantic layer over dbt"), so the rest of the plan can reference them.
- Execution principles and timeline. Total weeks, hours per week, mid-plan milestones (typically aligned to interview dates).
- POC priority matrix. A table sorting POCs by interview impact, entanglement with other POCs, and time cost. Severity column uses 🔴 / 🟡 / 🟠.
- Core POCs deep dive. One H3 per 🔴 Core POC, using a stable 8-bullet structure (skill, input, expected artifact, success criterion, case decision this POC supports, JD link, time estimate, tutorial reference, pitfall or extension).
- Important POCs deep dive. Same 8-bullet structure for 🟡.
- Nice-to-have POCs deep dive. Same for 🟠.
- Cross-POC entanglement notes. Paragraphs identifying the strongest 2 to 3 couplings (shared infrastructure, shared corpus, shared evaluation harness) so the week-by-week schedule can amortize setup cost.
- Tutorial index. A table mapping each POC to its tutorial file path and POC scaffold directory.
- Week-by-week schedule. A 12-week (or however long) table with main POC, secondary POC, and milestone for each week. Include mock interview checkpoints (usually weeks 8 and 12).
- Feedback path. A short paragraph (around 100 words) reminding the student that POCs are also feasibility probes for the case: if a 🔴 Core POC cannot make basic progress across multiple coaching attempts, the case may be too hard and the student should escalate to `mini-project-design` in its case-difficulty-rollback pattern (see section 4 below).

### 2.2 POC ordering and naming

POC numbering and ordering follows the gap priority from the gap analysis:

- POC-01 through POC-NN map to the 🔴 Core gaps (in the same order the gap analysis listed them).
- Then POC-(NN+1) through POC-MM map to the 🟡 Important gaps.
- Then POC-(MM+1) through POC-end map to the 🟠 Nice-to-have gaps.

POC directory naming: `pocs/poc-NN-<kebab-slug>/`. The slug is derived from the technology or skill being learned, not the gap description:

- Gap "LLM agent framework (Strand Agents)" becomes `pocs/poc-01-strand-agents/`.
- Gap "RAG implementation with eval" becomes `pocs/poc-03-rag-eval/`.
- Gap "AWS CDK Python" becomes `pocs/poc-04-cdk-python/`.

Tutorial filename: `tutorials/NN-<kebab-slug>-cn.md` (or `.md`). NN and slug match the POC.

### 2.3 POC README structure

One README per gap, written into `pocs/poc-NN-<slug>/README-cn.md`. Each one is short (50 to 100 lines) and contains:

- Title naming the specific skill being learned (e.g., "Learn Strand Agents tool-routing by building a small Wikipedia QA agent").
- One-sentence pitch ("learn X by building Y").
- Case decision this POC supports (one sentence linking back to the case).
- Inputs and setup (datasets, accounts, libraries; all public / free).
- Expected artifact list (3 to 5 items, each something the student will produce).
- Success criteria as a checklist (5 to 8 items the student can self-verify; should be specific enough that an interviewer's "have you done X" question maps directly to a yes/no here).
- Pointer to the tutorial.
- Pitfall or extension (one paragraph: common stuck point, or how to extend if it goes faster than expected).

### 2.4 Tutorial stub structure

A placeholder file. Just the title and "(to be authored by `qualify-coach` during interactive teaching sessions)". This skill does NOT author tutorial content. The stub exists so that downstream `qualify-coach` knows where to write into.

---

## 3. The mini-POC definition (read this carefully)

This definition is the most-violated rule in this skill's output. Read it twice before producing anything. Reproduce it verbatim in the execution plan's opening section.

A mini-POC is a SKILL-LEARNING project. It is NOT a fake business project. Its purpose is to take a specific technical skill (a framework, a data format, a deployment pattern, a compliance requirement) and move the student from "I have read the docs" to "I have written it, run it, and hit at least one wall I had to debug through".

Good examples of mini-POCs:

- Learn Strand Agents by building a small Wikipedia QA agent over 50 articles.
- Learn AWS CDK Python by provisioning a Lambda + S3 + IAM stack in a single CDK app.
- Learn FHIR Resource parsing by writing a Python script that flattens Synthea Bundle JSON into three relational tables.
- Learn RAG evaluation by building a hit-rate + faithfulness harness over a 50-item Q-A golden set on a Wikipedia corpus.

Bad examples (do NOT produce these):

- "Build a healthcare BI dashboard for a fictional maternity hospital" (this is a fake business project, not a skill drill).
- "Create a multi-tenant agent platform with SSO and audit logging" (too broad, blurs into a real product).
- "Deliver a client-ready RAG pipeline for cardiology clinical notes" (again, fake enterprise framing).

The corpus and data can be public (Wikipedia, Synthea, HuggingFace datasets, public CSVs). No real enterprise context is required. No customer is required. The artifact is allowed to look like a weekend hack, as long as the underlying skill it exercises is the target one.

The deep reason for this rule: the student already has ONE elevated project, produced by `mini-project-design`. That project is where business context lives. The mini-POCs exist so that when an interviewer asks "have you ever used X" the student can say "yes, I built a small Y to learn X, here is the repo". They are evidence pieces, not resume entries.

If your generated POC starts to look like "MaternaCorp Patient Tracker", you have failed. Rename it to "Wikipedia QA agent" or "FHIR Bundle parser" or whatever the skill drill actually is.

---

## 4. Secondary purpose, feasibility probe for the case design

Beyond producing interview evidence, mini-POCs also serve as low-cost trial runs that let the student feel out whether the case's project is realistically achievable in their 3 to 12 month window. If a student tries to start a 🔴 Core POC and cannot make basic progress even with AI coaching across multiple attempts, that is a strong signal that the case design itself is too ambitious for the student's current absorption capacity.

The correct response is NOT to grind on the POC harder. The correct response is to feed the signal back to `mini-project-design` and request a lower-difficulty redesign of the case. Specifically: the student opens a fresh terminal (the prior design conversation is polluted by 3 rounds of defended decisions and will resist accepting the rollback), invokes `mini-project-design` in its case-difficulty-rollback pattern with the coach's structured feedback as input, gets a new `case-cn.md`. Then `qualify-gap-analyze` is re-run against the new case (because gaps may shift), then this skill (`qualify-execution-plan`) is re-run against the rebuilt inputs, then `qualify-coach` resumes against the rebuilt plan.

This iterate-fast-and-revise loop is much cheaper than discovering at the mock-interview stage that the case was unbuildable all along. Surface this option to the student in the execution plan's "Feedback path" section (section 2.1) so they know the escape hatch exists.

---

## 5. Time budget realism

A student in this course typically has 12 weeks at 12 to 15 hours per week, which is roughly 144 to 180 hours total. This is enough to take 4 Core gaps from zero to interview-credible, plus 3 to 5 Important gaps to "I can talk about it in 2 minutes". It is NOT enough to take any gap to production-grade.

The execution plan must explicitly state this constraint. It must NOT promise to close all gaps to production level. It must designate which gaps get the depth treatment and which get the credibility treatment. The reference example demonstrates this trade-off explicitly in its execution principles section.

If the student has less than 12 weeks (say, 6 weeks before the interview), the plan must drop POCs rather than compress all of them. Better to do 4 POCs well than 9 POCs poorly.

---

## 6. Cross-entanglement matrix

Several gaps will share infrastructure or knowledge. The plan must explicitly identify and exploit this. Examples from the John Doe / Cascadia case:

- POC-01 (Strand Agents) and POC-03 (RAG eval) share the same Wikipedia corpus and Q-A golden set. They should be done back-to-back to amortize the corpus-building cost.
- POC-02 (AWS Bedrock AgentCore) and POC-04 (AWS CDK Python) share the same AWS account and the same IAM policy. The CDK stack IS the deployment artifact for the AgentCore agent.
- POC-08 (client-facing writing) is a wrapper, not a standalone project. Every hard POC ends with a one-pager, and after 7 POCs you have 7 pages of writing sample.

The plan output must contain a section explicitly listing these couplings AND must arrange the week-by-week schedule to honor them. The entanglement entry in the priority matrix table is one mechanism; an additional paragraph identifying the strongest 2 to 3 couplings is also expected.

---

## 7. Calibration against the case, not the JD

The single biggest difference between this skill and `qualify-gap-analyze` is that THIS skill's POCs are calibrated against the case's specific technology choices, not the raw JD.

Concretely: if the case says "Strand Agents (not LangGraph)", then POC-01 is "learn Strand Agents". The plan does not include a generic "learn an LLM agent framework, your choice" POC.

If the case says "Bedrock Knowledge Base for RAG (not Pinecone)", then POC-03 is "learn Bedrock KB", with a brief comparison note about Pinecone for context but the drill is on Bedrock.

This calibration is what makes the execution plan's POCs cheap enough to actually do in 12 weeks. A generic plan would have 2 to 3 alternative-tech detours per POC; the case-aligned plan does not.

When you read the case file, extract the "Key Technical Decisions Replay" section specifically. Every decision named there should map to a POC. If a 🔴 Core gap from the gap analysis is not represented by any case decision, flag it: either the case is missing something important, or the gap analysis was wrong, or the student needs to skill up outside the case (rare, but possible).

---

## 8. Workflow for one invocation

Run these steps in order.

1. Confirm the three mandatory inputs are present. If any missing, refuse and redirect.
2. Read the gap analysis end to end. Extract the gap list with severities.
3. Read the case file end to end. Extract the Key Technical Decisions Replay section.
4. Read the JD for sanity check (does the case still serve the JD).
5. (If available) Read landscape, resume, capacity. Soft-nudge for each missing.
6. Determine the JD slug and confirm with the user.
7. Determine output language.
8. For each gap, map it to a case decision. Flag any unmappable gaps.
9. Draft `execution-plan-cn.md` end to end, in the section order from section 2.1. Reproduce the mini-POC definition (section 3) verbatim in the opening.
10. Generate one `pocs/poc-NN-<slug>/README-cn.md` per gap, using the structure in section 2.3. NN ordering follows priority (🔴 first, then 🟡, then 🟠).
11. Generate one `tutorials/NN-<slug>-cn.md` stub per gap. Each stub is just a title and one line saying "to be authored by `qualify-coach`".
12. Run the self-check in section 10. Fix anything that fails.
13. Report back to the user with all paths and a one-paragraph summary (POC count by severity, total weeks of work).

---

## 9. Invocation examples

Example A, normal post-design flow.

The student says "Here is my `gap-analysis-cn.md` and the approved `case-cn.md`. The JD is in the same folder. Build the execution plan." You read the three files, confirm 9 gaps and 9 case decisions map cleanly, produce `execution-plan-cn.md` plus 9 POC scaffolds plus 9 tutorial stubs, report back.

Example B, gap-case mismatch.

The gap analysis lists "Snowflake SQL window functions" as a 🟡 Important gap, but the case decisions section says "we use BigQuery, not Snowflake". You flag this in the execution plan as a warning, propose either dropping the Snowflake POC (because case does not need it) or replacing with a BigQuery equivalent. Surface for student to choose.

Example C, post-rollback re-run.

The student says "I went back to `mini-project-design` because POC-04 was unbuildable and got a new case. Re-run the execution plan." You see the new `case-cn.md` is fresher than your existing `execution-plan-cn.md`. You confirm with the student that you should overwrite the plan and POC scaffolds, archive the old ones if needed, then re-run from step 2.

---

## 10. Self-check before declaring done

Before saving the files and reporting back, walk through these checks. If any fail, fix the output first.

- The execution plan opens with the mini-POC definition reproduced verbatim from section 3 of this SKILL.md.
- Every POC in the plan is a skill-learning drill, not a fake business project. No POC reads as a mini-resume entry.
- Every POC maps to at least one case decision named in the case file's "Key Technical Decisions Replay" section. Any unmappable gap is flagged with a warning, not silently dropped.
- The cross-entanglement matrix is present, both as a table column and as a prose paragraph identifying the strongest 2 to 3 couplings.
- The week-by-week schedule names mock interview checkpoints (typically weeks 8 and 12) and aligns with the student's capacity profile (or flags loudly that capacity was missing).
- POC names and tutorial filenames match: `pocs/poc-01-strand-agents/` pairs with `tutorials/01-strand-agents-quickstart-cn.md`.
- All artifacts (1 plan + N POC READMEs + N tutorial stubs) were actually written to disk in the right paths.
- The Feedback path section names the case-difficulty-rollback escape hatch and explains when to use it.
- No em dashes or en dashes appear in body text. Hyphens only in compound words.
- H2 sections are numbered `## N. Title` with `---` separators between them.

---

## 11. What this skill does NOT do

This skill does not diagnose gaps from scratch. It reads the gap analysis produced by `qualify-gap-analyze`.

This skill does not design the project case. It reads the case produced by `mini-project-design`.

This skill does not author tutorial content. It writes only the stub. The actual concept-by-concept teaching belongs to `qualify-coach`.

This skill does not run mock interviews. That is `qualify-mock-interview`, which runs after the student has done the POCs and read the tutorials.

This skill does not produce landscape research. That is `understand-landscape` from the prerequisite course `career_planning`.

If the user asks for any of those, redirect them to the correct skill rather than absorbing the work.
