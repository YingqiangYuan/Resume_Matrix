# The 1+N Resume Method, One Master Resume and N Targeted Versions

> This is the fifth piece in the examples series. The first four covered the hiring pipeline, ATS, and how new grads and experienced candidates should each approach the resume. This one shifts angle: when you are applying to several different kinds of roles at once, how do you use a single body of material to efficiently produce N targeted resumes.

## 1. Lesson Overview

By now you know who reads a resume, how ATS works, and how new grads and experienced candidates each approach writing one. Those are skills for producing a single resume.

This section tackles a different problem. In real life, most people are not chasing only one kind of role, and that is doubly true in the AI era. The lines between roles keep blurring, and employers now assume that with AI in hand you can cross industries and ramp up on new tools and concepts quickly. The upshot is that the same candidate often fits several Job Families at once. A CS student can reasonably apply to Software Engineer, Data Analyst, and AI Engineer in parallel. An experienced candidate might apply to startups and large companies simultaneously while also spanning healthcare, finance, and consumer internet.

This "one person aimed at many directions" pattern is essentially the same idea as the short-video account matrix everyone is running today. One person, one core body of content, repackaged with different emphasis into multiple versions and pushed across different channels. The probability that a target audience (employers and recruiters, in our case) actually sees you goes up dramatically.

Every direction asks for a different highlight reel, and every company cares about a different storyline. If you write a fresh resume from scratch for every direction and every company, your time cost explodes. If you use one resume for everything, your targeting falls apart. This section covers the middle path, called the 1+N resume method. The core idea is to keep one master that holds all your material, then derive each targeted version through deletion.

There is no hands-on exercise here. The goal is to make the concept and the workflow clear. Later examples will show you how to use AI tools to efficiently maintain this body of material.

> Note: this section uses a fictional student named John Doe as the example. He is not a real person. His projects and company names are invented to help you follow the workflow.

---

## 2. What a Resume Really Is, High Density Polished Writing

Let's go back to a basic question. What is a resume actually for?

A resume is a document that shows your best feathers at very high information density to the people doing the hiring. A Recruiter usually gives you a few seconds. A Hiring Manager might give you a few dozen seconds at most. On one page you have to convey who you are, what you have done, and what problem of theirs you can solve.

"High information density" means every line has to earn its keep. If a bullet does not help with the role in front of you, it is occupying space that some other bullet could have used. A bullet that lands well for an SDE role (heavy on system architecture and API design) carries much less weight for a Data Analyst role. A bullet that emphasizes A/B testing, SQL exploration, and dashboard design will not do much for an SDE role either.

So your experience can be wide, the stories you could tell can be many, but for any one specific role you only end up using a slice of that material. You may also need a completely different presentation, shifting the emphasis so the right person sees the right thing. That is what people mean by a "tailored resume."

Tailoring is easy to talk about. The problem is that tailoring is easy to talk about and expensive to do. A genuinely tailored resume, from reading the JD to picking material to rewriting bullet phrasing to reordering sections to fixing layout, can easily run an hour or two. Apply that to 10 roles across 3 directions and your whole week is gone to resume editing.

Is there a way to be both highly targeted and highly efficient? There has to be a hack, and this is it. The 1+N resume method.

---

## 3. What the 1+N Resume Method Is

One sentence: maintain one fully populated Master Resume, and derive N targeted versions from it for different roles, where derivation is pure deletion and never addition.

What is the Master Resume? It is a "ridiculously long" resume. It piles all your material together, sorted chronologically or grouped by which direction each project speaks to. The Skills section is a superset of every skill you have. The most important part is Summary: instead of writing one, you write a separate targeted Summary for each direction you plan to apply to, and stack them all at the top.

What is a derived resume? You open the master, delete the Summary variants that do not match this role, delete the Skills lines you do not need, delete the experiences or bullets that do not help this direction. What remains is the targeted version for that direction.

The master itself never gets sent out. It functions as a material library plus a fixed layout template. Every submitted version is produced by subtraction from it.

Why is this so efficient? Because subtraction is much easier than addition. Addition means that for every resume you rewrite, you have to think again: what does this role care about, which piece of my experience fits, how should I phrase it, in what order. Subtraction just asks, while looking at material that is already written, "is this useful for this role" and deletes if not.

> Note: "deletion" here covers more than dropping bullets. It includes removing Skills lines that do not fit the role, removing irrelevant Summary variants, and swapping the "data-focused" version of an experience for the "AI-focused" version (which is also deletion, you are deleting the AI variant of that section). Every operation is "keep" or "delete," never "write something new."

