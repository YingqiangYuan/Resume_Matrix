# Reverse engineer Summary from your bullets, one per direction, all in the master resume

> This is the tenth post in the examples series. Prerequisite: you have finished [09-write-bullets](../09-write-bullets/README.md) and your master `resume.md` already has a few Bullet Sets in it. This section teaches you how to **take those Bullet Sets and reverse engineer N Summaries from them**, one per Job Family direction (for example, one for AI Engineer, one for Data Analyst, one for Software Engineer), all written into the §1 Summary section of the same master `resume.md`.

## 1. Course overview

After finishing 09, the §4 Experience section of your master `resume.md` already holds 5 to 8 Bullet Sets, and the same role might have both an AI angle version and a Data Analytics angle version. The raw material is honestly **already enough to start applying**. What you are still missing is the line at the top that gives a hiring manager 6 seconds to decide whether to keep reading, the **Summary**.

> **Quick terminology note (angle vs direction)**: in 09, Bullet Sets use the word "angle" (multiple ways to frame the same role, for example the Cedar Ridge role has both an "AI emphasis" angle and a "Data Analytics emphasis" angle). From 10 onward, Summary Variants use the word "direction" (which Job Family the whole person is positioned toward, for example the "AI Engineer direction" or the "Data Analyst direction"). These two words sit at different levels of granularity. One master resume carries **N directions** (Summary Variants) and **M angles** (Bullet Sets) at the same time. When you derive a resume, you pick 1 Summary Variant for a direction plus the few Bullet Sets that direction covers. It sounds twisty, but it clicks once you read the example in §3.

In 09 we explained at length [why we teach bullets before Summary](../09-write-bullets/README.md): bullets are the evidence, the Summary is the positioning label you stick on that evidence. Sticking a label on nothing leaves you stuck or sounds fake. Now in 10 we finally get to apply the label, and because the evidence is already on the table, this section is very smooth.

The core claims of this section:

- A Summary is a **positioning label for a 6-second scan**, not a "story of me", not a compressed autobiography, and not a long string of adjectives
- The §1 Summary section of a master resume **holds N Summary Variants at the same time**, one per Job Family direction. When you derive a resume, you keep 1 and delete the rest based on the target JD
- The core method for writing a Summary is **reverse induction from Bullet Sets already written in the master**. Look at which Bullet Sets you plan to keep in the derived version for this direction, pull out the recurring keywords, tech stack, and problem domain, then state the positioning in one sentence

---

## 2. The Summary is a positioning label, not a shrunken autobiography

When a hiring manager picks up a resume, their first scan lands on the Summary. They are looking for the answer to one question: **"Which direction is this person trying to position themselves in?"**

Notice the phrasing is "trying to position themselves", not "who is this person". The Summary is not an introduction. It is a **claim**.

The claim has to answer 3 things:

- **Professional Identity**: what kind of engineer do you want to be seen as (Backend Engineer, AI Engineer, Data Analyst, ...)
- **Core Strength**: what are the 1 or 2 strongest abilities you bring to that identity (for example, "can build an LLM agent from zero" or "can design a semantic layer")
- **Supporting Evidence**: 1 piece of high-impact project evidence or a metric so the first two lines do not feel empty

Compress those 3 things into 2 lines, 200 to 300 characters. That is a good Summary.

Not recommended (the vague "about me" Summary):

> "Passionate and results-driven M.S. Computer Science student with a strong background in software engineering and a love for solving complex problems. Excited to apply my skills to make impact at innovative companies."

After scanning it, the reader thinks: I do not know what you do, I do not know what you are good at, I do not know what problems you can solve. The adjectives are empty words with no signal at all.

