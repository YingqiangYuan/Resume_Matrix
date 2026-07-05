# From case document to resume bullets, compressing one experience into 3 to 4 lines

> This is the ninth post in the examples series. Prerequisite: you have finished [07-elevate-existing-project](../07-elevate-existing-project/README.md) or [08-design-new-project](../08-design-new-project/README.md), and you now have an elevated `case.md` plus a `job-description.md` in hand. This post focuses on how to compress that long case document into the 3 to 4 bullets that fit on your resume, while making sure those bullets hold up under interviewer scrutiny.

## 1. Course intro

After 07 and 08, you already hold the most valuable raw material for a resume: a `case.md` that has been elevated against a target JD. A case document often runs five thousand words and covers business context, technical decisions, output metrics, and reflection. But on the resume, **this single experience only gets 3 to 4 bullets of real estate**. This section is dedicated to that compression step.

Why give bullets their own section? Because bullets are the real core of a resume. A Hiring Manager scans the Summary for 6 seconds to decide whether to keep reading, and once they keep reading, what decides "is this person worth a 30-minute phone screen" is the bullets. The Summary makes someone want to keep reading. The bullets make someone want to interview you.

### Why 09 teaches bullets and 10 teaches Summary

Many resume courses teach it the other way around (Summary first, then bullets). We deliberately flip it. The reasoning is simple.

- **Bullets are the evidence. Summary is the label you slap on the evidence.** If you label without evidence, the label is empty and abstract. Students get stuck (am I a backend engineer or fullstack? Am I better at ML or data?) because without concrete project bullets sitting on the table, these positioning questions cannot be answered.
- **Once the bullets are written, the Summary almost writes itself.** Glance at the 4 bullets for an experience, look at what **keywords, tech stack, and problem domains keep showing up**, and the Summary is right there.
- The other way around fails. **If you write the Summary first and then force yourself to "write bullets that match the Summary," you will subconsciously bend the bullets toward the label the Summary claims**, which distorts them. The work you actually did looks one way, but you twist it to fit the Summary's claim.

So the teaching order is case then bullets then Summary. Evidence comes before label.

---

## 2. Bullets are the real core of the resume

When a Hiring Manager reads bullets, they are really hunting for the answers to 4 questions in their head.

- What did this person do? (roughly what shape was the work)
- What tech stack was used, and does it match what we need?
- Did the project produce real results, and how deep did it go?
- What was this person's role on the project, and how much ownership did they have?

The answers to all 4 questions live inside the bullets. The case document has the answers too, but spread out. Bullets are an engineering artifact that **arranges, compresses, and packages those answers into the minimum amount of interviewer eye time**.

But bullet compression has one core difficulty. **You have to compress a ten-thousand-word case into 200 words, and every specific claim still has to survive an interviewer's follow-up questions.** This is a different problem from "write it short." "Short and defensible" is engineering, not writing. The skill and workflow used later in this section exist to solve that engineering problem.

---

## 3. How many bullets per experience, and how to order them internally (the most important section)

This is the genuinely new material in 09, and it is the part you should **go change your current resume after reading**.

> **For complete beginners**: every bullet example below uses John Doe's healthcare AI project as the setting (Strand Agents, Bedrock, semantic layer, Charge Nurse, etc. are real terms from the technology and roles in his project). You do not need to understand every technical term. The point is to feel the **progressive structure** across the 4 bullets, from "project picture" to "design decision" to "hard problem" to "ownership." That structure holds up whether you swap in your own project's terminology (a Python microservice, an ML model, a product design, etc.). The skill looks at your case document and swaps terminology on its own. It will not force healthcare framing on you.

First, on **count**. One experience **typically gets 3 to 4 bullets**. A thin experience may only support 2, and occasionally a flagship project can stretch to 5. **There is no "must be 4" rule.** Count depends on how many distinct angles your experience can actually support. Padding to a number dilutes the impact. The examples below use 4 bullets to walk through the progression logic, but the same logic works for 3 or 2 bullets (just cut the relatively weakest one).

A word on **length** too. Each bullet should occupy **1 to 2 lines** on the resume, with very rare flagship bullets going to 3 lines (typically the core selling point of the experience, where information density is high enough that cutting any segment drops a tier). Exact word count depends on resume formatting. **Do not obsess over word count at this stage.** Once the content takes shape, fine-tune the formatting in course 11 (submission).

