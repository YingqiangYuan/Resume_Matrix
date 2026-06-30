# Where Bullet Material Comes From, Three Paths to Prepare Your Project Documents

> This is the sixth piece in the examples series. From here on, we move into the "project material preparation" phase. The first five pieces covered theory (recruiting, ATS, resume structure, the 1+N resume method). Now we tackle a more concrete question. Where do the project documents behind those bullets actually come from?

## 1. Overview

A resume lives or dies on its bullets. Almost every ounce of competitive strength on a resume is concentrated in each individual bullet. Summary and Skills are positioning labels. What actually decides whether you land an interview is the evidence delivered by your bullets.

But bullets do not appear out of thin air. Behind every strong bullet stands a project experience you can explain clearly, in depth, and defend under follow-up questioning. That experience first needs to be documented in detail, then compressed into one or two lines on the resume.

Think about the John Doe example we used in 05-resume-matrix (the case files inside `students/john-doe/experiences/`). Each of his internships has a project record running anywhere from a few hundred to over a thousand words behind it. The 3 to 4 bullets on the resume are refined out of that longer document. They were not written directly.

So the question becomes this. What kind of projects are you going to use to fill out your own experiences folder?

That is what this section answers. I will walk through three common approaches, each matching a different type of student. After reading this section you may not be ready to start building immediately, because the specific tools and AI workflows come later (method two goes to [07-elevate-existing-project](../07-elevate-existing-project/README.md), method three goes to [08-design-new-project](../08-design-new-project/README.md), bullet writing goes to [09-write-bullets](../09-write-bullets/README.md)). But you will understand the underlying logic. Make the project itself solid first, document it clearly, and AI can then help you turn it into polished bullets.

---

## 2. A Prerequisite, Finish Your Targeting First

Before we go further, I am assuming one thing. You have already finished the Understand Landscape stage.

What does that mean? It means you have seriously broken down several Job Descriptions you find interesting, dug into what the companies behind those JDs actually do, what the role does day to day, and what trends the industry is seeing. Based on all of that, you have a reasonably clear picture of the Job Families and the general type of target company you are aiming for next.

Why does this step need to be done first? Because without a target, there is no answer to the question "what kind of material should I prepare." Material preparation is fundamentally a directional activity. You prepare your project material with the specific abilities, tech stack, and business understanding that a particular type of role's recruiter cares about.

If you have not finished Understand Landscape, I strongly recommend going back to complete it. All three methods below assume you are starting from a place where your target is reasonably clear.

---

## 3. What Bullet Material Actually Is

Let's nail down what "material" means here.

Material is not the bullet itself. Material is the source the bullet comes from. It is a detailed document about a specific project. Project background, the business problem being solved, your role in it, the concrete decisions you made, the technologies you used, the final output, how the numbers were measured. Each experience corresponds to one such document, stored in your own Master Repo.

> Note. A Master Repo is a Git repo named with a date and your own name (for example `2026-06-01-john-doe`). It holds all the material you need to write your resume, the Master Resume itself, and the targeted resume variants derived from it. For how to set up this repo and organize the directory structure, [11-submit-and-collaborate §3.1](../11-submit-and-collaborate/README.md#31-github-上有简历所需要的所有素材-这是无可替代的) has a complete directory tree example.
>
> **About the `students/john-doe/` directory in this course's examples.** This is the fictional student used throughout the course for demonstrations. All the paths you see across the examples chapters (like `students/john-doe/resume.md`, `students/john-doe/experiences/...`, `students/john-doe/.../qualify-for-.../case.md`) live, in John's real world, inside his own `2026-06-01-john-doe/` Master Repo. The directory structure is identical, just relocated under the course repo's `students/` subdirectory so you can follow along as we teach. **When you do this yourself, you should have a separate Git repo named `<YYYY-MM-DD>-<your-name>`. You should not fork this course repo and stuff a `students/your-name/` folder inside it.** Doing that couples your own resume assets to course content, and when the course repo updates later you will not be able to merge cleanly. Read the course repo as a reference library, and build your own Master Repo as a standalone repo. That is the right posture. (If this course later adds more fictional student examples, like `students/jane-roe/`, the structure will be identical to John's. They are all instances of the same Master Repo template.)

