# learn_build_resume_matrix

这个 repo 教你一套面向美国求职市场的简历方法论 —— **1+N resume matrix**：维护一份"长得离谱"的 master resume，里面存着多个 Summary Variant 和多个 Bullet Set；任何一份投给具体公司的 resume 都通过**只做删减、不做新增**从 master 里派生出来。配套还有一整套 agent skill，把整套流程从"诊断差距 → 设计项目 → 学习补强 → 模拟面试 → 写 bullet/summary"操作化成可以反复跑的工具。

这不是一个"简历模板大全"，也不是"教你怎么用 ChatGPT 写简历"。它教的是一套**可以扛住面试官追问**的简历生产流程。

## What is this — 30 秒看懂学习方法

这是一个 "learn-this-project" 风格的 repo：一个**刻意做小**的小型代码库，端到端教一个垂直主题。重点不是把代码跑起来，而是通过**吸收**完成这件事 —— 把它读完、跑过、能为里面每一个设计选择辩护，最后在自己的 GitHub 上发布一份属于自己的版本。

整个流程由 6 个 interactive skill 串起来：

- **`/learn-this-project-absorb`** —— repo 的 on-call mentor。多模式：Orient（给你地图 + "files to READ" vs "files to RUN/DO" 列表）、Context-dive（你给一个 `file:line`，它帮你拆解那个点）、Next-step、Build（帮你扩展 repo）。是个导师，不是课表 —— 你需要帮助的时候去找它，不要把它当成线性课程从头跟到尾。
- **`/learn-this-project-quiz`** —— 讨论式问答。每个答案都按 **"where to look + what + why"** 的三段式标准打分。一句话答对一个事实不算过关。两种模式：题库模式（兜底检验）和开放模式（你点 topic，它现场出题）。
- **`/learn-this-project-elevate`** —— 看到这个 repo 当前状态之外的更高水平版本。每个升级方向它都会走一遍 current state → senior target → alternatives → 前置知识，最后**收敛到一个可立刻动手的 starter deliverable**，让你拿回 Absorb 的 Build mode 里真的把它建出来。
- **`/learn-this-project-interview`** —— 全 repo 的 mock interview，带 pushback。检验你能不能把这套东西讲给陌生人听。
- **`/learn-this-project-demo`** —— 帮你写出一份现场 demo 的脚本；最高价值的部分是"不要展示哪些教学痕迹"的 cardinal-rule 清单。
- **`/learn-this-project-publish`** —— 把这个教学 repo 转换成你自己 GitHub 上的 portfolio 版本。删教学痕迹、生成 commit 清单（你自己 copy-paste 跑）、用你的口吻协作写 README、最后跑一遍 hostile-scan audit。

**推荐顺序**：absorb → quiz → elevate → interview → demo → publish。但记住这是 6 个**待命的导师**，需要时再叫它们 —— 不是按顺序坐穿的课程。

## What's in this repo