Many people write the bullets for one experience and end up with something like this.

> Not recommended (parallel list, 4 bullets shown here but the problem is the same for 3 or 5):
>
> - Built LLM-powered BI agent on AWS Bedrock and Strand Agents framework, achieving 93% accuracy on a 50-question medical reporting eval set.
> - Implemented retry-with-feedback loop integrating Snowflake error messages back into the agent prompt, improving zero-shot SQL generation accuracy from 78% to 93%.
> - Designed YAML-based semantic layer abstracting 40+ clinical metrics from underlying Snowflake schema, reducing analyst onboarding time by 60%.
> - Deployed agent service to AWS ECS with CloudWatch monitoring, achieving 99.7% uptime over an 8-week production rollout.

Each one looks fine on its own. There is a verb, there is a tech stack, there is an impact number, and the formula checks out. The problem is that **the 4 of them together form a list, not a story**.

- An HR or non-technical recruiter scanning the first bullet cannot form a mental picture of "what kind of project is this" (LLM agent? Medical reporting? Who uses it? What does it solve?).
- Every bullet follows the same "Used X to do Y getting Z%" mold. The 4 could be shuffled in any order with no difference.
- There is no progression. After reading bullet 4, your understanding of the project is no deeper than after bullet 1. You just know one more tech stack.

The right approach is to organize these 4 bullets along a **progression that goes deeper and deeper**. The first bullet gives an HR-readable picture, and the next 3 dig down along different dimensions.

### The right structure, B1 is the picture and B2 through B4 are progressive cross-sections

**Bullet 1, the HR-readable project picture**

The audience for this bullet is the **non-technical HR person, recruiter, or business-leaning Hiring Manager**. A single glance should instantly form the picture of "what this person did." The structure is:

> For \<business problem or customer scenario\>, built \<a thing that can be turned into a noun phrase\>, using \<2 to 3 core tech stack items\>, achieving \<one-line impact\>.

Note all 4 elements need to be there, and all 4 need to be short.

> Recommended, B1 example (same MaternaPulse BI agent project):
>
> Built MaternaPulse, an LLM-powered natural-language BI agent for Cedar Ridge's 12-unit maternity hospital network on AWS Bedrock + Snowflake, letting nurse managers query daily census, staffing, and supply data in plain English instead of waiting on custom SQL reports, cutting analyst report turnaround from 2 days to under 5 minutes on routine questions.

A non-technical recruiter scans that bullet: "healthcare plus AI plus natural-language query plus time saved." The picture forms. They decide to keep reading.

**Bullet 2, key technical decision (shows depth of thinking, not tech stack stacking)**

This bullet exists to showcase **one non-trivial design choice**. Not "I used X" but a short version of "between X and Y, I chose X because..."

> Recommended, B2 example:
>
> Designed a YAML-based semantic layer decoupling 40+ clinical metric definitions from the underlying Snowflake schema, letting analysts onboard new metric questions by editing 30-line YAML files instead of touching agent code, and grounding the LLM's generated SQL in business-approved metric definitions rather than raw column names.

What the reader sees is: "they did not just bolt an LLM on top, they put a semantic layer in the middle to solve the data drift problem." Meaningful follow-up questions arise immediately (why not dbt? How do you version control it?).

**Bullet 3, solving the hardest specific technical problem (shows hands-on ability)**

This bullet exists to showcase **the most non-trivial sub-problem in the project** and how you solved it.

> Recommended, B3 example:
>
> Implemented a retry-with-feedback loop using Strand Agents' tool-calling framework: when generated SQL hit runtime errors or returned obviously wrong row counts, the agent re-prompted itself with the database error message, raising end-to-end answering accuracy on the 50-question eval set from 78% (zero-shot) to 93%.

This bullet lets the Hiring Manager know you are not just gluing libraries together. You solved a **genuinely hard small problem**.

**Bullet 4, ownership plus cross-team impact (shows project ownership and real reach)**

The last bullet wraps it up by showing **your real role on the project plus the real reach of the project beyond just the code**.

> Recommended, B4 example:
>
> Partnered with 3 nurse managers, 2 analysts, and the data engineering team across 4 sprints to define the eval set, ground-truth answer keys, and rollout criteria; presented monthly demos to clinical operations leadership and shipped to production for 12 maternity units serving roughly 4,000 monthly inpatient cases.

