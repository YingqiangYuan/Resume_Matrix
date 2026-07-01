# learn_build_resume_matrix

This repo teaches a resume methodology for the US job market — the **1+N resume matrix**. You maintain one ridiculously long master resume that holds multiple Summary Variants and multiple Bullet Sets, then derive every company-targeted resume by **deletion only**, never addition. The repo also ships a fleet of agent skills that operationalize the full pipeline: diagnose gaps against a target JD, design a 3–6 month project that closes them, learn the missing concepts, stress-test under a mock interview, and finally compress the result into bullets and summary variants that defend themselves under follow-up questions.

It is not a template gallery, and it is not a "use ChatGPT to write your resume" guide. It teaches a production process for resumes that hold up under interview pressure.

## What is this — the methodology in 30 seconds

This is a "learn-this-project" repo: a small, deliberately-scoped codebase that teaches one vertical skill end-to-end. The point isn't to ship the code — it's to **absorb** the skill by running it, reading it, being able to defend every design choice, and finishing with a portfolio version on your own GitHub.

Six interactive skills make up the process:

- **`/learn-this-project-absorb`** — on-call mentor for the repo. Multi-mode: Orient (gives you the map plus a `files to READ` vs `files to RUN/DO` split), Context-dive (you bring a `file:line`, it unpacks that spot), Next-step, Build (helps you extend the repo). It's a mentor, not a curriculum — use it when you need help, not as something to sit through linearly.
- **`/learn-this-project-quiz`** — discussion-style Q&A. Each answer is scored against the 3-part standard: **where to look + what + why**. A factually correct one-liner doesn't pass. Two modes: the pre-written bank (lower-bound check) and open-ended (you name a topic, it generates fresh questions).
- **`/learn-this-project-elevate`** — what's beyond this repo's current state. For each upgrade direction it walks you through current state → senior target → alternatives → prerequisite knowledge, and **converges into a concrete starter deliverable** you can hand back to Absorb Build mode to actually build.
- **`/learn-this-project-interview`** — full-project mock interview with pushback. Tests whether you can defend the work to a stranger.
- **`/learn-this-project-demo`** — script your live walk-through; the highest-value part is the "don't show teaching artifacts" cardinal-rule list.
- **`/learn-this-project-publish`** — convert this teaching repo into a portfolio version on your own GitHub. Deletes teaching artifacts, generates a commit cheat-sheet for you to copy-paste, co-writes your README in your own voice, finishes with a hostile-scan audit.

**Recommended order**: absorb → quiz → elevate → interview → demo → publish. These skills are mentors on call — invoke when you need orientation, context, or help. Not a curriculum to follow linearly.

## What's in this repo

```
learn_build_resume_matrix-project/
├── examples/                    # 13-lesson curriculum — the spine of the topic
│   ├── 01-hiring/               # Who actually reads your resume (Recruiter / HM / ATS)
│   ├── 02-ats/                  # The truth about ATS in the AI era
│   ├── 03-new-grad/             # How new grads should write
│   ├── 04-experienced/          # How 1–5 year experienced candidates should write
│   ├── 05-resume-matrix/        # The 1+N method (the architectural lesson)
│   ├── 06-prepare-project-material/   # Three paths to bullet material
│   ├── 07-elevate-existing-project/   # 6-stage elevation pipeline
│   ├── 08-design-new-project/         # 6-stage from-scratch pipeline
│   ├── 09-write-bullets/        # Compress one case into 3–4 bullets
│   ├── 10-write-summary/        # Reverse-engineer Summary from bullets
│   ├── 11-submit-and-collaborate/     # GitHub → Google Docs → PDF
│   ├── 12-maintain-new-projects/      # The 1→N maintenance loop
│   └── 13-final-synthesis/      # Closing the flywheel
├── students/john-doe/           # One complete fictional case (3 projects, 1 master + 4 derived resumes)
├── .claude/skills/              # 10 first-class agent skills — the other half of the topic
│   ├── qualify-gap-analyze/     # Diagnose the gap between resume and JD
│   ├── mini-project-design/     # Design a 3–6 month project (elevate / from-scratch modes)
│   ├── mini-project-review/     # Independent review of mini-project-design output
│   ├── qualify-execution-plan/  # Turn gap + case into a concrete plan with POCs
│   ├── qualify-coach/           # Dialogue-driven, concept-by-concept teaching
│   ├── qualify-mock-interview/  # High-pressure mock interview, never teaches mid-flow
│   ├── bullet-writer/           # Compress a case into a Bullet Set with inline rationale
│   ├── bullet-reviewer/         # Hiring-manager pushback for JD-targeted bullet fine-tuning
│   ├── summary-writer/          # Reverse-engineer a Summary Variant from existing bullets
│   └── summary-reviewer/        # Hiring-manager pushback for JD-targeted Summary fine-tuning
├── docs/learn-this-project/     # 7 analysis docs (meta-skill output, knowledge base for the 6 skills)
├── CLAUDE.md / mise.toml / pyproject.toml  # Minimal scaffolding (zero runtime deps)
└── README.md / README-cn.md     # The file you're reading
```

**Two parallel topic axes**:

1. **The abstract workflow** — 13 lessons that explain the logic (why reverse-engineer Summary from bullets, why bullets follow B1→B4, why elevate-mode locks the company and only re-scopes the tech).
2. **The 10 first-class skills** — tools that operationalize the workflow. They work on any (case, JD) pair, not just on the john-doe example.

