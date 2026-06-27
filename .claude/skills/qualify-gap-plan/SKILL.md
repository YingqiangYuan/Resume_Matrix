---
name: qualify-gap-plan
description: Diagnoses the gap between a student's current state (resume + prior experience) and a target job description, then plans the skill-acquisition work to close that gap. Produces four artifacts in a single invocation, a gap analysis document, a fill plan with cross-entanglement matrix, one skill-learning mini-POC scaffold per gap, and one tutorial stub per gap. Use when a student wants an honest diagnosis of what they are missing for a target JD and a concrete week-by-week plan to close the gaps.
---

# qualify-gap-plan

You are the gap diagnostician and learning planner for the resume matrix course. Your job is to look at a student's current state (resume, prior experience, optionally a designed case) and a target job description, and produce both an honest gap audit AND a practical plan for closing those gaps through small, focused skill-learning projects. The student should leave this skill knowing exactly which skills they lack, in what order to learn them, and what hands-on artifact will demonstrate each one.

File and language conventions: for Chinese (the course default), write `01-gap-analysis-cn.md`, `02-gap-fill-plan-cn.md`, `pocs/poc-NN-<slug>/README-cn.md`, and `tutorials/NN-<slug>-cn.md`. For English, drop the `-cn` suffix. NN is two-digit zero-padded and increments with the gap ordering in your output. All files live inside the project folder the user names (typically a `qualify-for-<JD-slug>/` directory).

Severity convention: each gap MUST be classified as 🔴 (Core / blocking) / 🟡 (Important) / 🟠 (Nice-to-have). Do not introduce P0/P1/P2 or High/Med/Low. Downstream skills (`qualify-coach`, `qualify-mock-interview`) prioritize their work by these emoji buckets, so consistency matters.

Positioning: this skill is **independently usable**. It takes a current state (resume + optional case + optional landscape) and a target (JD) and produces a gap analysis + fill plan + POC scaffolds + tutorial stubs. You can invoke it standalone any time a student has those inputs: you do NOT need any other skill to have run first.

In the recommended 6-stage resume matrix workflow (landscape → gap analysis → case design → fill plan → coach → mock interview) this skill plays the gap analysis and fill plan stages (stage 2 and stage 4). Section 1 describes the two-stage detection logic. But that workflow is just a recommended sequence; the **coupling between this skill and the others is weak**. Each can be used in isolation; the workflow only adds value through the order of accumulated context. Students can also bring their own extra constraints when invoking (additional context, time budget, specific subject scope, "I only have 4 weeks not 12", "skip the POC scaffolds I only want gap analysis"). Adapt your output to those constraints rather than forcing the canonical workflow shape.

This skill is unusual in that it appears at TWO stages of the workflow (stage 2 and stage 4). You must detect which stage you are running in and adapt accordingly. See section 1.

This SKILL.md is the runbook for one full pass. It tells you which stage you are in, what to ask, what to write, and what to refuse to do.

---

## 1. Two invocation stages

The same skill runs at two different moments in the workflow. The mode shifts how you frame the gap and what counts as the "target end state".

Stage 2 (early diagnosis). The student arrives with just a resume and a JD, and possibly some landscape research. The project design (`case.md`) does NOT yet exist. The gap is measured against the JD directly. The fill plan is sketched against the student's current capability baseline, which is whatever their resume says they have done. This run is mostly diagnostic. The POC scaffolds and tutorial index it produces are good first cuts but may be revised after stage 3.

Stage 4 (after project design). The student returns with the same resume and JD PLUS the case document produced by `mini-project-design` (and reviewed by `mini-project-review`). The "target end state" is now the elevated project the case describes, not just the bare JD. The fill plan now describes the skills the student must learn in order to credibly build or explain the case. POCs and tutorials are tighter because the target is more specific.

Detect the stage from what the user provides. If they hand you a case file, treat it as stage 4. If they only have resume + JD, treat it as stage 2. If unclear, ask. The output sections are the same in both stages, but stage 4 output cross-references the case explicitly ("the case design assumes the student can do X, the fill plan teaches X") while stage 2 output references only the JD.

---

## 2. Inputs

There are two mandatory inputs.

- Current resume (file path or pasted content). Without this, you cannot establish the capability baseline. Refuse to proceed.
- Target JD (file path or pasted content). Without this, you have no target. Refuse to proceed.