After B4, the reader knows: this is not heads-down code work. This person can align with non-technical stakeholders, can own a cross-team rollout, and shipped impact that touched 4,000 real patients.

### The effect of the 4 together

Read those 4 bullets together again. B1 gives the overall picture. B2 shows the design. B3 shows the hands-on work. B4 shows the ownership. What gradually forms in the reader's mind is not a list, but a **3D project** and a **3D engineer**. That is what the progression structure of a good bullet set delivers.

**Core principle summary**:

| | Parallel list (avoid) | Progressive structure (aim for) |
|---|---|---|
| Each bullet's angle | All "Used X to do Y getting Z%" | B1 picture, B2 design, B3 hard problem, B4 ownership |
| HR scans B1 | Cannot tell what the project is | Project picture forms instantly |
| Overall feel | A pile of tech details | A 3D project plus engineer |
| Order swappable? | Yes (any order works) | No (shuffling kills the progression) |

> Note. The template above is for 4 bullets, not a hard rule. If your experience only supports 3 bullets, the pattern is B1 + B3 + B4 (cut B2 because there is no non-trivial design decision to expand on). If you can write 5, then B2 splits into two ("core architecture" plus "one specific design choice"). But **B1 must be the HR-readable picture**. That one cannot be cut.

---

## 4. bullet-writer in detail

### 4.1 Step back. Revising bullets is fundamentally about feeding information to AI

Before diving into the skill mechanics, let's get the essence of what is happening straight. Otherwise you will be pressing buttons against a template without knowing why.

**Revising resume bullets is fundamentally this: you feed information to AI, and AI gives you back well-written bullets.** Concretely.