```
learn_build_resume_matrix-project/
├── examples/                    # 13 节课程，主题的脊柱
│   ├── 01-hiring/               # 谁在读你的简历（Recruiter / HM / ATS）
│   ├── 02-ats/                  # AI 时代的 ATS 真相
│   ├── 03-new-grad/             # 新毕业生怎么写
│   ├── 04-experienced/          # 1-5 年经验的人怎么写
│   ├── 05-resume-matrix/        # 1+N 方法（核心架构课）
│   ├── 06-prepare-project-material/   # 项目素材的三条来源路径
│   ├── 07-elevate-existing-project/   # 6-stage elevation pipeline
│   ├── 08-design-new-project/         # 6-stage from-scratch pipeline
│   ├── 09-write-bullets/        # 把 case 压成 3-4 条 bullet
│   ├── 10-write-summary/        # 从 bullet 反推 Summary
│   ├── 11-submit-and-collaborate/     # GitHub → Google Docs → PDF
│   ├── 12-maintain-new-projects/      # 1→N 维护循环
│   └── 13-final-synthesis/      # 把飞轮转起来
├── students/john-doe/           # 一个完整的虚构案例（3 个项目，1 master + 4 衍生简历）
├── .claude/skills/              # 10 个一等公民 agent skill（主题的另一半）
│   ├── qualify-gap-analyze/     # 诊断 resume 和 JD 之间的差距
│   ├── mini-project-design/     # 设计 3-6 个月的项目（elevate / from-scratch 两种模式）
│   ├── mini-project-review/     # 独立评审 mini-project-design 的产物
│   ├── qualify-execution-plan/  # 把差距 + case 翻译成具体执行计划 + POC
│   ├── qualify-coach/           # 对话式逐概念教学，闭一个差距开下一个
│   ├── qualify-mock-interview/  # 高压模拟面试，永不在面试中教学
│   ├── bullet-writer/           # 把 case 压成一组 bullet（带 inline rationale）
│   ├── bullet-reviewer/         # 扮演 hiring manager 对 bullet 做 JD 定向微调
│   ├── summary-writer/          # 从已写好的 bullet 反推 Summary Variant
│   └── summary-reviewer/        # 扮演 hiring manager 对 Summary 做 JD 定向微调
├── docs/learn-this-project/     # 7 份分析文档（meta-skill 生成，是 6 个 skill 的知识库）
├── CLAUDE.md / mise.toml / pyproject.toml  # 最小化的项目脚手架（运行依赖为零）
└── README.md / README-cn.md     # 你正在读的这份
```

**两条平行的主题轴**：

1. **抽象工作流** —— 13 节课讲清楚整套流程的逻辑（为什么 Summary 反推、为什么 bullet 是 B1→B4、为什么 elevate 锁公司只改技术决策）。
2. **10 个一等 skill** —— 把上述工作流操作化的工具。它们脱离课程也能单独使用，对任何 (case, JD) pair 都成立。

`students/john-doe/` 是把上述两轴端到端跑过一遍的产物 —— 你看到每个 skill 写出来的文件长什么样、放在哪。

## Tech stack + setup

| 用途 | 选型 |
| :--- | :--- |
| Python | 3.12（mise 管理） |
| Python 包管理 / venv | uv（mise 管理） |
| 工具版本 + task runner | mise |
| 跑 skill 的客户端 | Claude Code |

整个 repo **没有任何运行时依赖**（`pyproject.toml` 的 dependencies 是空的）。所有"系统"就是你的文件系统 —— skill 跑完就是几个 markdown 文件落盘。

```bash
git clone <repo url>
cd learn_build_resume_matrix-project
mise install                # 装 Python 3.12 + uv
mise run venv-create        # 建 .venv/
mise run inst               # uv sync（目前空跑，为以后留口子）
```

之后用 Claude Code 打开 repo 根目录就能用 `/<skill-name>` 调任何 skill。

## Recommended learning flow

走一遍 6 个 skill 的推荐顺序：

1. **`/learn-this-project-absorb`**（Orient 模式起步）—— 先拿到 repo 的地图和 read/run 两个列表。然后**离开聊天**，自己去把 examples/ 里的 13 节课读一遍、把 students/john-doe/ 翻一遍。读到具体某一段卡住了，再回来用 Context-dive 模式问。
2. **`/learn-this-project-quiz`** —— 先跑 Bank 模式当兜底检验，能用 where + what + why 三段式答出来才算过。哪里答得虚就用 Open-ended 模式专门刷那个方向。
3. **`/learn-this-project-elevate`** —— 挑 1-2 个升级方向（比如"加上自动校验 SKILL.md frontmatter 的 pytest"、"加第二个 worked example 覆盖职业转型路径"）。每个方向都让它收敛到具体的 starter deliverable。
4. **（可选高价值步骤）** 把 elevate 收敛的 deliverable 直接喂给 `/learn-this-project-absorb` 的 **Build mode**，把第一个可跑版本真的建出来。这是把"理解"变"作品"的桥。
5. **`/learn-this-project-interview`** —— 完整跑一轮 mock interview。Debrief 里如果某个题答得弱，回 quiz / absorb 把那块再补一遍。
6. **`/learn-this-project-demo`** —— 排练 demo 脚本，特别是走一遍 cardinal-rule "不要展示哪些教学痕迹"清单。

