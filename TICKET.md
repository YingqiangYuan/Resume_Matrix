# TICKET: 1+N Resume Matrix Methodology — Learning Checklist

[Tutorial](https://github.com/easyscale-academy/learn_build_resume_matrix-project/tree/01-Learn-This-Project/)

## Objective

Absorb the 1+N resume matrix methodology end-to-end: the 13-lesson curriculum, the 10 first-class agent skills that operationalize it, and the worked example under `students/john-doe/`. Finish by publishing your own sanitized version of this repo on your GitHub.

## Checklist

### Setup
- [X] Clone the repo and switch to the `01-Learn-This-Project` branch
- [X] Install `mise` (`curl https://mise.run | sh`) and add it to your shell rc
- [X] Run `mise install` at the repo root to pick up Python 3.12 + uv
- [X] Run `mise run venv-create` to create `.venv/`
- [X] Run `mise run inst` to sync deps (no-op today, kept for forward compatibility)
- [X] Confirm Claude Code can see the skills: open the repo root in Claude Code and check that the 6 `/learn-this-project-*` skills and the 10 first-class workflow skills appear in the `/` menu

### Absorb (learn the content)
- [X] Run `/learn-this-project-absorb` in **Orient mode** for the high-level map and the `files to READ` vs `files to RUN/DO` lists
- [X] Work through every file on the run-list: read the 13 lessons under `examples/01-hiring/` through `examples/13-final-synthesis/` in order, then skim `students/john-doe/` to see what each skill's output looks like
- [X] Come back to `/learn-this-project-absorb` in **Context-dive mode** whenever a specific spot needs unpacking (give it a `file:line` or paste the code snippet)
- [X] Be able to explain **why the 1+N method derives targeted resumes by deletion only, never addition** — i.e., why subtraction scales and what internal-consistency property addition would break (lesson `examples/05-resume-matrix/`)
- [X] Be able to explain **why Summary is reverse-engineered from existing Bullet Sets, not written first** — i.e., what aspirational-vague failure mode the reversed order prevents (lesson `examples/10-write-summary/`)
- [X] Be able to explain **why `qualify-coach` and `qualify-mock-interview` must be invoked in separate Claude Code conversations** — i.e., what the coach's accumulated memory does to interview verdicts (`docs/learn-this-project/01-knowhow-inventory.md` § qualify-coach Gotchas)
- [X] Be able to explain **why the writer/reviewer pairing pattern exists** (bullet-writer ↔ bullet-reviewer, summary-writer ↔ summary-reviewer, mini-project-design ↔ mini-project-review) and what asymmetry the design ↔ review file-based loop enforces

### Quiz (verify understanding)
- [-] Run `/learn-this-project-quiz` in **Bank mode** and clear the floor — a 10-question round with no ⚠️ partial / ❌ wrong, where every answer hits the 3-part standard (**where to look + what + why**), not just a factually correct one-liner
- [-] Use **Open-ended mode** to drill 2–3 topics where you came up shallow (likely candidates: the elevate vs from-scratch mode split, the gap severity 🔴/🟡/🟠 convention, the inline rationale blockquote convention)
- [-] If anything keeps coming up partial, go back to the relevant `examples/` lesson or `docs/learn-this-project/01-knowhow-inventory.md`, then re-quiz

### Elevate (see what's beyond)
- [ ] Run `/learn-this-project-elevate` and explore 1–2 upgrade directions from `docs/learn-this-project/03-elevation-roadmap.md` (the highest-priority candidates: validation evidence beyond john-doe, schema and link-integrity tests for the curriculum, a Chinese↔English parity check)
- [ ] **Converge each chosen direction into a concrete starter deliverable** — e.g. "a `tests/test_skill_frontmatter.py` that asserts every `SKILL.md` has `name`/`description`/`allowed-tools`/`argument-hint` and `name` matches the directory"
- [ ] (Optional, high-value) Hand the deliverable to `/learn-this-project-absorb` in **Build mode** and actually build the first iteration
- [ ] Note down "next small projects" that interest you (e.g., adding a second worked student covering a career-changer)

### Interview (pressure-test yourself)
- [ ] Run `/learn-this-project-interview` and complete a full mock session — calibrate it for the role/seniority you actually expect to interview for
- [ ] Review the debrief; for the 3 weak-spot questions, return to quiz / absorb and re-cover the gap, then re-run the relevant round

### Demo (learn to present)
- [ ] Run `/learn-this-project-demo` and rehearse at least the 5-minute version against the audience type you're most likely to face (typically hiring manager — related-domain)
- [ ] Walk the cardinal-rule "do NOT show" list with the skill: confirm that `docs/learn-this-project/`, the five `learn-this-project-{absorb,quiz,elevate,interview,demo}` skill directories, `README-cn.md`, and `README-ORIGINAL.md` are **never on screen** during the demo (the only safe path is rehearsing against your published, sanitized repo)

### Mastery Gate
- [ ] You can answer ~70% of quiz questions to the 3-part standard (**where + what + why**), not just factually
- [ ] You can survive at least one pushback round per interview question
- [ ] You have a clear list of "what I'd study next" from the elevate session, each item with a concrete starter deliverable
- [ ] You can deliver the demo without notes, without exposing any cardinal teaching artifact

### Publish (turn it into a portfolio artifact)
- [ ] Decide on a new public repo name (suggested pattern: `<firstname>-<lastname>-resume-matrix-poc`)
- [ ] Run `/learn-this-project-publish` in **Transform mode** — the skill walks you through:
  - [ ] Intake: new repo name + your name / byline
  - [ ] Delete cardinal teaching artifacts (dry-run shown, then consent-gated `rm -rf`)
  - [ ] Borderline review (your call on each project-specific file)
  - [ ] Generate `tmp/publish-commit-plan.md` (your copy-paste cheat-sheet for the 10–15+ commits)
  - [ ] Co-write your `README.md` in your own voice (D-mode — the skill asks, you answer, it drafts, you edit)
- [ ] Verify **Audit mode** returns 0 🔴 HIGH RISK findings before publishing
- [ ] Create the public GitHub repo yourself (the skill won't do this)
- [ ] Open `tmp/publish-commit-plan.md` and run the 10–15+ commits one at a time
- [ ] `git remote add origin <github-url>` and `git push -u origin main`