Why do you need this long document first, and only then a resume bullet, rather than writing the bullet directly? Because a bullet is a highly compressed result, and compression requires sufficient raw information to compress from. When you cannot write bullets that hold up under follow-up questions, the root cause is usually not weak phrasing. It is shallow understanding of the project itself.

So the real meaning of "material preparation" is to thoroughly work through every experience you plan to put on your resume, then capture it in writing. That record later serves two purposes. First, it becomes Context for AI when it writes the bullets. Second, it becomes the mental script of "what did I actually do" that you draw on during interviews.

Below we look at the three most common sources of material.

---

## 4. Method One, Projects Designed by Your Mentor

The first approach is the most direct. Have a mentor with strong judgment design a project for you, and then you execute it.

How it works in practice. Inside your own Master Repo, your mentor creates a folder under `profile/experiences/` or a similarly named directory, corresponding to the project they designed. They write out every dimension of the project (the problem to solve, the tech stack, milestones, expected output). That folder, before you even start building, already contains all the information needed for your resume.

When you read this you most likely have not done the project yet. That is fine. Our expectation is that the mentor designs the project at the same time you are working through this course, and you spend the next few months actually building it. Documentation preceding output is the standard cadence for this kind of learning path.

As your skills grow, you build projects faster. We have seen real examples like this. A student scrolls LinkedIn on Monday and finds a perfectly matched role. That same day, they work with their mentor to design a tight mini project aimed at the company's product direction and industry. They hand-build it and deploy it to the public internet within a day. Tuesday and Wednesday they fill out the README and case document. Thursday and Friday they use the freshly built project as material, revise into a targeted resume, and submit. This kind of cadence is only possible under the "detailed documentation first, then AI compresses the documentation into bullets" workflow.

Right now, open up the corresponding directory in your own Master Repo and take a look. If your mentor has already placed a project folder there, you have your first piece of material. If not, contact your mentor directly.

Later when you do new projects yourself (whether suggested by your mentor or invented by you), you will maintain documentation the same way. The operational flow for that is in [12-maintain-new-projects §3](../12-maintain-new-projects/README.md), "the standard steps for onboarding a new project," with all 8 steps laid out.

---

## 5. Method Two, Elevate an Existing Project

If you are already a junior, senior, or graduate student with some project experience, the second path is the most economical. Take a project you have already done and elevate it.

Solid prior projects are obviously best. But even if your past projects were pretty ordinary, with no real highlights, or honestly weak, or even of the "I heard lots of people just make this up" variety, do not panic. Later we will teach you a method for systematically filling in the missing depth on top of the existing project skeleton.

The core idea of this method. Imagine someone gives you an extra 6 months to redo this old project carefully, and to dig out every part that you did not really understand or fully absorb the first time. Conceptual gaps, the reasoning behind decisions, the underlying logic, the deeper business understanding. All filled in one by one. Once filled in, you go back and revise the corresponding bullets on the resume.

Going further, while you are filling in the depth you can spread out the target-role JDs you previously profiled on your desk. As you deepen the project, look at those JDs and ask yourself. Why does the project, filled in this way, line up exactly with the abilities they care about? That comparison process is itself the key move that binds the "project" to the "role."

The prerequisite for this path is that you already have one or two projects you can elevate. If you have nothing at all, this path does not apply and you need path three.

The full workflow for this method (landscape research, gap analysis, elevation design, mini-POC plus tutorial, coach, mock interview) is laid out in [07-elevate-existing-project](../07-elevate-existing-project/README.md), which walks through John Doe's actual case from start to finish.

---

## 6. Method Three, Begin with the End in Mind and Design a Mini Project Yourself

The third path is for people with zero prior project experience. Freshmen starting from scratch, graduate students who just switched majors, undergraduates in their first two years. Beyond waiting for a mentor to design a project for you, can you design one yourself, working backwards from the end goal?

Let me be honest first. Over a long career, the kind of project design that is logically tight, data-grounded, and substantively deep requires considerable industry experience and judgment. You cannot get there overnight through self-study. But for beginner-stage projects (whose complexity is inherently low and whose scope is inherently limited), designing your own is completely feasible. And training this ability pays back enormously.

