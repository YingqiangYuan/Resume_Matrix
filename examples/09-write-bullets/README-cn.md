# 从 case 文档到简历 bullets: 一段经历压成 3 到 4 条

> 这是 examples 系列里的第九篇. 前置: [07-elevate-existing-project](../07-elevate-existing-project/README-cn.md) 或 [08-design-new-project](../08-design-new-project/README-cn.md) 已经跑通, 你手里有一份 `case-cn.md` 拔高版加一份 `job-description.md`. 这一篇专门讲怎么把这份万字 case 文档压成简历上的 3 到 4 条 bullet, 而且保证这些 bullet 经得起面试官追问.

## 1. 课程导读

07 和 08 教完之后, 你手里其实已经有了简历最贵的素材: 一份对着目标 JD 拔高过的 `case-cn.md`. case 文档动辄五千字, 涵盖业务背景, 技术决策, 产出指标, 反思. 但简历上**这段经历你只有 3 到 4 条 bullet 的位置**. 这一节就专门做这个压缩.

为什么把 bullet 单独拎出来一节? 因为 bullet 是简历真正的核心. Hiring manager 6 秒扫 summary 决定要不要继续往下看, 一旦继续往下看, 决定"这个人值不值得花 30 分钟做电话面试"的就是 bullet. Summary 让人想继续看, bullet 让人想面试你.

### 为什么 09 教 bullets, 10 才教 summary

很多简历课会反过来教: 先教 summary 后教 bullet. 我们故意倒过来. 原因不复杂:

- **bullet 是证据, summary 是给证据贴的定位标签**. 没有证据就贴标签, 标签是空的, 抽象的. 学生会卡住 ("我是 backend engineer 还是 fullstack? 我擅长 ML 还是 data?"), 因为没有具体的项目 bullets 摆在桌上, 这些定位问题答不出来.
- **写完 bullets 之后, summary 几乎是自动的**: 翻一眼你这段经历的 4 条 bullets, 看里面**反复出现的关键词, 技术栈, 问题领域**是什么, summary 就有了.
- 反过来: **先写 summary 然后逼自己"按 summary 写 bullet", 你会下意识把 bullet 往 summary 的标签上靠**, 导致 bullet 失真. 你做的事是这样的, 但你硬要写成 summary 标榜的那样.

所以教学顺序是 case → bullets → summary. 证据先于标签.

---

## 2. bullet 是简历真正的核心

Hiring manager 看 bullet 的时候, 他脑子里其实在找 4 个问题的答案:

- 这个人做了什么? (做的事情大概是什么形态)
- 用了哪些技术栈, 跟我们要的对不对得上?
- 项目本身有没有产生真实结果, 有多深?
- 这个人在项目里的角色是什么, ownership 多大?

这 4 个问题的答案全在 bullet 里. case 文档里答案在但太散; bullet 是把答案**编排好, 压缩好, 放在面试官最少的视线时间里**的工程产物.

但 bullet 压缩有个核心难点: **你要把万字 case 压成 200 字, 而且每个具体声称都还要经得起面试官追问**. 这跟"写得简短"是两回事. "简短而能追问"是工程, 不是写作; 这一节后面用的 skill 加流程, 解决的就是这个工程问题.

---

## 3. 一段经历几条 bullet, 内部结构怎么排 (本节最关键的一段)

这一节是 09 真正的新东西, 也是你看完之后**就该改你现在简历**的部分.

> **给零基础读者**: 下面所有 bullet 范例都用 John Doe 的医疗 AI 项目作场景 (Strand Agents, Bedrock, semantic layer, Charge Nurse 这些名词都是他项目里的具体技术和角色). 你不需要看懂每个技术词, 重点感受 4 条 bullet 之间从 "项目画面" → "设计决策" → "硬问题" → "ownership" 的**递进结构**, 这个结构换成你自己项目的术语 (Python 微服务也好, ML 模型也好, 产品设计也好) 都成立. skill 看你的 case 文档自己会换术语, 不会硬套医疗.

先说**条数**: 一段经历**典型情况下写 3 到 4 条**, 薄经历只够写 2 条, 偶尔旗舰项目写到 5 条. **没有"必须 4 条"这个规则**. 条数取决于你这段经历能撑起几个不同角度的亮点, 强行凑数会稀释. 下面用 4 条做例子讲递进逻辑, 但同样的逻辑用在 3 条或 2 条上一样适用 (砍掉相对最弱那一条即可).