> **提示**：如果你之前跑过早期版本的 learn-this-project 课程，现在的几个 skill 已经升级过：absorb 变成多模式（不再是线性课程）、quiz 变成 where+what+why 三段式打分（加了开放模式）、elevate 收尾要 converge 到 starter deliverable、publish 是新加的第六个 skill。

## Publish —— 把它变成你自己的 portfolio

学完之后，最值钱的一步是把这个教学 repo 转换成**你自己 GitHub 上的一份 portfolio 作品**。这一步全部交给 `/learn-this-project-publish` 自动化处理：

- **删除教学痕迹** —— `docs/learn-this-project/`、五个 `learn-this-project-{absorb,quiz,elevate,interview,demo}` skill、`README-cn.md`、`README-ORIGINAL.md` 等。每一步都先 dry-run 给你看，再问你 yes/no。
- **生成 commit 清单** —— 把改动重新组织成 10-15+ 个按依赖顺序排的 commit，写到 `tmp/publish-commit-plan.md`，你自己 copy-paste 跑（skill 不碰任何 `git` 命令）。
- **协作写 README** —— 用你的口吻而不是教学口吻：它逐节问你 2-4 个问题，根据你的回答起草，再让你逐节确认/改稿，确保最终 README 听起来像你写的。
- **Hostile-scan audit** —— 假装是个怀疑你"是不是套了教程"的面试官，把 repo 扫一遍报告 🔴 HIGH / 🟡 MEDIUM / 🔵 LOW 风险点。HIGH 没清零之前不算发布完成。

**cardinal rule（红线）**：发布出去的 repo 不能被读出来是个教学项目 —— 一旦被察觉，"我学过这套东西"的信号就翻转成"我跑了个教程"的负面信号。Publish skill 的整个设计就是围绕这条红线打造的。

## 把这套方法论本身当作面试卖点

学完之后，这份 repo 给你的不止是"会改简历"。它本身就是一个**可以拿出来直接吹的方法论展品** —— 当面试官问你"在 AI 时代你是怎么提升技能的"，你可以直接把这套逻辑摊给他看：

> 我做事一直**以终为始**。很早就研究过你们这个岗位的 JD，用 AI 帮我分析我和这个岗位之间的 Gap，再让 AI 帮我制定一份学习计划，然后跟着计划做项目、补短板。这就是我的**目标导向**学习法 —— 中间一直用 AI **recursively 拆解**每一层问题，一层一层往下钻，直到我把这块真的变强为止。
>
> 具体怎么干：我会先手搓一个 **master agent**（比如 `qualify-gap-analyze`、`mini-project-design` 这种），让它帮我生产更细分的**专家 agent**（比如 `qualify-coach`、`qualify-mock-interview`），再让这些专家 agent 反过来教我、追问我、检验我。AI 是我的工具，我是工具的主人 —— 不是反过来。

这种**以终为始 + 用 AI 拆 AI** 的工作姿态，恰好是 AI 时代企业最缺、最贵的那一类人才画像。所以记住一件事：这个 repo 不只是教你简历方法论，它教你的是 **"我自己怎么变强"** 这件事本身可以怎么操作化。**publish 之后，这套方法论是你能讲给任何面试官听的故事。**

## 学完什么样算掌握

你应该能把任何一段薄的实习/课程项目，通过 6-stage qualify pipeline 升级到能扛 senior 面试官追问的水平；能维护一份长得离谱的 master resume 让每次新申请只花 30-60 分钟；能为简历上每一个 verb、每一个 noun、每一条 tech list 排序说出"为什么是这个不是那个"。简历不再是一份每次重写的静态文档，而是你长期职业管理飞轮的副产品。
