# MaternaPulse BI Agent: Cedar Ridge 实习的拔高版

> 这个文件夹是 John Doe 把 2025 年夏天 Cedar Ridge 妇产科那段薄实习经过 deepen 工作流拔高之后的产物集合。原始薄经历在 [`../from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting-cn.md`](../from-2025-06-to-2025-09-CedarRidge-maternity-sql-reporting-cn.md)。

## 文件夹结构说明

```
from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/
  README-cn.md                                            ← 本文件，结构说明
  qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/
    job-description.md                                    ← 输入：目标 JD
    landscape/                                            ← 阶段 1 产出：JD 背后的 landscape 调研
      00-title-cn.md
      01-industry-cn.md
      02-company-cn.md
      03-role-cn.md
      04-market-cn.md
    01-gap-analysis-cn.md                                 ← 阶段 2 产出：gap 诊断
    case-cn.md                                            ← 阶段 3 产出：elevated 项目设计（bullet 写作的源头）
    02-gap-fill-plan-cn.md                                ← 阶段 4 产出：gap 填充计划
    pocs/                                                 ← 阶段 4-5 产出：mini-POC 实操
      poc-01-strand-agents/README-cn.md
      poc-05-semantic-yaml/README-cn.md
      ...
    tutorials/                                            ← 阶段 4 产出：每个 POC 配套教程
      01-strand-agents-quickstart-cn.md
      05-semantic-layer-yaml-design-cn.md
      ...
```

## 为什么是这个结构

同一段经历可以为不同的目标 JD 拔高出**不同的形态**。每个 `qualify-for-<job>/` 子文件夹对应一次「针对某个具体岗位的 deepen」，里面是那次 deepen 跑出来的全部产物。一段经历可以有多个 qualify-for 子文件夹并存。

目前只有一个 qualify-for 例子（针对 2026 年 2 月 Cascadia Health Insights AI Solutions Engineer 岗位）。后续如果 John 想用同一段经历投另一家公司，会在这里新增一个 `qualify-for-<新公司-岗位>/` 子文件夹，再跑一遍工作流。互不打架。

## 想看 elevated 后的 case 怎么写

打开 [`qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case-cn.md`](qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case-cn.md)。这是 1 篇万字长文的整合版 case，涵盖背景、团队、做了什么、产出、技术决策回放、技术栈。这个文件是 [resume.md](../../resume.md) 里 Bullet Set 2 和 Bullet Set 3 的源头。

## 想看 deepen 工作流是什么

打开 [`../../../../examples/07-elevate-existing-project/README-cn.md`](../../../../examples/07-elevate-existing-project/README-cn.md)，那一节专门讲 deepen 工作流的逻辑，并把这个文件夹里的产物作为实例串起来。