也说一下**长度**: 每条 bullet 写出来应该在简历上占 **1 到 2 行**, 极少数王牌亮点可以到 3 行 (例如这段经历的核心卖点, 信息密度高到删任何一段都掉档次). 具体多少词数取决于简历排版, **这个阶段不要纠结字数**, 内容成型就行, 排版微调留到 11 投递课.

很多人写一段经历的 bullet, 写完是这样的:

> 不推荐写法 (平行清单, 这里举 4 条但 3 条 / 5 条问题一样):
>
> - Built LLM-powered BI agent on AWS Bedrock and Strand Agents framework, achieving 93% accuracy on a 50-question medical reporting eval set.
> - Implemented retry-with-feedback loop integrating Snowflake error messages back into the agent prompt, improving zero-shot SQL generation accuracy from 78% to 93%.
> - Designed YAML-based semantic layer abstracting 40+ clinical metrics from underlying Snowflake schema, reducing analyst onboarding time by 60%.
> - Deployed agent service to AWS ECS with CloudWatch monitoring, achieving 99.7% uptime over an 8-week production rollout.

每一条单独看都不错: 有动词, 有技术栈, 有 impact 数字, 公式上都对. 问题在于**4 条放一起是一个清单, 不是一个故事**:

- HR / 非技术招聘官扫第一条, 没法在脑子里形成"这是个什么项目"的画面 (LLM agent? medical reporting? 谁用? 解决什么?)
- 每条都是"Used X to do Y getting Z%"一个模子, 4 条互相之间可以打乱顺序也没区别
- 没有递进: 看完第 4 条你对这个项目的理解不会比看完第 1 条更深, 只是知道了又一个技术栈

正确的写法是把这 4 条**按"越来越深"的递进逻辑组织起来**, 第一条给一个 HR 都看得懂的画面, 后面 3 条沿不同维度往下挖.

### 正确结构: B1 是画面, B2-B4 是递进的横切面

**Bullet 1: HR-readable 项目画面**

这一条的读者是**非技术的 HR, recruiter, 或偏业务的 hiring manager**. 他扫一眼必须能立刻形成"这人做了个什么事"的画面. 结构是:

> 为了 \<业务问题或客户场景\>, 做了 \<一个能被名词化的东西\>, 用 \<2 到 3 个核心技术栈\>, 达成 \<一句话 impact\>.

注意 4 个要素都要有, 但都要短.

> 推荐写法, B1 范例 (同一个 MaternaPulse BI agent 项目):
>
> Built MaternaPulse, an LLM-powered natural-language BI agent for Cedar Ridge's 12-unit maternity hospital network on AWS Bedrock + Snowflake, letting nurse managers query daily census, staffing, and supply data in plain English instead of waiting on custom SQL reports, cutting analyst report turnaround from 2 days to under 5 minutes on routine questions.

非技术招聘官扫这一条: "医疗加 AI 加自然语言查询加节省时间", 画面成型了, 决定继续往下看.

**Bullet 2: 关键技术决策 (展示思考深度, 不是技术堆砌)**

这一条专门展示**一个非平凡的设计选型**. 不是"我用了 X", 是"在 X 和 Y 之间, 我选了 X, 因为..."的简短版本.

> 推荐写法, B2 范例:
>
> Designed a YAML-based semantic layer decoupling 40+ clinical metric definitions from the underlying Snowflake schema, letting analysts onboard new metric questions by editing 30-line YAML files instead of touching agent code, and grounding the LLM's generated SQL in business-approved metric definitions rather than raw column names.

读者看到的是: "他不是无脑接 LLM 上去, 他在中间塞了一层 semantic layer 解决数据漂移问题", 立刻能问出有意义的追问 (为什么不用 dbt? 怎么 version control?).

**Bullet 3: 解决最硬的那个具体技术问题 (展示动手能力)**

这一条专门展示**项目里最非平凡的一个子问题**和你怎么解决的.

> 推荐写法, B3 范例:
>
> Implemented a retry-with-feedback loop using Strand Agents' tool-calling framework: when generated SQL hit runtime errors or returned obviously wrong row counts, the agent re-prompted itself with the database error message, raising end-to-end answering accuracy on the 50-question eval set from 78% (zero-shot) to 93%.

