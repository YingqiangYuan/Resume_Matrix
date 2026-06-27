# Resume Matrix 课程整体 outline (01-13)

> 这份文档是整个 examples 系列 13 节课的路线图。01-08 已经写完, 09-13 是后续待写。Context 满或换新 conversation 时回来读这份文档就能立刻接上进度。

## 01-04: 理论基础 (已完成)

| 节 | 主题 | 教什么 |
|---|---|---|
| 01-hiring | 招聘流程 | 美国招聘流程, 不同场景下的招聘官心智, 简历给谁看 |
| 02-ats | ATS 系统 | 破除 ATS 误区, AI 时代的简历筛选, 平台行为信号 |
| 03-new-grad | 应届生简历 | Action+Tech+Impact 公式, 多版本简历需求, 项目至上 |
| 04-experienced | 1-5 年简历 | 从 "做了什么" 转向 "做成了什么", Impact 量化 |

## 05-08: 实操工作流 (已完成)

| 节 | 主题 | 教什么 |
|---|---|---|
| 05-resume-matrix | 1+N 简历法 | 维护一份 master + 多份针对性派生, 操作全是删减 |
| 06-prepare-project-material | 项目素材三条路 | 方法一导师设计, 方法二拔高已有, 方法三自己设计 |
| 07-elevate-existing-project | 方法二完整工作流 | 6 阶段链路 + 5 个 agent skill, John Doe Cedar Ridge → Cascadia 实例 |
| 08-design-new-project | 方法三完整工作流 | 同一套 6 阶段工作流, 起点从 "有薄经历" 换成 "白纸", Pulse Social 实例 |

## 09-13: 写 bullet 与持续维护 (待写)

| 节 | 主题 | 教什么 |
|---|---|---|
| 09-write-bullets | 从 case 写 bullet | 给定项目文档 (case-cn.md), 怎么压缩成简历上的 3-4 条 bullet。Action + Tech + Impact 三件套在文档级别落地。AI 工具怎么把万字 case 压成 200 字 bullet 而保持可追问性 |
| 10-write-summary | 写 summary | 同一个 case 对应不同岗位方向的多份 summary。Summary 是定位标签, bullet 是证据, 两者怎么配合 |
| 11-submit-and-collaborate | 投递 + 跟导师协作 | 投递时怎么用 master + N 份针对性简历。跟导师在 Google Doc 和 GitHub repo 上的双轨协作: GitHub 管内容 (源头), Google Doc 管排版。Review 周期、改 bullet 排版的工作流 |
| 12-maintain-new-projects | 后续新项目维护 | 当你做了新项目 (新实习、新 side project), 怎么把新经历接入到现有的 master + qualify-for 结构里。多个 qualify-for 子目录怎么共存 |
| 13-final-synthesis | 汇总 + 持续改简历的闭环 | 把 01-12 重新串一遍。学生在职业生涯里持续改简历的闭环: 设计 mini 项目 → 补技能 → 把项目变成 case → 写成 bullet → 派生针对性简历 → 投递 → 反馈 → 再设计 mini 项目。这是 13 节课的终点, 也是工程师终身职业管理的起点 |

## 当前文件结构 (截至 2026-06-26)

```
examples/
  01-hiring 到 08-design-new-project (已完成)
  _outline.md (本文件)

.claude/skills/
  _workflow.md (架构 spec, 5 个 skill 共用)
  mini-project-design / mini-project-review / qualify-gap-plan / qualify-coach / qualify-mock-interview
  markdown-style / write-agent-skill (现有)

students/john-doe/
  resume.md (拔高后 master, 6 bullet sets)
  resume-old.md (拔高前对照)
  resume-role-1.md 到 -4.md (4 份针对性派生)
  experiences/
    CedarRidge-sql-reporting (薄经历, 07 输入)
    CedarRidge-bi-agent/
      README-cn.md
      qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/
        job-description.md
        landscape/00 到 04 (5 篇完整)
        01-gap-analysis-cn.md / case-cn.md / 02-gap-fill-plan-cn.md
        pocs/poc-01 加 poc-05 (示例 2 个)
        tutorials/01 加 05 (示例 2 个)
    NovaRisk-bi-agent (5 文档导师设计模板)
    Pulse-feed-ranker/
      README-cn.md
      executed-case-cn.md (成熟版)
      qualify-for-Pulse-Social-Backend-Engineer-Intern/
        job-description.md
        landscape/00-title-cn.md (abbreviated)
        case-cn.md (forward-looking 设计)
        01-gap-analysis-cn.md / 02-gap-fill-plan-cn.md (stubs)
```

## 关键设计决策档案

### 1. 文件命名约定
- 中文内容: `-cn.md` 后缀
- 英文内容: `.md` 无后缀
- Agent skill 源文件: 始终纯英文, SKILL.md 不加后缀

### 2. understand-landscape skill 在哪
**重要**: `understand-landscape` 是前置课程 (career_planning) 教的 agent skill, NOT 本 repo 的内容。本 repo 里 07/08 只引用它, 假设学生已经会用。

### 3. 5 个 skill 的双向独立 context 规则
- `mini-project-design` ↔ `mini-project-review`: 必须开两个 terminal/conversation 来回 review, 至少 3 轮迭代直到 review 给 "approve"
- `qualify-coach` ↔ `qualify-mock-interview`: coach 教 + mock interview 考, 必须独立 context, 防止考试官提前知道学生哪里学得弱

### 4. POC 在本课程的定义 (区别于 "假装的业务项目")
POC = Proof of Concept, 在 `qualify-gap-plan` 产出的 mini-POC 中特指: 一个聚焦 1 个技能点的小练习项目 (例如"学 Strand Agents 用 Wikipedia 语料写问答 agent")。不需要企业背景, 不模拟业务场景, 唯一目的是证明你能用这个技能。

### 5. mini-POC 加教程的真正目的 (容易被误解)
教程**不是**让你在改简历之前就把所有技能学透。它的首要目的是让你**用 AI 帮助试着学一学、感觉一下**, 看这个项目设计**是不是 3-12 个月够得着**。如果上手就觉得"完全摸不到边", 说明设计太难, 应该回到 `mini-project-design` 让 AI 修改设计降难度。

这是"小步迭代、快速验证"的哲学: **先试, 觉得不对赶紧调难度**, 因为如果设计本身不靠谱, 后面真做的时候回头改成本极高。

## Context 续航笔记

如果新 conversation 进来需要快速 ramp up:
1. 读这份 `_outline.md` 了解整体进度
2. 读 `.claude/skills/_workflow.md` 了解 5 skill 架构
3. 读 memory 里 2 条 (CN/EN 命名约定, 5-skill 架构决策)
4. 重点未完成事项: 09-13 还没写; 07 + 08 已经写完但有用户反馈待落实