- **Input** (information): your **existing resume** (the master `resume.md`, whose other experience bullets serve as a style and density reference) + the **case document for one experience** + everything else about the project + **optionally** the target JD (with a JD, bullets will lean toward that role's keywords and emphases; without a JD, the skill produces a Job Family generic version). Note that a "Job Family generic version" only stays representative because the case document itself was already synthesized across the whole family back in 07/08 stage 1, not merely because a JD was omitted here — see [07 §4.1, "When the target is an entire Job Family, not one specific company"](../07-elevate-existing-project/README.md#when-the-target-is-an-entire-job-family-not-one-specific-company).
- **Output** (3 to 4 bullets) has 3 landing options:
  1. Print straight into the chat for you to copy by hand
  2. Write to a separate file (for example `qualify-for-.../bullets.md`) for later merging into the resume
  3. **Edit directly into your master `resume.md`** (this course picks this one)

Why this course picks option 3.

- Sitting next to the bullets from your other experiences, you can immediately tell "do these bullets feel coordinated with the rest of my resume, are the style and density consistent"
- The AI / Data / SDE **multiple-angle versions of the same experience sit side by side for comparison** (John's master `resume.md` for the Cedar Ridge experience contains both Bullet Set 2 "AI emphasis" and Bullet Set 3 "Data Analytics emphasis", and only with both lined up can you see the differentiated wording)
- Lowest maintenance cost: to update a bullet later, just edit the master in place. No chasing down multiple scattered files
- Consistent with the "**1 master + N derivatives**" pattern taught in [05-resume-matrix](../05-resume-matrix/README.md). The master is the source of truth. At submission time, derive the variant by deleting from the master against the target JD. There is always only one source of truth.

So the `bullet-writer` skill works like this. It interacts with you to write a few bullets (typically 3 to 4, ranging from 2 to 5 depending on the case depth), then **directly Edits your master `resume.md`** (which is in git, so if it messes up you can revert without fear). Under section 4 Experience, it adds a `### Bullet Set N, <Company>, <Project> (<angle> emphasis)` block, **and right below the bullets it writes a markdown blockquote (`>`) commentary** that explains why each verb was chosen, what concepts the key nouns are packaging, and how the quantitative numbers are defensible (industry baseline plus calculation formula). In chat it just sends one short prompt: "done, please verify with git diff." The detailed rationale lives entirely in the file for you to refer back to later.

> **Beginner note**: the phrase "please verify with git diff" will keep showing up. `git diff` is a built-in Git command. In the terminal, `cd` into your repo and type `git diff` to see what lines changed since the last commit (red = deleted, green = added). When AI uses the Edit tool to modify markdown, occasionally it can miss something, misplace edits, or accidentally delete other sections. `git diff` is the fastest 30-second way to confirm "did this land correctly." If you have never used git, the Source Control panel in your IDE (VS Code, Cursor, JetBrains) does the same thing visually. That works too.

### 4.2 How to actually use it

**When to use it**: you have finished 07 or 08 and have your [`case.md`](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case.md), and in the same directory there is also a [`job-description.md`](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/job-description.md). You are ready to write this experience's bullets into the master [`resume.md`](../../students/john-doe/resume.md).

**What to prepare beforehand**:

- The path to your master resume (for example [`students/john-doe/resume.md`](../../students/john-doe/resume.md))
- The path to the case file for this experience
- (Optional but strongly recommended) the target JD path, so the skill knows which keywords to lean toward
- Which angle you want to use (AI emphasis / Data Analytics emphasis / Software emphasis / ...), i.e. the Job Family positioning for this Bullet Set
- Your experience level (new grads use Built / Implemented / Designed, 3+ years of experience can consider Led / Owned / Architected). The skill will infer this from your master resume, but if you want to be safe, tell it explicitly.

**How to invoke it**: speak plainly in the Claude Code terminal. Following John's tone:

```
Please call /bullet-writer skill.
I'm editing the master resume at students/john-doe/resume.md.

I want bullets for the internship case at
students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case.md
targeting the job-description.md in the same folder.
Angle: AI emphasis (I'm planning to apply for the AI Engineer / AI Solutions Engineer Job Family).

After writing, add a new Bullet Set directly under §4 Experience in resume.md.
```

**What happens during the process**:

1. The skill reads your master `resume.md` + case + JD and gives you 3 "core transferable capabilities" it pulled from the case for you to confirm or adjust (for example "can build an LLM agent from scratch," "can design intermediate abstractions like a semantic layer," "can iterate with non-technical stakeholders").
2. The skill drafts a 4-bullet first cut, explicitly labeling what each of B1 / B2 / B3 / B4 carries (HR picture / design decision / hard problem / ownership). At the same time, it shows you the existing Bullet Sets in your master resume and notes "I'm going to use the same density and verb intensity as Set 4 / Set 6, does that look right?"
3. You push back. The skill adjusts. Common push-back categories:
   - "I can't defend this number in an interview, how was it measured? Replace it." (the skill swaps in an impact phrasing you can defend)
   - "This verb is too strong, I'm a new grad, I can't write Architected." (the skill downgrades to Designed / Built)
   - "B2 is too similar to B3 in angle, both are about technical decisions, change the angle"
   - "B4 doesn't capture my cross-team work, add the part about running 4 sprints with the nurse managers"
4. Once you lock in the 2 to 5 bullet version, the skill **Edits your master `resume.md` directly**, appending a `### Bullet Set N, <Company>, <Project> (<angle> emphasis)` block at the end of §4 Experience, where N = the current number of bullet sets in your master + 1.
5. **Immediately below the bullets, in a markdown blockquote (`>`), the skill writes a "Rationale for this Bullet Set" block**, explaining each bullet's keyword choices: why this verb (what role it plays, whether it matches your level), what concept the key noun packages (sharper than what), and how the quantitative number is defensible (what the industry baseline is, what the calculation formula is, whether each factor is sound). The rationale block is for you to refer to later, and gets deleted entirely when you derive a role-specific resume.
6. In chat, the skill posts one short note saying "Bullet Set N is written, modified lines X through Y, please verify with git diff." **The detailed commentary is not duplicated in chat. It all lives in the file.** Look at the file. If you don't like it, ask the skill to revert or adjust.

**What the output looks like** (the block appended to the end of §4 in `resume.md`, with bullets and the Rationale right below written into the file together):

> **The block below is a *final form* example**: 30 lines of markdown with 4 bullets plus a long Rationale blockquote. First time you see this, you might think "this is too heavy, I'll never produce something like this." That reaction is normal. Two key reminders: (1) the skill drafts the entire Rationale on its own. Your job is to review, push back, and approve. Not write from scratch. (2) The first pass at the Rationale being a bit rough is fine. What matters is that **the block exists**. You can refine it later. Don't let "looks too perfect" psychologically block you from starting.

```markdown
### Bullet Set 7, Cedar Ridge Women's Health, MaternaPulse BI Agent (AI emphasis, for Cascadia AI Solutions Engineer)

Full-stack AI/Data Intern. 2025-06 to 2025-09.

- Built MaternaPulse, an LLM-powered natural-language BI agent ... cutting analyst report turnaround from ~2 days to under 5 minutes.
- Designed a YAML-based semantic layer decoupling 40+ clinical metrics from the Snowflake schema ...
- Implemented a retry-with-feedback loop using Strand Agents' tool-calling framework: ... raising answering accuracy from 78% to 93%.
- Partnered with 3 nurse managers and 2 analysts across 4 sprints ... shipped to 12 maternity units serving ~4,000 monthly inpatient cases.

> **Rationale for this Bullet Set** (internal commentary; strip from any submitted resume)
>
> **Verbs**:
> - Bullet 1 "Built": hands-on builder verb appropriate for new grad. Considered "Architected" (overclaims at this level) and "Developed" (less complete-system feel). "Built" matches the case's actual evidence of you shipping the whole agent yourself.
> - Bullet 2 "Designed": signals ownership of the architecture choice (semantic layer was your call), not just implementation. Matches the case.
> - Bullet 3 "Implemented": describes execution of a specific technique inside the larger system. Calibrated lower than "Designed" because the retry loop is a well-known pattern you applied, not a novel design.
> - Bullet 4 "Partnered": collaborative verb without overclaiming. Says you worked across team boundaries without claiming you led the team.
>
> **Key nouns**:
> - "natural-language BI agent" packages LLM + database query + business domain + non-engineer end user in 4 words. Sharper than "AI tool" or "data assistant".
> - "semantic layer" is an industry-recognized term that signals you understand metric-definition abstractions belong above the database schema. Invites the "vs dbt?" follow-up which is good (interview-readiness signal).
> - "retry-with-feedback loop" is more specific than "self-correction" and tells an experienced engineer exactly what the mechanism is.
>
> **Quantitative claims**:
> - "~2 days to under 5 minutes" report turnaround. Formula: T_before ≈ 2 business days ≈ 960 working minutes (analyst SLA from case prior-state). T_after ≈ 10s agent p95 latency (Bedrock + Snowflake on warm data, CloudWatch-measurable) + 4 min user read-verify (from UAT logs) ≈ 4.5 min. Industry baseline: clinical analyst ad-hoc reports typically 1-3 business days at midsize hospital systems. Defensibility: agent latency from CloudWatch, user time from UAT log, baseline from case description.
> - "78% to 93%" accuracy. Formula: correct / total = 47/50 (rounded to 93%). Industry baseline: SQL-generation accuracy for natural-language BI agents on domain-specific evals runs 70-90% in current published benchmarks (Spider, BIRD). 93% is in the upper range, defensible because eval is moderately curated and schema is constrained. Defensibility: pull the 50-question eval and walk through.
> - "12 maternity units / 4,000 monthly inpatient cases" scale, not impact. Directly from the case prior-state description; no fabrication.
>
> **Notes**: if you later derive a role-specific resume from this master, delete this rationale block. Lives in master only for your reference.
```

Note that the title **explicitly encodes both the angle and the target Job Family** (`AI emphasis, for Cascadia AI Solutions Engineer`), so when multiple AI / Data / Software versions of the same experience sit side by side in your master resume, they are visually distinct at a glance.

**Common failure modes**:

- Skipping the "confirm 3 transferable capabilities" step and letting the skill draft directly. Without an anchor, the skill drafts bullets that follow the case document's section order in parallel, degenerating into a parallel list.
- Not telling the skill whether you are a new grad or experienced. It defaults to medium-intensity verbs, which look too strong for new grads and too weak for senior people. State it upfront, or let the skill infer from your master resume.
- Obsessing over wording from the start instead of questioning the structure. **The first round of push back should be "is the count and angle combination right,"** not "Built or Developed." If the structure is wrong, swapping verbs won't help.
- Letting the skill invent numbers without explaining defensibility. The new skill defaults to actively giving you the industry baseline plus the calculation formula. Your job is to glance at whether the baseline and formula are reasonable. Do not accept unexplained numbers. When the case has no numbers, let the skill construct one for you, but require it to lay out the industry baseline + calculation formula + each factor's plausibility.
- After the skill finishes editing resume.md, treating the task as done **without running `git diff`** to verify. AI edits to files can occasionally miss, misalign, or corrupt other Bullet Sets. Always diff after a run.
- When deriving a role-specific resume, **forgetting to delete the Rationale blockquote**. These are internal commentary for yourself. Make sure they are stripped clean before submission.

---

## 5. bullet-reviewer in detail (optional, for fine-tuning)

What `bullet-writer` produces is a "Job Family generic" master version, already written into `resume.md`. For most cases, when the same master targets different companies in the same Job Family, no changes are needed.

But for **a company you genuinely want**, where the JD has very specific keywords or emphases, use `bullet-reviewer` for a light touch-up. This skill has two landing modes, internally named **Mode A** and **Mode B**.

- **Mode A (in-place edit)**: directly edits the wording of the original Bullet Set in master `resume.md`, overwriting the original. Suitable for small changes (swapping 1 or 2 keywords, adjusting the emphasis of one bullet). Mode A is the default.
- **Mode B (add a new JD-targeted variant)**: keep the original Bullet Set untouched, and **add** a new Bullet Set at the end of §4 Experience, with the title explicitly encoding the target company plus role (for example `### Bullet Set 8, Cedar Ridge MaternaPulse BI Agent (AI emphasis, for Nimbus Health LLM Engineer JD)`). Suitable for larger changes (more than 30% wording change) where you want to keep the original around for future submissions to other companies.

The skill internally uses a 30% diff threshold for Mode recommendation. Less than 30% change defaults to Mode A, more than 30% defaults to Mode B, and you make the final call. When 12 §8.1 talks about "use `bullet-reviewer` Mode A for fine-tuning" when one experience has multiple qualify-for/ subdirectories coexisting, it is referring to this Mode A.

**Input/output essence** (same analysis pattern as §4.1):

- **Input**: an **already written Bullet Set** in your master `resume.md` (for example Bullet Set 7) + a new target JD
- **Output**: before / after fine-tuning suggestions for this JD. After you approve, **let the skill Edit the corresponding Bullet Set wording in `resume.md` directly**, or manually edit it yourself.

**When to use it**: the master bullets are already in master `resume.md`, and you are about to submit to a **strong-match JD where the wording differs slightly**. For example, the master says "LLM agent" but the JD says "conversational AI assistant." Swap the keyword and you match the ATS and Hiring Manager's instincts better.

**How to invoke it**:

```
Please call /bullet-reviewer skill.
My master resume: students/john-doe/resume.md
I want to fine-tune Bullet Set 7 (Cedar Ridge MaternaPulse AI emphasis) for a new JD.
New JD path: <path to another company's JD>
Please play the Hiring Manager for that company and give before / after fine-tuning suggestions.
After I approve, Edit the corresponding lines of that Bullet Set in resume.md directly.
```

**What happens during the process**: the skill plays the Hiring Manager at that company.

1. Analyzes the JD: must-haves, nice-to-haves, and the implicit expectations between the lines
2. 6-second scan first impression (interested / needs adjustment / not a fit)
3. Bullet-by-bullet before / after fine-tuning suggestions (table format), noting which words swap for which words, which number should be pulled forward
4. Skill-transfer analysis (your AWS maps to their GCP equivalent, and whether to call it out explicitly in the bullet)
5. After you confirm, the skill Edits the corresponding lines of that Bullet Set in `resume.md` directly, and asks you to verify with `git diff`

**Common failure modes**:

- Using reviewer to rewrite the bullet itself. The reviewer fine-tunes, it does not rewrite. If the JD is too far from the case, go back to 07 or 08 and rerun the workflow. Do not force-fit at the bullet stage.
- Running reviewer for every single application. Not necessary. Within the same Job Family, most JDs differ by only a few keywords. The master version is enough. Only run reviewer for the **3 to 5 companies you genuinely want**.
- Running reviewer on the same Bullet Set over and over, mutating the master into a mess. Recommendation: once you fine-tune a new version for a specific JD, if the change is substantial, **keep the original in the master and add a new Bullet Set variant targeting that company** instead of overwriting in place.

> Note. `bullet-writer` and `bullet-reviewer` are **loosely coupled** with the 6 skills used in 07/08. Their inputs are files on disk (your master `resume.md` + `case.md` + optional `job-description.md`), and they do not depend on runtime state from prior skills. You are perfectly free to **skip 07 and 08**, hand-write your own case, paste a JD, and call `bullet-writer` directly. Conversely, after running 07 or 08 and getting a case, you are also free to **skip bullet-writer**, write by hand following the 4-bullet internal structure from §3, and Edit `resume.md` yourself. The skill just industrializes the workflow with a quality floor. It is not a required path. The real asset of this course is the **"HR picture first, technical progression second" internal structure thinking** from §3. The skill is just the engineering container.

---

## 6. Look at the bullets John actually produced

Open John's master resume [`resume.md`](../../students/john-doe/resume.md) and scroll to §4 Experience. You'll see **6 Bullet Sets side by side**, and that is the core design of this course. One experience in the master resume can have multiple Bullet Sets, each corresponding to a Job Family angle. At submission time, trim down to the few that are relevant for the target JD.

Specifically, look at the Cedar Ridge experience. It has 2 Bullet Sets in the master at the same time.

- **[Bullet Set 2, Cedar Ridge MaternaPulse BI Agent (AI emphasis)](../../students/john-doe/resume.md)**: used when applying for AI Engineer / AI Solutions Engineer. B1 gives the HR-readable picture "LLM-powered BI Agent on AWS Bedrock AgentCore + Strand Agents," and B2-B4 expand along Knowledge Base retrieval / multi-provider abstraction / evaluation harness.
- **[Bullet Set 3, Cedar Ridge MaternaPulse BI Agent (Data Analytics emphasis)](../../students/john-doe/resume.md)**: used when applying for Data Analyst / Analytics Engineer. **Same Cedar Ridge internship**, same MaternaPulse project, but B1 becomes "Codified maternity ward's operational metric definitions into a YAML semantic layer," and B2-B4 expand along UAT research / adoption dashboard / HIPAA audit (three Data Analytics angles).

Open Set 2 and Set 3 side by side and you'll immediately get what "the same experience writes completely different bullets for different Job Families" means. These two Bullet Sets share the same [`case.md`](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case.md) as the source material, but the angles and wording they extract point in two completely different directions.

You'll also see **Bullet Set 1 ("BEFORE elevation, teaching artifact")** sitting in the master purely as a teaching contrast. The same Cedar Ridge internship, what bullets looked like before elevation ("Wrote 15 SQL queries") versus after (Set 2 / Set 3). Set 1 will never appear in any derived version. Always delete it at submission time.

If you want to see how a case document compresses into bullets, open [`case.md`](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case.md) alongside `resume.md` §4 Bullet Set 2 / 3, and feel how "**ten thousand words of material in the case**" compresses into "**4 bullets totaling 200 words**." What got cut (architecture diagram details, alternative comparisons, reflections and outstanding issues) stays in the case because it doesn't belong on the resume, ready as ammunition for interviewer follow-ups.

---

## 7. Where this step sits in the overall workflow

By now you have completed 06 through 09.

- 06 teaches three paths for project material (mentor-designed / elevate existing / design from scratch)
- 07 walks through method 2's 6-stage pipeline, producing the elevated case
- 08 walks through method 3's same 6-stage pipeline, producing the forward-looking case
- 09 (this section) compresses the case into 3 to 4 bullets and **Edits them directly into master `resume.md` §4 Experience**. One experience can hold multiple angle Bullet Sets in parallel.

The next section [10-write-summary](../10-write-summary/README.md) teaches you to **take all the Bullet Sets already written in master `resume.md` and reverse-engineer N Summaries**, one per Job Family, also written directly into master `resume.md` §1 Summary. Further out, 11 teaches submission: at submission time, **trim down from the master against the target JD** into a derived version (`resume-role-1.md` and similar), deleting irrelevant Summaries, Skills rows, and Bullet Sets. What is left is the resume for that specific application.

---

## 8. Mentor's note

A lot of people get stuck on "should I use Built or Developed" when writing bullets, polishing verbs back and forth. Then the 4 bullets all sound polished, but read together they're a mess, and the Hiring Manager finishes scanning with no idea what project you actually built.

The problem is not the verbs. The problem is **structure**. Are the 4 bullets organized by a progression, does B1 give the HR-readable picture, are B2 through B4 digging along different dimensions.

What this section really wants to teach you is not "how to use AI to write bullets." It is the thinking around **the progressive structure inside the 4 bullets**. Once you understand it, even if you never use the `bullet-writer` skill, your own handwriting will improve dramatically. The skill is the tool that industrializes the thinking. The thinking itself is the core asset of this section.

See you in the next section.