这一条让 hiring manager 知道你不是搬轮子的人, 你解决过一个**真硬的小问题**.

**Bullet 4: ownership 加跨团队 impact (展示项目 ownership 和真实影响范围)**

最后一条收尾, 展示**你在项目里的真实角色 + 项目跨出代码的真实影响**.

> 推荐写法, B4 范例:
>
> Partnered with 3 nurse managers, 2 analysts, and the data engineering team across 4 sprints to define the eval set, ground-truth answer keys, and rollout criteria; presented monthly demos to clinical operations leadership and shipped to production for 12 maternity units serving roughly 4,000 monthly inpatient cases.

读者看完 B4 知道: 这不是闷头写代码的工程, 这人能跟非技术方对齐, 能 own 一个跨团队的 rollout, 影响了真实的 4000 名病人.

### 4 条放一起的效果

你把这 4 条放一起再读一遍: B1 给整体画面, B2 展示设计, B3 展示动手, B4 展示 ownership. 读者大脑里逐渐生成的不是一个清单, 而是一个**立体的项目**和一个**立体的工程师**. 这就是好 bullet 的递进结构.

**核心原则总结**:

| | 平行清单 (避免) | 递进结构 (追求) |
|---|---|---|
| 每条角度 | 都是"Used X to do Y getting Z%" | B1 画面, B2 设计, B3 硬问题, B4 ownership |
| HR 扫 B1 | 看不出这是个什么项目 | 立刻形成项目画面 |
| 整体观感 | 一堆技术细节 | 一个立体的项目 + 工程师 |
| 顺序可换? | 是 (随便排) | 否 (打乱就失去递进) |

> 注: 这里给的是 4 条的模板, 不是死规定. 如果你的经历只够 3 条 bullet, 那就是 B1 + B3 + B4 (砍掉 B2, 因为没有非平凡设计决策可以展开); 如果你能写 5 条, 那就是 B2 拆成两条"核心架构 + 一个具体设计选型". 但 **B1 必须是 HR-readable 画面**, 这条不能砍.

---

## 4. bullet-writer 用法详解

### 4.1 退一步看: 改 bullet 本质上是把信息给 AI

具体讲 skill 用法之前, 先把这件事的本质捋清楚, 不然你只会照着模板按按钮, 不知道为什么这么做.

**改简历 bullet 这件事的本质是: 你把信息给 AI, AI 给你写好的 bullet**. 具体是:

