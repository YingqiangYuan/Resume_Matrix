# MaternaPulse BI Agent, the Elevated Version of the Cedar Ridge Internship

> This folder holds everything John Doe produced after running his thin summer 2025 Cedar Ridge maternity internship through the elevation workflow. The original thin experience lives in [`../from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting.md`](../from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting.md).

## Folder Layout

```
from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/
  README.md                                               ← this file, folder layout
  qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/
    job-description.md                                    ← input: target JD
    landscape/                                            ← stage 1 output: landscape research behind the JD
      00-title.md
      01-industry.md
      02-company.md
      03-role.md
      04-market.md
    gap-analysis.md                                       ← stage 2 output: gap diagnosis
    case.md                                               ← stage 3 output: elevated project design (source for bullet writing)
    execution-plan.md                                     ← stage 4 output: gap-fill plan
    pocs/                                                 ← stage 4 to 5 output: hands-on mini POCs
      poc-01-strand-agents/README.md
      poc-05-semantic-yaml/README.md
      ...
    tutorials/                                            ← stage 4 output: one tutorial per POC
      01-strand-agents-quickstart.md
      05-semantic-layer-yaml-design.md
      ...
```

## Why This Structure

The same experience can be elevated into different shapes for different target JDs. Each `qualify-for-<job>/` subfolder corresponds to one elevation pass aimed at one specific role, and contains every artifact produced during that pass. A single experience can hold multiple qualify-for subfolders side by side.

Right now there is only one qualify-for example (targeting the February 2026 Cascadia Health Insights AI Solutions Engineer role). If John later wants to use the same experience for a different company, he will add a new `qualify-for-<new-company-role>/` subfolder here and run the workflow again. The two passes do not interfere.

## Want to See What the Elevated Case Looks Like

Open [`qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case.md`](qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case.md). It is a single long-form integrated case (around ten thousand words) covering background, team, what was built, deliverables, a technical decision replay, and the tech stack. This file is the source for Bullet Set 2 and Bullet Set 3 in [resume.md](../../resume.md).

## Want to See What the Elevation Workflow Is

Open [`../../../../examples/07-elevate-existing-project/README.md`](../../../../examples/07-elevate-existing-project/README.md). That section walks through the logic of the elevation workflow and uses the artifacts in this folder as the running example.