Recommended (positioning plus strength plus evidence Summary, for example John's AI Engineer variant):

> "M.S. Computer Science student building production AI systems. Hands-on experience designing two natural-language BI Agents on AWS Bedrock AgentCore, covering maternity-ward operations at a 6-hospital healthcare network and fraud-ops self-serve analytics at a B2B fraud-detection SaaS. Comfortable with Strand Agents, Bedrock Knowledge Base, semantic layers over Snowflake, multi-provider LLM abstraction, prompt evaluation harnesses, and end-to-end AWS CDK deployment."

After scanning it, the reader thinks: "this person is building production AI, specifically shipped two BI Agents across healthcare and fraud, using these specific technologies". A picture forms in 6 seconds, and they decide to keep reading the bullets to verify.

---

## 3. One master resume, N Summaries lined up side by side

This is the key engineering decision of the course. One master `resume.md` does not carry just one Summary, it **carries multiple Summary Variants at the same time**, one per Job Family direction.

John's master `resume.md` has the following lined up under [§1 Summary](../../students/john-doe/resume.md):

- **Variant A, for AI Engineer roles**
- **Variant B, for Data Analyst roles**
- **Variant C, for Software Engineer roles**

3 variants side by side. When you apply, you pick 1 based on the target JD, delete the other 2, also delete the Bullet Sets in §4 Experience that do not belong to this direction, and derive `resume-role-1.md`. When you apply to a different Software Engineer role, you derive `resume-role-3.md` from Variant C.

**Why hang all 3 variants on the same master instead of writing 3 separate masters?**

- Each role has one canonical set of bullets. Splitting them across 3 masters fragments the source, causes drift, and they fall out of sync
- With 3 variants side by side, you can see at a glance **"is this person telling the same factual story across 3 directions with different emphasis, or are they pretending to be 3 different people"**. This is honest positioning, not deception.
- Maintenance cost is lowest. When you edit one variant, the other variants are right there so you instantly see whether something needs to be updated in tandem
- It carries the same spirit as the "1 master plus N derived" pattern taught in [05-resume-matrix](../05-resume-matrix/README.md): the master is the source, derived versions come from deletion

---

## 4. summary-writer in detail

### 4.1 Step back: writing a Summary is also about feeding information to the AI

Same analysis as 09 §4.1.

**The essence of editing a resume Summary is: you feed information to the AI, the AI gives you back a one-line positioning label**. Concretely:

- **Input** (information): your master `resume.md` (the Bullet Sets plus Education plus Skills already in it are the full evidence for this positioning) + a **target Job Family** (for example "AI Engineer", "Data Analyst", "Software Engineer") + (**optional**) a target JD (with a JD, the Summary will be more targeted; without one, it is a general version for the Job Family)
- **Output** (one 2-line Summary), same 3 landing options:
  1. Print to the chat window so you copy by hand
  2. Write to a separate file
  3. **Edit directly into your master `resume.md` §1 Summary section as a new Variant** (the choice this course makes)

The reasoning is identical to 09 §4.1: N variants side by side are easier to compare, easier to maintain, and carry the same spirit as the "1 plus N" design.

So the runtime model of the `summary-writer` skill is: **interact with you to settle on a Job Family direction and lock in a 2-line Summary, then directly Edit your master `resume.md` to append a new `### Variant X, for <Job Family> roles` section under §1 Summary**.

### 4.2 How to use it

**When to use it**: §4 Experience in your master `resume.md` already has a few Bullet Sets, and you want to write a Summary for a particular Job Family direction and add it in.

**What to prepare beforehand**:

- master resume path (for example [`students/john-doe/resume.md`](../../students/john-doe/resume.md))
- the Job Family direction you want to write for (for example "AI Engineer", "Data Analyst", "Backend Engineer", "Data Engineer")
- (optional) one target JD you really want, so the Summary can be tuned for it
- decide **which Bullet Sets in the master count toward the derived resume for this direction** (not every Bullet Set belongs to one direction). The skill will propose this for you, you just confirm

**How to invoke it**:

```
Please call the /summary-writer skill.
My master resume: students/john-doe/resume.md

Please write a Summary for the "AI Engineer" direction and add it under §1 Summary.
Direction assignment: in the master, Bullet Set 2 (Cedar Ridge AI emphasis) and Bullet Set 4 (NovaRisk AI emphasis) belong to the AI Engineer direction.
(optional) Target JD: students/john-doe/experiences/.../qualify-for-Cascadia-.../job-description.md
```

**What happens during the run**:

1. The skill reads your master `resume.md`, finds the §1 Summary section to confirm how many variants already exist, and finds the Bullet Sets in §4 Experience that you named to pull the evidence.
2. The skill extracts the **recurring keywords** from those Bullet Sets: tech stack (AWS Bedrock, Strand Agents, Snowflake, ...), problem domain (production AI agents, clinical analytics, fraud-ops, ...), and signals of intensity (scale, accuracy, shipping, user count).
3. The skill drafts a first version of the Summary (2 lines, 200 to 300 characters), with the 3 components labeled explicitly (Identity, Strength, Evidence). It looks like this:

   ```
   Draft (Variant D, for AI Engineer roles):

   - Identity: M.S. Computer Science student building production AI systems.
   - Strength + Evidence: Hands-on experience designing two natural-language BI Agents on AWS Bedrock AgentCore, covering maternity-ward operations at a 6-hospital healthcare network and fraud-ops self-serve analytics at a B2B fraud-detection SaaS.
   - Tech list: Comfortable with Strand Agents, Bedrock Knowledge Base, semantic layers over Snowflake, multi-provider LLM abstraction, prompt evaluation harnesses, and end-to-end AWS CDK deployment.

   Looks 280 chars, fits 2 lines. Match the existing Variants in your master?
   ```
4. You push back, the skill adjusts. Common push backs:
   - "This Identity is too weak. 'M.S. CS student' is everywhere in the Bay Area. Switch to something sharper like 'AI Engineer building production LLM agents in healthcare and fintech'"
   - "The tech list is too long. 6 buzzwords looks like a Skills section. Cut to the 3 that matter most"
   - "In the Evidence clause, maternal-ward is healthcare and B2B fraud-detection is fintech, which gives a sense of cross-industry range, keep both. But multi-provider LLM abstraction is an implementation detail, that belongs in Skills, not in the Summary"
5. Once it converges to a 2-line version, the skill **directly Edits your master `resume.md`** and appends a `### Variant X, for <Job Family> roles` section at the end of §1 Summary. X equals existing variant count plus 1, and the letter numbering stays sequential (Variant A, B, C, D).
6. **Immediately under the Variant section, using a markdown blockquote (`>`), it writes a "Rationale for this Variant"** paragraph that lays out the Identity choice, the key nouns, verbs, and the ordering of the tech list, and records which Bullet Sets in the master this Variant anchors on (so when deriving, those Bullet Sets get kept together). This rationale block is for you to revisit later, and gets deleted entirely when you derive a role-specific resume.
7. The skill posts only a short note in chat like "Variant X is written, lines X-Y were changed, please git diff to verify". **The detailed walkthrough does not get repeated in chat, it all lives in the file**.

**What the output looks like** (appended to the end of `resume.md` §1 Summary, the Variant plus the adjacent Rationale are written into the file as one unit):

```markdown
### Variant D, for AI Engineer roles

M.S. Computer Science student building production AI systems. Hands-on experience designing two natural-language BI Agents on AWS Bedrock AgentCore, covering maternity-ward operations at a 6-hospital healthcare network and fraud-ops self-serve analytics at a B2B fraud-detection SaaS. Comfortable with Strand Agents, Bedrock Knowledge Base, semantic layers over Snowflake, multi-provider LLM abstraction, prompt evaluation harnesses, and end-to-end AWS CDK deployment.

> **Rationale for this Variant** (internal commentary; strip from any submitted resume)
>
> **Identity choice**: chose "M.S. Computer Science student building production AI systems" over generic "Software Engineer". The Bullet Sets actually show 2 shipped production AI agents, so this Identity is defensible. Avoided "AI Engineer" (claims more years than your level supports) and avoided "Aspiring AI Engineer" (signals junior anxiety).
>
> **Verbs**: "designing" in the Strength clause matches your actual case (you designed the architectures, didn't just implement). Avoided "led" (no team leadership in case).
>
> **Key nouns**: "production AI systems" packages shipped + on real users + on real infrastructure. "natural-language BI Agents" matches exactly the noun used in your Bullet Sets, keeping wording consistent. "semantic layers over Snowflake" signals you understand a non-trivial architectural pattern (invites a good follow-up question).
>
> **Tech-list ordering**: front-loaded "Strand Agents, Bedrock Knowledge Base" because they appear in 3 of 4 AI-direction Bullet Sets and are increasingly searched-for by hiring managers. "AWS CDK" at the end as a deploy-side signal.
>
> **Anchored on Bullet Sets**: 2 (Cedar Ridge AI emphasis), 4 (NovaRisk AI emphasis). When deriving a Variant D resume, keep these two Bullet Sets and drop the others.
>
> **Notes**: if you later derive a role-specific resume from this master, delete this rationale block.
```

**Common ways this goes wrong**:

- Jumping straight into writing the Summary without first confirming "which Bullet Sets in the master correspond to this direction". The Summary ends up not matching the Bullet Sets that actually survive in §4 Experience. The Summary says AI Engineer while the bullets are all Software Engineer projects, and the derived resume tears at the seam. **Pick the evidence first, then write the positioning label**.
- Padding with adjectives (passionate, results-driven, innovative, dynamic, fast-learning). In ATS and hiring manager eyes these are noise. Delete them all, replace with concrete nouns (technologies, projects, metrics).
- First-person pronouns (I, my). The Summary defaults to a subjectless third-person voice, same convention as bullets.
- The skill finishes without running git diff. AI Edits have some chance of missing or misplacing content, always glance at the diff after a run.
- When deriving a role-specific resume, **forgetting to delete the Rationale blockquote**. That block is internal commentary for you alone. When you submit, scrub it cleanly.

---

## 5. summary-reviewer in detail (optional, for fine-tuning)

Same positioning as the `bullet-reviewer` in 09: when there is **a company you actually want** and their JD uses keywords that do not quite line up with the Variant for that direction in your master, use `summary-reviewer` for a light fine-tune.

### Input and output essence

- **Input**: a **Summary Variant already written** in master `resume.md` (for example Variant A) + one new target JD
- **Output**: before/after fine-tuning suggestions for that JD. After you review and approve, **have the skill directly Edit the wording of that Variant in `resume.md`**, or add a new JD-targeted variant

**When to use it**: the master Summary is already in the master, and you now want to apply to a specific role that is **a strong match but uses slightly different JD wording**.

**How to invoke it**:

```
Please call the /summary-reviewer skill.
master resume: students/john-doe/resume.md
I want to fine-tune Variant A (AI Engineer) for a new JD.
New JD path: <path to a company's JD>
Please play the hiring manager for that company and give me before/after fine-tuning suggestions.
After I confirm, directly Edit the corresponding lines of that Variant in master.
```

**What happens during the run**: the skill plays the hiring manager for the company:

1. Analyzes the core buzzwords and emphasis points in this JD
2. Does a 6-second scan of your Variant A through their eyes (interested, needs adjustment, wrong fit)
3. Before/after fine-tuning suggestions: which words to swap, which phrase to move to the front of the Summary
4. Asks you to decide between "**in-place edit Variant A**" or "**add a new JD-targeted variant like Variant A-Cascadia**" (default is in-place, unless changes exceed 30%)
5. Edits master, you verify with git diff

**Common ways this goes wrong**:

- Repeatedly using the reviewer to edit the same Variant, master gets messier with every pass. Suggestion: in-place edit is fine for small tweaks (swap 1 or 2 keywords). For bigger changes, add a JD-targeted variant and keep the master original intact
- Using the reviewer to change direction. The reviewer fine-tunes wording, it does not switch Job Family. If you find that Variant A is completely off for the target JD, you should run `summary-writer` to write a Variant for a new direction, not force the issue here

> Note: `summary-writer` and `summary-reviewer` are **loosely coupled** with the prior 7 skills (5 qualify-* / mini-project-* + 2 bullet-*). The inputs are files on disk (master `resume.md` + optional `job-description.md`), with no dependency on runtime state from prior skills. You can **skip 09 entirely**, hand-write a few bullets into master, and call `summary-writer` directly to write a Summary. Going the other way, after running 09 and getting Bullet Sets in master, you can also **skip summary-writer** and hand-write per the 3 things in §2 (Identity, Strength, Evidence). The skill simply industrializes the process and provides a quality floor.

---

## 6. A look at John's actual 3 Summary Variants

Open John's master resume [`resume.md`](../../students/john-doe/resume.md) and scroll to §1 Summary. You will see 3 Variants side by side:

- **Variant A, for AI Engineer roles**: Identity is "building production AI systems", evidence is the 2 BI Agent projects, tech list focuses on LLM agent / Bedrock / Snowflake / CDK
- **Variant B, for Data Analyst roles**: Identity is "focused on Data Analytics for AI and risk products", evidence is semantic layer / UAT / dashboards, tech list focuses on SQL / Python / pandas / Looker / Tableau
- **Variant C, for Software Engineer roles**: Identity is "building distributed backend services", evidence is the Go feed ranker plus AI systems, tech list focuses on Go / Python / gRPC / Kubernetes / AWS CDK

3 Variants describing **the same person**, but from 3 completely different angles.

If you read Variant A and Variant B side by side, you will see an obvious pattern: the same Cedar Ridge MaternaPulse role is described as "LLM agent on AWS Bedrock" in Variant A and as "semantic layer over Snowflake" in Variant B. Same project, same artifact, two angles, both true, just different emphasis. This follows the same playbook as the Bullet Set 2 (AI emphasis) plus Bullet Set 3 (Data Analytics emphasis) example in 09 §6: **same role, N angles, all in master, derive by keeping 1 per JD**.

Also notice that the **tech lists across the 3 Variants are completely different**. This is intentional. When deriving a resume, the §3 Skills section also gets trimmed by Variant (the Variant A derived version keeps only the AI/ML, Cloud, and Data Infra rows; the Variant B derived version keeps only the Data Analytics and Data Infra rows). Summary, Bullet Sets, and Skills all get trimmed in sync when deriving, and the result is an internally consistent resume for that direction.

---

## 7. Where this step sits in the full workflow

By this point you have walked through 06 through 10:

- 06 taught the three paths for project material
- 07 / 08 ran the 6-stage pipeline and produced the case
- 09 compressed the case into Bullet Sets and Edited them into master `resume.md` §4 Experience
- 10 (this section) reverse engineered N Summary Variants from the Bullet Sets already in master and Edited them into master `resume.md` §1 Summary

At this point the master `resume.md` is **written**: §1 has multiple Summary Variants, §2 Education, §3 Skills full table, §4 multiple Bullet Sets. The next section [11-submit-and-collaborate](../11-submit-and-collaborate/README.md) teaches you how to **derive a JD-targeted resume from this master** (`resume-role-N.md`), and how to put the master plus derived resumes on GitHub and Google Doc for dual-track collaborative review with a mentor.

---

## 8. A note from the mentor

A lot of people writing a Summary get stuck on "should I call myself passionate or results-driven", then they say both, then they tack on innovative / proactive / hands-on, and one Summary ends up loaded with 5 adjectives that they cannot even read out loud without cringing.

Adjectives are not the content of a Summary. **A specific identity, a specific ability, and specific evidence** are.

The hard part of a Summary is not the writing, it is **picking the direction correctly first**. You can only pick the direction correctly when you are looking at the Bullet Sets already in master. Going the other way, "first dream up a cool Summary, then force the Bullet Sets to fit" always fails. That is why 09 comes before 10.

The next section [11-submit-and-collaborate](../11-submit-and-collaborate/README.md) teaches you how to actually derive this master into the PDF that gets submitted, and how to collaborate with a mentor on Google Doc. That is the last mile between the resume that "looks great on GitHub" and the resume that "a recruiter sees on the company careers site".