- **输入** (信息): 你**现有的简历** (master `resume.md`, 里面已有的其他经历 bullet 是风格和密度参考) + **一段经历的 case 文档** + 项目的一切信息 + **可选**目标 JD (有 JD 时 bullet 会更针对那个岗位的关键词和强调点; 没有 JD 时 skill 写出的就是 Job Family 通用版). 注意"Job Family 通用版"要有代表性, 靠的不是这里省略掉 JD, 而是你的 case 文档本身在 07/08 阶段 1 就已经是针对整个 family 综合出来的, 见 [07 §4.1 "当目标是一整个 Job Family, 而不是一家具体公司时"](../07-elevate-existing-project/README-cn.md#当目标是一整个-job-family-而不是一家具体公司时).
- **输出** (3 到 4 条 bullet) 有 3 个落地位置可选:
  1. 直接吐到聊天框, 你手动复制
  2. 写到一份单独的文件 (例如 `qualify-for-.../bullets-cn.md`), 之后再合并到简历
  3. **直接合并写入你的 master `resume.md`** (本课程选这个)

为什么本课程选第 3 个:

- 跟其他经历的 bullets 放一起, 一眼能看到"我这段经历跟我别的经历放一起协不协调, 风格, 密度统一不统一"
- 同一段经历的 AI / Data / SDE **多个角度版本放在一起好对比** (John 的 master `resume.md` 里 Cedar Ridge 这一段就同时有 Bullet Set 2 "AI emphasis"加 Bullet Set 3 "Data Analytics emphasis", 两套并列才好看出差异化措辞)
- 维护成本最低: 后续要改一条 bullet, 直接在 master 里改, 不用追多份散落文件
- 跟 [05-resume-matrix](../05-resume-matrix/README-cn.md) 教的"**1 份 master + N 份派生**"一脉相承: master 是源头, 投递时按目标 JD 删减出派生版本, 永远只有一份事实

所以 `bullet-writer` 这个 skill 的运行机制是: 跟你互动写出几条 bullet (典型 3 到 4 条, 根据案例深度可以 2 到 5), 然后**直接 Edit 你的 master `resume.md`** (因为文件进 git, 改坏了能 revert 不怕), 在 §4 Experience 下新增一个 `### Bullet Set N, <公司>, <项目> (<角度> emphasis)` 段, **同时在 bullets 下面用 markdown blockquote (`>`) 写一段解说**, 讲清每条 bullet 的动词为什么这么选, 关键名词在打包什么概念, 量化数据怎么 defensible (industry baseline + 计算公式). 聊天里只发一句话提示"写好了, 请 git diff 验证", 详细解说全在文件里方便你以后回看.

> **小白说明**: 后面会反复出现 "请 git diff 验证" 这种话. `git diff` 是 Git 自带命令, 在终端 `cd` 到你的 repo 然后输 `git diff` 就能看到上次 commit 之后改了哪几行 (红色 = 删除, 绿色 = 新增). AI 用 Edit 工具改 markdown 偶尔会漏改 / 错位 / 误删别的段, `git diff` 是 30 秒内确认 "改对了没" 的最快方式. 如果你完全没用过 git, 在 IDE (VS Code / Cursor / JetBrains) 里也有 Source Control 面板, 视觉上是同一回事, 也行.

### 4.2 实操: 怎么用

**什么时候用**: 你跑完 07 或 08 拿到了 [`case-cn.md`](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case-cn.md), 同目录下还有 [`job-description.md`](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/job-description.md), 准备把这段经历的 bullets 写进 master [`resume.md`](../../students/john-doe/resume.md).

**用之前准备什么**:

- 你的 master 简历路径 (例如 [`students/john-doe/resume.md`](../../students/john-doe/resume.md))
- 这段经历的 case 文件路径
- (可选但强烈推荐) 目标 JD 路径, 让 skill 知道往哪个关键词靠
- 你想用哪个角度 (AI emphasis / Data Analytics emphasis / Software emphasis / ...), 即这个 Bullet Set 的 Job Family 定位
- 你的经验级别 (应届生用 Built / Implemented / Designed, 有 3 年以上的可以考虑 Led / Owned / Architected). skill 会从你 master 简历里自动推断, 但你不放心可以明确告诉它

**怎么调用**: Claude Code 终端里说人话. 仿照 John 的语气:

```
请调用 /bullet-writer skill.
我正在修改 students/john-doe/resume.md 这份 master 简历.

我想为 students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case-cn.md
这一段实习经历, 写出针对同目录 job-description.md 那个岗位的 bullets,
角度: AI emphasis (我打算投 AI Engineer / AI Solutions Engineer 那条 Job Family).

写完直接在 resume.md §4 Experience 下面新增一个 Bullet Set.
```

**过程中会发生什么**:

1. skill 读你 master `resume.md` + case + JD, 给你 3 条它从 case 里抽出来的"核心可迁移能力", 让你确认或调整 (例如"能从 0 搭 LLM agent""能设计 semantic layer 这种中间抽象""能跟非技术 stakeholder 跑迭代").
2. skill 起草初版 4 条 bullet, 显式标注 B1 / B2 / B3 / B4 各自承担什么 (HR 画面 / 设计决策 / 硬问题 / ownership), 同时贴出来给你看一眼你 master 简历里已经存在的其他 bullet set, 提示"我准备用跟 Set 4 / Set 6 同款的密度和动词强度, 你看 ok 不".
3. 你 push back, skill 调整. 常见 push back 类型:
   - "这个数字我面试答不出来怎么测的, 换一个"(skill 会换成你能 defend 的 impact 表述)
   - "这条用了太强的动词, 我是应届生不能写 Architected"(skill 会降级到 Designed / Built)
   - "B2 这条跟 B3 角度太像, 都是讲技术决策, 换一个角度"
   - "B4 没体现我跨团队的部分, 加上跟 nurse manager 跑了 4 个 sprint"
4. 收敛到你拍板的 2 到 5 条版本, skill **直接 Edit 你的 master `resume.md`**, 在 §4 Experience 末尾追加一个 `### Bullet Set N, <Company>, <Project> (<角度> emphasis)` 段, N = 你 master 里现有 bullet set 数加 1.
5. **bullets 段下面紧接着用 markdown blockquote (`>`) 写一段"Rationale for this Bullet Set"**, 把每条 bullet 的关键词选择讲清楚: 动词为什么这么选 (扮演什么角色, 跟你的级别配不配), 关键名词在打包什么概念 (sharper than 什么), 量化数字是怎么 defensible 的 (industry baseline 是什么, 计算公式是什么, 每个因子靠不靠谱). 这个 rationale 段是给你以后回看用的, 派生 role-specific 简历时整段删掉.
6. skill 在聊天里只发一段简短的话提示"Bullet Set N 写好了, 改了第 X-Y 行, 请 git diff 验证", **详细解说不在聊天里复述, 都在文件里**, 你看文件就行. 如果不满意就让 skill 改回去或调整.

**输出结果长什么样** (新增进 `resume.md` §4 末尾的段, bullets 加紧接着的 Rationale 整体写入文件):

> **下面这段是 *最终形态* 范例**: 30 行 markdown 带 4 条 bullet + 后面一大段 Rationale blockquote. 第一次看会觉得"太重了我永远写不出这种东西", 这个反应很正常. 关键提醒两件事: (1) skill 会自己起草整个 Rationale, 你的工作是 review + push back + 拍板, 不是凭空写; (2) 第一次跑出来的 Rationale 写得糙一点没关系, 关键是**有**这个区块, 以后可以回去补深. 别让 "看起来太完美" 这个心理障碍把你劝退到不开始.

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

注意标题里**显式编码了角度和目标 Job Family** (`AI emphasis, for Cascadia AI Solutions Engineer`), 这样你 master 简历里同一段经历的 AI / Data / Software 多个版本并列摆着的时候一眼能区分.

**翻车点**:

- 跳过"确认 3 条可迁移能力"直接让 skill 起草. skill 没有锚点, 起草出来的 bullet 会沿着 case 文档的章节顺序平铺, 退化成平行清单.
- 不告诉 skill 你是应届生还是有经验, 它默认会用中等强度动词, 应届生看起来过强, 资深人看起来过弱. 一开始就交代, 或者让它从你 master 简历里推断.
- 上来就纠结措辞, 不质疑结构. **第一轮 push back 应该是"条数和角度组合对不对"**, 不是"Built 还是 Developed". 结构错了, 换动词没用.
- 让 skill 凭空编数字而不让它说清楚 defensibility. 新版 skill 默认会主动给你 industry baseline 加计算公式; 你要做的是看一眼那个 baseline 和公式合不合理, 不要直接采纳没说明白的数字. case 里没数字时, 让 skill 帮你构造一个但要求它把 industry baseline + 计算公式 + 每个因子的合理性都列出来.
- skill Edit 完 resume.md **没有用 `git diff` 验证**就当事情结束了. AI Edit 文件有概率漏 / 错位 / 把别的 Bullet Set 改坏, 跑完总是 diff 一眼.
- 派生 role-specific 简历时**忘了删掉 Rationale blockquote**. 这些是给你自己看的内部解说, 投递时一定删干净.

---

## 5. bullet-reviewer 用法详解 (可选, 微调用)

`bullet-writer` 写出来的是"为 Job Family 通用"的 master 版本, 已经写进 `resume.md` 里. 同一份 master 投不同公司的同一个 Job Family 岗位, 大多数情况下不用改.

但你**真心想要的某个公司**, JD 里有很特殊的关键词或强调点, 这时候用 `bullet-reviewer` 做轻量微调. 这个 skill 有两种落地方式, 内部命名为 **Mode A** 和 **Mode B**:

- **Mode A (in-place 编辑)**: 直接在 master `resume.md` 里那个原 Bullet Set 上改措辞, 原版被覆盖. 适合改动小 (换 1 到 2 个关键词, 调整某条 bullet 的强调点) 的情况. 默认就选 Mode A.
- **Mode B (新增 JD-targeted 变体)**: master 里那个原 Bullet Set 保留不动, 在 §4 Experience 末尾**新增**一个 Bullet Set, 标题里显式编码目标公司加岗位 (例如 `### Bullet Set 8, Cedar Ridge MaternaPulse BI Agent (AI emphasis, for Nimbus Health LLM Engineer JD)`). 适合改动大 (超过 30% 措辞变化) 你又想保留原版以便日后投别家时用的情况.

skill 内部用 30% 的 diff 阈值做 Mode 推荐: 改动小于 30% 默认 Mode A, 超过 30% 默认 Mode B, 最终都让你拍板. 12 §8.1 讲一段经历挂多个 qualify-for/ 子目录共存时, 提到的"用 `bullet-reviewer` Mode A 微调"就是这里的 Mode A.

**输入输出本质** (跟 §4.1 同款分析):

- **输入**: 你 master `resume.md` 里**已经写好的某个 Bullet Set** (例如 Bullet Set 7) + 一份新的目标 JD
- **输出**: 针对这份 JD 的 before / after 微调建议, 你看完拍板之后**让 skill 直接 Edit `resume.md` 里那个 Bullet Set 的措辞**, 或者你自己手动改

**什么时候用**: master bullet 已经在 master `resume.md` 里, 现在要投一个**强匹配但措辞稍有差异**的 JD. 例如 master 写的是"LLM agent", JD 用的是"conversational AI assistant", 关键词换一下就更对得上 ATS 和 hiring manager 的语感.

**怎么调用**:

```
请调用 /bullet-reviewer skill.
我的 master 简历: students/john-doe/resume.md
我想针对一份新 JD 微调里面的 Bullet Set 7 (Cedar Ridge MaternaPulse AI emphasis).
新 JD 路径: <另一家公司 JD 路径>
请扮演这家公司的 hiring manager, 给我 before / after 微调建议.
确认之后直接 Edit resume.md 里那个 Bullet Set 的对应行.
```

**过程中会发生什么**: skill 扮演这家公司的 hiring manager:

1. 分析这个 JD: 必备 / 加分 / 字里行间的隐含期待
2. 6 秒扫描第一印象 (interested / 需要调整 / 不太对口)
3. 逐条 before / after 微调建议 (表格形式), 标明哪几个词换哪几个词, 哪个数字应该往前提
4. 技能迁移分析 (你的 AWS 对应他们的 GCP 等价物, 可不可以在 bullet 里显式提)
5. 你确认后, skill 直接 Edit `resume.md` 里那个 Bullet Set 的对应行, 同时让你 `git diff` 验证

**翻车点**:

- 用 reviewer 大改 bullet 本身. reviewer 是微调, 不是重写. 如果 JD 跟 case 错位太大, 应该回 07 或 08 重新跑一遍工作流, 不是在 bullet 这里硬扭.
- 每投一家都跑一遍 reviewer. 没必要. 同一个 Job Family 里, 大多数 JD 互相只差几个关键词, master 版本就够用了. 只对你**真心想要的 3 到 5 家**跑 reviewer.
- 反复 reviewer 同一个 Bullet Set 改了又改, master 越改越乱. 建议: 一旦针对某个 JD 微调出新版本, 如果改动比较大, 在 master 里**保留原版同时新增一个针对那家公司的 Bullet Set 变体**, 不要原地覆盖.

> 注: `bullet-writer` 和 `bullet-reviewer` 跟 07/08 用的那 6 个 skill 之间是**弱耦合**关系. 输入是磁盘上的文件 (你的 master `resume.md` + `case-cn.md` + 可选 `job-description.md`), 不依赖前面 skill 的运行时状态. 你完全可以**不跑 07 和 08**, 自己手写一份 case 加贴一份 JD, 直接调 `bullet-writer`. 反过来, 跑完 07 或 08 拿到 case 之后, 你也可以**不用 bullet-writer**, 照着 §3 那套 4-bullet 内部结构手写直接编辑 `resume.md`. skill 只是把流程工业化加上质量底线托底, 不是必经之路. 这套课的真正资产是 §3 那套**"先 HR 画面再技术递进"的内部结构思维**, skill 只是工程化容器.

---

## 6. 看一眼 John 实际跑出来的 bullets

打开 John 的 master 简历 [`resume.md`](../../students/john-doe/resume.md), 翻到 §4 Experience. 你会看到 **6 个 Bullet Set 并列**, 这就是本课程的核心设计: master 简历里一段经历可以有多个 Bullet Set, 每个对应一个 Job Family 角度, 投递时按目标 JD 删减剩下相关的几个.

具体看 Cedar Ridge 这一段经历, 它在 master 里同时有 2 个 Bullet Set:

- **[Bullet Set 2, Cedar Ridge MaternaPulse BI Agent (AI emphasis)](../../students/john-doe/resume.md)**: 投 AI Engineer / AI Solutions Engineer 时用. B1 给出"LLM-powered BI Agent on AWS Bedrock AgentCore + Strand Agents"这个 HR-readable 画面, B2-B4 沿 Knowledge Base 检索 / 多 provider 抽象 / 评估 harness 三条线展开.
- **[Bullet Set 3, Cedar Ridge MaternaPulse BI Agent (Data Analytics emphasis)](../../students/john-doe/resume.md)**: 投 Data Analyst / Analytics Engineer 时用. **同一段 Cedar Ridge 实习**, 同一个 MaternaPulse 项目, 但 B1 改成"Codified maternity ward's operational metric definitions into a YAML semantic layer", B2-B4 沿 UAT 研究 / adoption 仪表盘 / HIPAA audit 三条 Data Analytics 角度展开.

把 Set 2 和 Set 3 对照打开看, 你立刻就明白"同一段经历针对不同 Job Family 写出完全不同的 bullet"是什么意思. 这两套 Bullet Set 共享同一个 [`case-cn.md`](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case-cn.md) 当素材源, 但抽出来的角度和措辞完全是两个方向.

同时你可以看到 **Bullet Set 1 ("BEFORE elevation, teaching artifact")** 留在 master 里只是用来做教学对比: 同一段 Cedar Ridge 实习, 没拔高之前 bullet 长什么样 ("Wrote 15 SQL queries"), 拔高之后是什么样 (Set 2 / Set 3 那种). Set 1 不会出现在任何派生版本里, 投递时一定删掉.

如果你想看 case 文档怎么压缩成 bullet, 把 [`case-cn.md`](../../students/john-doe/experiences/from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/case-cn.md) 跟 `resume.md` §4 里 Bullet Set 2 / 3 同时打开, 体会一下"**case 里万字的素材**"是怎么压缩成"**bullets 里 4 条共 200 字**"的, 中间被砍掉的是哪些 (架构图细节, 备选方案对比, 反思与遗留这些上不了简历的部分留在 case 里, 给面试官追问时拿来当弹药).

---

## 7. 这一步在整套工作流里的位置

到这里你已经走完了 06 到 09:

- 06 教项目素材三条路 (导师设计 / 拔高已有 / 从 0 设计)
- 07 走完方法二的 6 阶段链路, 拿到 case 拔高版
- 08 走完方法三同样的 6 阶段链路, 拿到 forward-looking case
- 09 (本节) 把 case 压成 3 到 4 条 bullet, **直接 Edit 进 master `resume.md` §4 Experience**, 同一段经历可以有多个角度的 Bullet Set 并列

下一节 [10-write-summary](../10-write-summary/README-cn.md) 教你**拿着 master `resume.md` 里所有已经写好的 Bullet Set 反推 N 份 summary**, 每份对应一个 Job Family, 同样直接写进 master `resume.md` §1 Summary. 再往后 11 教投递: 投递的时候从 master 里**按目标 JD 删减**出派生版本 (`resume-role-1.md` 之类), 删掉不相关的 Summary, Skills 行, Bullet Set, 剩下的就是这次投递用的简历.

---

## 8. 导师寄语

很多人写 bullet 的时候卡在"我应该用 Built 还是 Developed", 在动词上反复 polish. 然后 4 条 bullet 措辞都漂亮, 但放一起读是一团乱麻, hiring manager 扫完不知道你做了个什么项目.

问题不是动词. 问题是**结构**: 4 条 bullet 是不是按递进逻辑组织, B1 有没有给 HR-readable 画面, B2-B4 是不是沿不同维度往下挖.

这一节真正想教你的不是"怎么用 AI 写 bullet", 是**4 条 bullet 内部的递进结构**这套思维. 理解了之后, 哪怕你完全不用 `bullet-writer` 这个 skill, 你自己手写也会比之前写得好得多. skill 是把这套思维工程化的工具, 思维本身才是这一节的核心资产.

下一节继续.