---

## 4. A Concrete Example, Three Projects, Five Bullet Sets, Four Resumes

Enough abstraction. Let's walk through what one person actually does.

> Note: we use healthcare and finance for the examples because those are the two industries where AI is landing best in production work, with the most roles and the most money.

John Doe is a CS Master's student at the University of Washington, targeting a December 2026 graduation. He has three experiences: a women's health hospital chain, a fraud and AML SaaS, and a consumer social product. His projects each combine AI, Data, and Software capabilities in different proportions.

The first experience is a summer 2025 healthcare project, where at Cedar Ridge Women's Health he built MaternaPulse, an internal natural-language BI Agent for the maternity ward built on AWS Bedrock AgentCore. The full project design material is at [CedarRidge-maternity-bi-agent/](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/). This experience can be told from the AI angle, emphasizing Strand Agents, Bedrock Knowledge Base, multi-provider LLM abstraction, and an evaluation Harness. It can also be told from the Data Analytics angle, emphasizing the semantic layer on Snowflake, Charge Nurse UAT research, a self-service query rate dashboard, and HIPAA plus TJC audit log compliance.

The second experience is a winter 2025 finance project, where as a contractor at NovaRisk AI he built the Phase 0 foundation of a Fraud and AML BI Agent for the anti-fraud operations team. Project design material is at [NovaRisk-fraud-aml-bi-agent/](../../students/john-doe/experiences/from-2025-12-to-2026-01-NovaRisk-fraud-aml-bi-agent/). This one also has two angles. The AI angle emphasizes the tool chain built on Strand Agents, the metric glossary loaded into Bedrock Knowledge Base, and the SOC2 plus FinCEN audit chain. The Data Analytics angle emphasizes a SQL pattern analysis of 6 months of historical ad-hoc requests, rewriting the definitions scattered across Slack and Notion into a YAML semantic layer, and backtesting drift between analyst-written SQL and semantic-layer SQL.

The third experience is a summer 2026 social product project. The case lives at [Pulse Social Feed Ranker mature case](../../students/john-doe/experiences/from-2026-04-to-2026-09-pulse-social-feed-ranker/executed-case.md). This one is relatively pure software work: Go microservices, gRPC, Kubernetes, gradual rollout. There is no second angle on this one.

So John Doe's master resume has these five "shippable" Bullet Sets, each with 3 to 4 bullets underneath:

- MaternaPulse, AI angle
- MaternaPulse, Data Analytics angle
- NovaRisk BI Agent, AI angle
- NovaRisk BI Agent, Data Analytics angle
- Feed ranking microservice, software engineering angle

Notice that the first two and the middle two are really the same project written two ways. The master repeats itself on purpose, because this is his material pool when he is applying, and it does not need to be lean.

He also keeps three Summary variants, one for each target direction: AI Engineer, Data Analyst, Software Engineer. Skills is a superset that covers every technology he has used.

The master looks like this: [resume.md](../../students/john-doe/resume.md). Open it and you will see what "ridiculously long" means.

> Note: when you open the master you will notice it actually has 6 Bullet Sets, with the first one labeled "BEFORE elevation, teaching artifact." That one is a teaching prop for [06-prepare-project-material](../06-prepare-project-material/README.md) and [07-elevate-existing-project](../07-elevate-existing-project/README.md), showing the same internship "before vs after elevation" bullet comparison. It has nothing to do with the 1+N mechanism. You can ignore it for this section and focus on the 5 that follow. 06 and 07 will explain it in detail.

---

## 5. How to Derive Targeted Resumes from the Master

John Doe can apply to AI, Data Analyst, and Software roles at the same time. He derives several targeted resumes from the same master, and the process is entirely deletion.

For an AI Engineer application, he keeps Summary variant A, keeps the AI/ML related Skills lines, keeps the AI version of MaternaPulse, the AI version of NovaRisk BI Agent, and the Feed ranking microservice (since software ability is a plus for AI roles). Everything else gets deleted. The result is [resume-role-1.md](../../students/john-doe/resume-role-1.md).

For a Data Analyst application, he keeps Summary variant B, keeps the Data Analytics related Skills lines, keeps the Analytics version of MaternaPulse, the Analytics version of NovaRisk BI Agent, and the Feed ranking microservice. The result is [resume-role-2.md](../../students/john-doe/resume-role-2.md).

For a Software Engineer application where the target company is a healthcare firm, he keeps Summary variant C, keeps the software/backend Skills, keeps the AI version of MaternaPulse (because the target company cares about healthcare AI) and the Feed ranking microservice. The result is [resume-role-3.md](../../students/john-doe/resume-role-3.md).