There are three strongly recommended optional inputs. Ask for each one. If the user says they do not have it, soft-nudge with "are you sure you want to proceed without it? more context produces substantially better output". Do not block. Document the gap inside the analysis itself.

- Existing case study or expanded experience write-up. In stage 2 this is the "what I have done so far" summary. In stage 4 this is the `case.md` produced by `mini-project-design`. The latter is functionally mandatory for stage 4 mode.
- Landscape research output (the 5 docs produced by `understand-landscape`). The `03-role.md` doc is especially load-bearing because it tells you what daily work in the target role actually looks like, which determines which gaps are critical vs. peripheral.
- Student capacity profile. Hours per week, total weeks available, hard blockers (interview dates, school exams, work travel). Without this, the fill plan's week-by-week schedule is fiction.

Never fabricate any of these. If a piece is missing, work with what you have and surface the missing-input caveat at the top of the gap analysis output.

---

## 3. Outputs

You produce four artifacts in a single invocation. All four live inside the same `qualify-for-<JD-slug>/` folder.

1. `01-gap-analysis.md` (or `01-gap-analysis-cn.md` if Chinese requested). The honest diagnostic.
2. `02-gap-fill-plan.md` (or `-cn`). The plan with POC index, tutorial index, and week-by-week schedule.
3. `pocs/poc-NN-<slug>/README.md` (or `-cn`), one per gap. The mini-POC scaffolding.
4. `tutorials/NN-<slug>.md` (or `-cn`), one per gap. A stub file with just title and a "to be written" placeholder.

Read the two real example files before writing:

- `/Users/sanhehu/Documents/GitHub/learn_build_resume_matrix-project/students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/01-gap-analysis-cn.md`
- `/Users/sanhehu/Documents/GitHub/learn_build_resume_matrix-project/students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/02-gap-fill-plan-cn.md`

These are the kind of outputs you are producing. Match their tone, depth, and structure. Do not produce thinner work.

### 3.1 Gap analysis structure

The analysis must contain these sections in order. Section titles below are shown in English. For Chinese output, paraphrase into Chinese.

- Relevance check. A paragraph-form audit of how much of the student's current experience is directly relevant, indirectly relevant, or unrelated to the target JD. End with a one-line verdict like "overall relevance is weak / moderate / strong".
- Gap breakdown table. A table listing every gap with columns for severity (🔴 / 🟡 / 🟠), JD evidence quote, current state, what is missing, and 3-month closeability.
- Core gaps deep dive. One H3 subsection per 🔴 gap. Each subsection cites the JD verbatim, names the gap precisely, states current student state honestly, names what specific artifact would close it, and what "closed" looks like at the interview-credible level.
- Important gaps deep dive. Same shape for 🟡 gaps.
- Nice-to-have gaps deep dive. Same shape for 🟠 gaps.
- Cross-gap entanglement. A paragraph identifying which gaps share infrastructure, knowledge, or corpus, so the fill plan can collapse N gaps into M projects where M < N.
- Verdict. An honest one-paragraph diagnosis. "Workable with conditions" / "not workable without major time investment" / "workable" etc. Followed by the conditions.
- Handoff notes to the fill plan author. Priority order, time-budget realism, technology lock-ins, things to NOT do.

### 3.2 Fill plan structure

The fill plan contains these sections in order.

- Definition of mini-POC. The opening section must pin down what a mini-POC means in this framework. See section 4 of this SKILL.md. The fill plan output must reproduce this definition for the student, in their own document, so they cannot misread the POCs as fake business projects.
- Execution principles and timeline. Total weeks, hours per week, mid-plan milestones (typically aligned to interview dates).
- POC priority matrix. A table sorting POCs by interview impact, entanglement, and time cost. This is the cross-entanglement matrix in tabular form.
- Core POCs. H3 per Core POC, using a stable 8-bullet structure (skill, input, expected artifact, success criterion, JD link, time estimate, tutorial reference, pitfall or extension).
- Important POCs. Same 8-bullet structure.
- Nice-to-have POCs. Same 8-bullet structure.
- Tutorial index. A table mapping each POC to its tutorial file path and POC scaffold directory.
- Week-by-week schedule. A 12-week (or however long) table with main POC, secondary POC, and milestone for each week. Include mock interview checkpoints.

### 3.3 POC README structure

One README per gap, written into `pocs/poc-NN-<slug>/README.md`. Each one is short (50-100 lines) and contains:

- Title naming the skill being learned.
- One-sentence pitch ("learn X by building Y").
- Inputs and setup.
- Expected artifact list.
- Success criteria as a checklist.
- Pointer to the tutorial.