Why is the payoff so high? Because this is exactly the kind of capability that grows fastest in the AI era. You have a target (the target role you nailed down in the prerequisite step). To reach that target, you look at what you are currently missing, then reverse-engineer a project that fills those gaps. Once this "goal-driven plus reverse design" muscle is built, you will use it repeatedly throughout your career.

> Note. In the beginner stage, toy-grade projects are OK. But once your career truly takes off, when you use the same method to design projects yourself to reach new goals, the bar rises sharply. The project you design has to have real business value, has to be something that would actually happen inside a company. A mentor designing projects for you solves your immediate problem, but the ability to design projects yourself is your real long-term capital.

The final output of this step is a document about the project you plan to build. What problem it solves, why this problem is worth solving, the planned tech stack, the milestones, and the expected final output. Then you need to be confident about one thing. This is something you can actually finish within 6 months. Your mentor gives the final sign-off to make sure it is neither overblown nor too shallow to anchor a resume experience.

Once that design document is complete, you also have a new piece of material ready for your resume.

How exactly you reverse from goal to project, what the document should look like, and what counts as "deep enough but not overblown," is covered in [08-design-new-project](../08-design-new-project/README.md). 08 reuses the same 6-stage workflow as 07. Only the starting point changes (from "thin existing experience" to "blank slate").

---

## 7. A Full Example, John Doe's Three Experiences Map Exactly to These Three Paths

In [05-resume-matrix](../05-resume-matrix/README.md) we used the fictional student John Doe to demonstrate the 1+N resume method. Looking back at his three experiences, you will find they map exactly to the three paths above. This lets you see directly what "material preparation" looks like on each path.

### 7.1 NovaRisk Anti-Fraud Contract (Method One, Mentor-Designed)

When John joined this 6-week winter contract in the winter of 2025, his mentor had already written [a complete set of project design materials](../../students/john-doe/experiences/from-2025-12-to-2026-01-NovaRisk-fraud-aml-bi-agent/). Project background and business requirements, team and collaboration model, detailed functional and non-functional requirements, technical architecture, execution plan. Before he started building, every bit of context needed for the resume was already in the folder. This level of design granularity is the standard starting point you will get when you work with your mentor on projects later.

### 7.2 Cedar Ridge Women's Health Internship (Method Two, Elevate an Existing Project)

The summer 2025 internship is the most instructive piece in this course, because we have published both versions of the material (before and after elevation) for direct comparison.

Before elevation, John's description of this internship was [a thin SQL reporting document](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting.md). He wrote 15 SQL queries on Snowflake to help a senior analyst produce reports. No business framing, no metrics, no ownership, no decisions of his own. If he used this material directly to write his resume, the best he could squeeze out would be "wrote 15 SQL queries, produced Excel weekly reports." Bullets with no competitive edge.

After elevation, the same internship is reframed as [the full MaternaPulse BI Agent project design](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case.md). He is no longer "doing grunt SQL work." He is "building a natural-language BI Agent powered by AWS Bedrock AgentCore for the entire maternity ward network." Same experience, same time period, same company, but the material depth is an order of magnitude different.

You can open the master resume [resume.md](../../students/john-doe/resume.md) and directly see these two versions compared at the bullet level. The first Bullet Set labeled "Maternity SQL Reporting (BEFORE elevation, teaching artifact)" is the pre-elevation version. The two Bullet Sets immediately after (the MaternaPulse AI emphasis and Data Analytics emphasis) are post-elevation. This is the teaching artifact we told you to "ignore for now" back in 05-resume-matrix. Its purpose surfaces here.

How exactly to elevate from the former to the latter is laid out in [07-elevate-existing-project](../07-elevate-existing-project/README.md), which breaks the full elevation workflow into 6 stages (landscape research, gap analysis, elevation project design, learning plan, execution, mock interview) and walks through John Doe's actual output end to end.

### 7.3 Pulse Social Internship (Method Three, Self-Designed Mini Project)

In the summer of 2026, John's situation was this. He had already nailed down in the earlier targeting phase that his target direction was "Software Engineer who can join a Big Tech backend team." So he reverse-engineered a project he was confident he could finish within 12 weeks. Rewriting a Feed ranking microservice in Go, extracting it from a Python monolith, and rolling it out by percentage. He came up with this design himself. His mentor only signed off on it, he did not design it for him. The end result is [the mature Pulse Social feed ranker case](../../students/john-doe/experiences/from-2026-04-to-2026-09-pulse-social-feed-ranker/executed-case.md). How exactly to reverse from goal to a project design that fits you is covered in [08-design-new-project](../08-design-new-project/README.md). It shares the same 6-stage workflow as 07, just with a different starting point (07 starts from "thin experience," 08 starts from "blank slate").