For a Software Engineer application where the target company is a finance firm, he keeps Summary variant C, the same software Skills, but keeps the AI version of NovaRisk BI Agent and the Feed ranking microservice. The result is [resume-role-4.md](../../students/john-doe/resume-role-4.md).

See the pattern? Four targeted resumes, each one "deleted" out of the same master. The phrasing, the numbers, the layout all inherit from the master. He does not rewrite a single bullet, does not re-derive a single number, does not worry about inconsistencies across versions. That is the leverage.

---

## 6. A Full Worked Example

To give you a hands-on feel for what this looks like in practice, we have published John Doe's complete material. You do not need to start editing your own resume right now. Later examples will teach you how to use AI tools to efficiently maintain this structure.

Master resume:

- [resume.md](../../students/john-doe/resume.md): contains 3 Summary variants, a superset of Skills, and 5 shippable Bullet Sets (plus one teaching artifact that the next section will use).

The 4 derived submission resumes:

- [resume-role-1.md](../../students/john-doe/resume-role-1.md): AI Engineer direction.
- [resume-role-2.md](../../students/john-doe/resume-role-2.md): Data Analyst direction.
- [resume-role-3.md](../../students/john-doe/resume-role-3.md): Software Engineer, target company in healthcare.
- [resume-role-4.md](../../students/john-doe/resume-role-4.md): Software Engineer, target company in finance.

The raw material behind each experience lives in the `experiences/` folder. It is the full narrative of each project (the healthcare and finance projects each have one folder containing five documents covering background, team, business requirements, architecture, and execution plan; the social project is a single complete case file). This is the source from which bullets are extracted and compressed:

- [MaternaPulse BI Agent project design material](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/)
- [NovaRisk Fraud & AML BI Agent project design material](../../students/john-doe/experiences/from-2025-12-to-2026-01-NovaRisk-fraud-aml-bi-agent/)
- [Social Feed ranking project case](../../students/john-doe/experiences/from-2026-04-to-2026-09-pulse-social-feed-ranker/executed-case.md)

Suggested reading order: first skim the three pieces of source material to understand what each project actually is. Then open the master [resume.md](../../students/john-doe/resume.md) and see how each Bullet Set was pulled out of the source, paying attention to the phrasing differences between the two angles of the same project (AI version vs Data Analytics version). Finally open the four derived resumes and compare them to the master to see what was kept and what was cut.

---

## 7. Core Insight

Stepping back, the 1+N resume method works because resumes have a few hidden properties.

First, targeting equals deletion, not new writing. A highly targeted resume is fundamentally a selected subset of a larger pool of material. The act of selecting is much cheaper than the act of creating.

Second, the same experience can fit multiple "frames." The same project told from the AI angle is one story, told from the Data Analytics angle is another. Neither version is fake, they just emphasize different things. What you need is to write both versions ahead of time rather than improvise at submission.

Third, Summary and Skills are "positioning tags," and bullets are "evidence." A resume's fit for a role is judged first by whether the Summary speaks to what they care about, then by whether the Skills hit the keywords, and only then by the evidence the bullets provide. So all three need multiple versions maintained per role.

Fourth, the master never goes out the door, but it is the most valuable asset. The completeness of the master directly determines how many directions you can apply to and how fast you can produce resumes. Maintaining the master well matters more than perfecting any single submission resume.

---

## 8. Mentor's Note

I have seen too many job seekers open a Word document and rewrite their resume from scratch before every application. Two days later they have sent five resumes, every one of them slightly off, and a few still have the previous company's name on them. That workflow is exhausting and the quality is uneven.

The logic of the 1+N method is plain. Concentrate the work in one place, and everything else becomes mechanical copying and deleting. It splits "writing a resume" into two layers. The first layer is maintaining the underlying material, low frequency, high depth, written once and reused for a long time. The second layer is deriving for a specific role, high frequency, shallow depth, a few minutes each time.

The biggest payoff is not saved time. It is the ability to do large-volume applications while staying high quality. If you send 30 resumes, the 30 derived from one master will be much higher quality on average than 30 written from scratch, because each round of polishing gets amortized onto that one master.

Later examples will cover how to use AI tools to maintain each piece of the master (writing Summary, writing bullets, deepening project material) and how to automate the derivation step. By then you will see that the 1+N structure and AI tools are a perfect match. What AI is best at is constrained deletion and rewriting based on existing material, not creation from nothing.