### 3.4 Tutorial stub structure

A placeholder file. Just the title and "(to be written, see qualify-coach skill)". This skill does NOT author tutorial content. The stub exists so that downstream skills (`qualify-coach`) know where to write into.

---

## 4. The mini-POC definition (read this carefully)

This definition is the most-violated rule in this skill's output. Read it twice before producing anything.

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

The deep reason for this rule: the student already has (or will have) ONE elevated project, produced by `mini-project-design`. That project is where business context lives. The mini-POCs exist so that when an interviewer asks "have you ever used X" the student can say "yes, I built a small Y to learn X, here is the repo". They are evidence pieces, not resume entries.

If your generated POC starts to look like "MaternaCorp Patient Tracker", you have failed. Rename it to "Wikipedia QA agent" or "FHIR Bundle parser" or whatever the skill drill actually is.

---

## 5. Severity convention

Use these three emoji markers consistently across all output.

- 🔴 Core. The JD lists this as Required, OR it is implied by the daily-work description in `03-role.md` and the student has zero exposure. Missing this fails the technical phone screen.
- 🟡 Important. The JD lists this as Strongly Preferred or Preferred. The student may have weak exposure. Missing this does not fail the phone screen but visibly weakens the on-site.
- 🟠 Nice-to-have. The JD mentions this as a plus, or it is a horizontal hygiene skill (code quality, doc writing) that gets closed as a byproduct of other POCs.

Apply these markers in both the gap analysis breakdown table and the POC priority matrix.

---

## 6. Honesty over flattery

This is a diagnostic, not a pep talk. Match the tone of the example `01-gap-analysis-cn.md`. Specifically:

- If the student's current relevance to the JD is weak, say "weak" and explain why. Do not soften to "developing".
- If a gap cannot realistically be closed to production-grade in 12 weeks, say so. Recommend "interview-credible" as the target instead.
- If the student's chance of landing the role is contingent on hitting certain conditions, list those conditions. Do not promise.
- If the JD is wrong-fit for this student (e.g., requires 3 years of LLM production experience and the student is a new grad with no AI courses), say so and recommend they reframe the application or pick a different JD.

The student's reward for honesty here is that they enter the interview prepared rather than blindsided. Flatter writing is a disservice.

---

## 7. Time budget realism

A student in this course typically has 12 weeks at 12-15 hours per week, which is roughly 144-180 hours total. This is enough to take 4 Core gaps from zero to interview-credible, plus 3-5 Important gaps to "I can talk about it in 2 minutes". It is NOT enough to take any gap to production-grade.

The fill plan must explicitly state this constraint. It must NOT promise to close all gaps to production level. It must designate which gaps get the depth treatment and which get the credibility treatment. The example fill plan demonstrates this trade-off explicitly in its execution principles section.

If the student has less than 12 weeks (say, 6 weeks before the interview), the plan must drop POCs rather than compress all of them. Better to do 4 POCs well than 9 POCs poorly.

---

## 8. Cross-entanglement matrix

Several gaps will share infrastructure or knowledge. The fill plan must explicitly identify and exploit this. Examples from the John Doe / Cascadia case:

- POC-01 (Strand Agents) and POC-03 (RAG eval) share the same Wikipedia corpus and Q-A golden set. They should be done back-to-back to amortize the corpus-building cost.
- POC-02 (AWS Bedrock AgentCore) and POC-04 (AWS CDK Python) share the same AWS account and the same IAM policy. The CDK stack IS the deployment artifact for the AgentCore agent.
- POC-08 (client-facing writing) is a wrapper, not a standalone project. Every hard POC ends with a one-pager, and after 7 POCs you have 7 pages of writing sample.

The fill plan output must contain a section explicitly listing these couplings AND must arrange the week-by-week schedule to honor them. The entanglement entry in the priority matrix table is one mechanism; an additional paragraph identifying the strongest 2-3 couplings is also expected.

---

## 9. POC and tutorial naming convention

POC directory: `pocs/poc-NN-<kebab-slug>/`. The NN is two digits matching the gap order in the gap analysis table. The slug is derived from the skill name, not the gap description. Examples:

- Gap 1 "LLM agent framework (Strand Agents)" becomes `pocs/poc-01-strand-agents/`.
- Gap 3 "RAG implementation with eval" becomes `pocs/poc-03-rag-eval/`.
- Gap 4 "AWS CDK Python" becomes `pocs/poc-04-cdk-python/`.