These three experiences together cover most situations students will face. Either you already have a mentor-designed project on hand (method one), or you have a relatively thin past project (method two), or you have to reverse-engineer one from your target (method three). The full workflow for method two is in [07-elevate-existing-project](../07-elevate-existing-project/README.md). Method three (sharing the same workflow with a different starting point) is in [08-design-new-project](../08-design-new-project/README.md). For this section, getting the underlying logic across is enough.

---

## 8. The Three Paths Are Really the Same Thing

Let's step back and look at all three methods together. On the surface they look very different. The first relies on a mentor, the second on your own past, the third on designing from zero. But underneath, they are doing the same thing.

Around your clearly defined target role, you prepare (or fill in) a project that you can genuinely build or explain within 6 to 12 months, and you maintain a detailed document for that project.

The only difference between the three is where the "project" part comes from.

1. Mentor-vetted. The advantage is the mentor's judgment is strong, the project is usually closer to real business, and the complexity and direction are well calibrated.
2. Your own past experience. The advantage is you already have the concepts in your head. "Elevation" is not filling in from nothing, it is filling in with grounding. You also remember it more easily and can speak to it under interview pressure.
3. Zero experience, designed from the end goal. The advantage is the design capability itself is more valuable than any specific project.

No matter which path you take, what you ultimately hand off to the "bullet writing" step is the same kind of thing. A document that clearly describes every dimension of a specific project, stored under the experiences directory of your own Master Repo.

---

## 9. How to Turn Material into Bullets in the AI Era

Finally, why is this workflow the correct posture for the AI era?

You might think this. AI is so strong now. Why not just tell AI "I want to apply for SDE roles, I'm a new CS grad" and have it write the bullets for me?

That does not work. Or more precisely, it can work, but the bullets produced cannot survive any follow-up questions. The interview will collapse on the first probe. The reason is simple. AI does not have your real project details. It can only generate content that sounds plausible but floats in midair. That content might look fine on the resume, but the moment an interviewer digs one layer down, it falls apart.

The correct posture is two steps. Step one, make the project itself solid (or elevate it solid, or design it solid), and produce a project document that covers every dimension. Step two, use AI to extract the essence from that document, write bullets aimed at different role angles, adjust emphasis, adjust phrasing, adjust length.

The order of these two steps absolutely cannot be reversed. Expand knowledge first (build the project plus write the documentation), then distill and absorb (write the bullets). That is the natural rhythm of real learning. What we are doing is not fabricating a resume. We are using the act of writing a resume to genuinely broaden your knowledge and deepen your understanding of the business. By the time you sit in front of an interviewer, every sentence out of your mouth stands on top of a project document you actually understand.

As for "using AI to turn the document into polished bullets, then fine-tuning the emphasis for different roles," once you have material in hand, with the AI workflows taught in [09-write-bullets](../09-write-bullets/README.md) and [10-write-summary](../10-write-summary/README.md), that part is honestly a matter of minutes. The hard part is always the earlier step. Make the project solid, write the documentation clearly. That is the underlying logic this piece is meant to convey.

---

## 10. A Note from Your Mentor

I often run into students asking me, "Teacher, I don't have any real projects on my resume right now, what should I do?"

On the surface this question is about the resume. Underneath, it is asking why your project library is empty.

The resume is the result, not the starting point. When your project library is empty, or holds only a few class assignments, no amount of AI-driven resume tuning will work magic. There is simply no material to feed the AI. But when your project library holds 2 to 3 projects you genuinely understand, writing a competitive resume in any direction becomes just an engineering problem.

So what this section really wants to tell you is this. Put the work in upstream. No matter where you stand today, whether you hold a strong hand, an ordinary one, or no cards at all, one of these three paths will fit you. Pick one, walk it, build the project solidly, write it up clearly. Then [09-write-bullets](../09-write-bullets/README.md) will teach you how to turn that solid foundation into resume bullets that hold up under follow-up questions.