`students/john-doe/` is the end-to-end run-through of both axes — you can read it to see what each skill's output file looks like and where it lands.

## Tech stack + setup

| Purpose | Choice |
| :------ | :----- |
| Python | 3.12 (managed by mise) |
| Python venv + packages | uv (managed by mise) |
| Tool versions + task runner | mise |
| Skill runner | Claude Code |

The repo has **zero runtime dependencies** (`pyproject.toml`'s dependency list is empty). The whole "system" is your filesystem — each skill writes a few markdown files when it runs.

```bash
git clone <repo url>
cd learn_build_resume_matrix-project
mise install                # installs Python 3.12 + uv
mise run venv-create        # creates .venv/
mise run inst               # uv sync (no-op today, kept for forward compatibility)
```

Then open the repo root in Claude Code and invoke any skill with `/<skill-name>`.

## Recommended learning flow

Walk the 6 skills in this order:

1. **`/learn-this-project-absorb`** (start in Orient mode) — get the map of the repo and the read/run split. Then **leave the chat**, read all 13 lessons under `examples/`, and skim `students/john-doe/`. When a specific spot trips you up, come back in Context-dive mode.
2. **`/learn-this-project-quiz`** — run Bank mode first as a floor check; an answer doesn't pass unless it lands on where + what + why. Use Open-ended mode to drill the topics where you came up shallow.
3. **`/learn-this-project-elevate`** — pick 1–2 upgrade directions (for example, "add a pytest suite that validates SKILL.md frontmatter" or "add a second worked example covering a career-changer"). Let it converge each direction into a concrete starter deliverable.
4. **(Optional, high-value)** Hand the starter deliverable to `/learn-this-project-absorb` in **Build mode** and actually build the first iteration. This is the bridge that turns understanding into a portfolio artifact.
5. **`/learn-this-project-interview`** — complete a full mock interview round. If the debrief flags a weak answer, return to quiz or absorb to patch that area.
6. **`/learn-this-project-demo`** — rehearse the live walk-through, especially the cardinal-rule "do NOT show" list.

> **Heads-up if you took an earlier learn-this-project course**: absorb is now multi-mode (no longer a linear walkthrough), quiz scores on the where + what + why standard (with an open-ended mode), elevate must end with a starter deliverable, and publish is the new sixth skill.

## Publish — turn it into your own portfolio

The highest-leverage move after learning the material is converting this teaching repo into **a portfolio piece on your own GitHub**. `/learn-this-project-publish` automates the whole conversion:

- **Deletes the teaching artifacts** — `docs/learn-this-project/`, the five `learn-this-project-{absorb,quiz,elevate,interview,demo}` skill directories, `README-cn.md`, `README-ORIGINAL.md`, etc. Every deletion is dry-run-previewed and consent-gated.
- **Generates a commit cheat-sheet** — reorganizes the surviving content into a dependency-ordered list of 10–15+ commits, written to `tmp/publish-commit-plan.md` for you to copy-paste run. The skill never touches `git` itself.
- **Co-writes your README in your voice** — asks you 2–4 prompts per section, drafts from your actual answers, lets you edit before locking each section. The published README should sound like you, not like a teaching artifact.
- **Hostile-scan audit** — assumes a skeptical interviewer is reading your repo with the question "did this come from a tutorial?" and reports findings as 🔴 HIGH / 🟡 MEDIUM / 🔵 LOW. The publish step doesn't count as done until HIGH is zero.

**Cardinal rule**: the published repo must not read as teaching material. If a reader can tell it came from a tutorial, the signal flips from "this person learned a hard thing" to "this person ran a tutorial". Every other publish rule exists to enforce that single constraint.

## Pitching this methodology in interviews

Once you've absorbed this repo, you walk away with more than a resume process. The methodology itself is a **pitchable asset** — when an interviewer asks "how do you level up your skills in the AI era?", you can literally hand them this repo's logic:

> I operate **end-in-mind**. I researched your job posting early, had AI analyze the gap between my current state and the role, used AI to draft my learning plan, then started building projects against it. That's my **goal-oriented** learning method — AI **recursively decomposes** each layer of the problem with me until I'm genuinely strong on that part.
>
> Concretely: I first hand-build a **master agent** (like `qualify-gap-analyze` or `mini-project-design`), use that to spawn more specialized **expert agents** (like `qualify-coach` or `qualify-mock-interview`), then let those expert agents teach me, push back on my answers, and stress-test me. AI is my tool — I'm not its passenger.

That **end-in-mind + use AI to decompose AI** posture is exactly the profile companies need most in the AI era. So remember: this repo doesn't just teach a resume methodology; it operationalizes **how you yourself get stronger**. After you publish your version, that story is yours to tell in every interview.

## What mastery looks like

You should be able to take any thin internship or coursework project and run it through the 6-stage qualify pipeline until it can survive senior-level interview pushback. You should be able to maintain a master resume long enough that a fresh application takes you 30–60 minutes instead of half a day. And for every verb, every noun, every tech-list ordering in your resume, you should be able to say *why that choice and not the alternative*. The resume stops being a document you rewrite from scratch each time, and becomes a byproduct of a longer career-management flywheel.