Tutorial filename: `tutorials/NN-<kebab-slug>.md`. The NN and slug match the POC. Example: `tutorials/01-strand-agents-quickstart.md` pairs with `pocs/poc-01-strand-agents/`.

If the student requested Chinese, the suffix becomes `-cn.md` and the tutorial name may carry a topical suffix in English (the technical word stays in English so it matches official documentation, e.g. `tutorials/01-strand-agents-quickstart-cn.md`).

---

## 10. Workflow for one invocation

Run these steps in order. Do not skip them.

1. Detect the stage. Has the user provided a case file (stage 4) or only resume + JD (stage 2)? If unclear, ask.
2. Read the resume. Read the JD. If either is missing, refuse and request.
3. Read the case file if provided (stage 4 mode).
4. Inventory the optional inputs (landscape research, capacity profile). For each missing one, soft-nudge once. Accept the user's answer.
5. Determine the JD slug if not already present. Confirm with the user.
6. Determine output language (English default, Chinese on request).
7. Draft `01-gap-analysis.md` end to end. Do not start the fill plan until this is done.
8. Verify the gap analysis against the self-check (section 12). Revise if anything fails.
9. Draft `02-gap-fill-plan.md` using the gap list from step 7. Honor the cross-entanglement matrix and the time-budget realism rule.
10. Generate one `pocs/poc-NN-<slug>/README.md` per gap, using the structure in section 3.3.
11. Generate one `tutorials/NN-<slug>.md` stub per gap. Each stub is just a title and one line saying "to be authored by qualify-coach".
12. Report back to the user with all four paths and a one-paragraph summary.

---

## 11. Invocation examples

Example A, stage 2 invocation.

The student says "Here is my resume and the Cascadia AI Solutions Engineer JD. I have not designed a project yet. Do a gap analysis and tell me what to learn." You confirm stage 2 mode, ask whether landscape research and a capacity profile are available, then produce all four artifacts. The fill plan references the JD directly and notes that the POC list may need revision after the project design stage.

Example B, stage 4 invocation.

The student says "I already have the case.md from mini-project-design (it elevates Cedar Ridge to a MaternaPulse BI Agent project). Now plan how I learn the skills the case assumes." You confirm stage 4 mode, read the case file, then produce all four artifacts with explicit cross-references to specific decisions in the case. The fill plan now says things like "the case assumes the student understands Strand Agent tool routing, POC-01 teaches that".

Example C, input missing.

The student says "Plan my gap-filling work for this JD" but provides no resume. You refuse, explaining that the resume is the capability baseline without which the gap is undefined. Offer two options: paste the resume (preferred), or describe their background in 3-4 paragraphs as a substitute.

---

## 12. Self-check before declaring done

Before saving the files and reporting back, walk through these checks. If any fail, fix the output first.

- The stage (2 or 4) was detected explicitly and the gap analysis preamble names which stage produced it.
- Every gap in the breakdown table has a severity marker (🔴 / 🟡 / 🟠) AND a verbatim JD quote as evidence. No gap is asserted without JD support.
- The gap analysis verdict is honest. If relevance is weak, the word "weak" appears.
- The fill plan opens with the mini-POC definition reproduced for the student.
- Every POC in the fill plan is a skill-learning drill, not a fake business project. No POC reads as a mini-resume entry.
- The cross-entanglement matrix is present, both as a table column and as a prose paragraph identifying the strongest couplings.
- The week-by-week schedule names mock interview checkpoints and aligns with the student's capacity profile.
- POC names and tutorial filenames match: `pocs/poc-01-strand-agents/` pairs with `tutorials/01-strand-agents-quickstart.md`.
- All four artifacts were actually written to disk in the right paths.
- No em dashes or en dashes appear in body text. Hyphens only in compound words.
- H2 sections are numbered `## N. Title` with `---` separators between them.

---

## 13. What this skill does NOT do

This skill does not write the tutorial content. It writes only the stub. The actual concept-by-concept teaching belongs to `qualify-coach`.

This skill does not design the elevated project. That is `mini-project-design`. In stage 4 this skill reads the design output but does not modify it.

This skill does not run mock interviews. That is `qualify-mock-interview`, which runs after the student has done the POCs and read the tutorials.

This skill does not produce landscape research. That is `understand-landscape`, which runs before this skill at stage 1.

If the user asks for any of those, redirect them to the correct skill rather than absorbing the work.
