# 从已经写好的 bullets 反推 Summary: 每个方向一份, 都进 master 简历

> 这是 examples 系列里的第十篇。前置: [09-write-bullets](../09-write-bullets/README-cn.md) 已经跑完, 你 master `resume.md` 里至少有几个 Bullet Set。这一节教你**拿着这些 Bullet Set 反推 N 份 Summary**, 每份对应一个 Job Family 方向 (例如 AI Engineer 一份、Data Analyst 一份、Software Engineer 一份), 全部写进 master 同一份 `resume.md` 的 §1 Summary 段。

## 1. 课程导读

跑完 09 之后, 你 master `resume.md` 的 §4 Experience 里已经有 5 到 8 个 Bullet Set, 同一段经历可能有 AI 角度版本加 Data Analytics 角度版本两份。素材到此为止其实**已经够投了**, 缺的就是顶上那行让 hiring manager 6 秒决定要不要继续往下读的 **Summary**。

09 我们专门解释过 [为什么先教 bullet 后教 summary](../09-write-bullets/README-cn.md): bullet 是证据, summary 是给证据贴的定位标签, 没证据就贴标签会卡住或失真。现在 10 终于来贴标签了, 而且因为证据已经在桌上, 这一节会非常轻松。

这一节的核心命题:

- Summary 是 **6 秒扫描的定位标签**, 不是一段「我的故事」, 不是一份简版自传, 也不是一行长长的形容词堆砌
- 一份 master 简历的 §1 Summary 里**同时挂 N 个 Summary variant**, 每个对应一个 Job Family 方向, 派生简历时按目标 JD 留 1 份删其它
- Summary 写作的核心方法是**从 master 里已经写好的 Bullet Set 反向归纳**: 看哪几个 Bullet Set 你打算放进这个方向的派生版本, 把里面反复出现的关键词、技术栈、问题领域抽出来, 一句话定位

---

## 2. Summary 是定位标签, 不是缩水版自传

Hiring manager 拿到一份简历, 第一眼扫 Summary。他在找一个问题的答案: **「这个人想往哪个方向定位自己?」**

注意是「想往哪个方向定位」, 不是「这个人是谁」。Summary 不是介绍, 是**声明**。

声明里要回答 3 件事:

- **职业身份 (Identity)**: 你想被看成什么样的工程师 (Backend Engineer / AI Engineer / Data Analyst / ...)
- **核心能力 (Strength)**: 这个身份下你最强的 1 到 2 个能力是什么 (例如「能从 0 搭 LLM agent」「能设计 semantic layer」)
- **支持证据 (Evidence)**: 1 个最有冲击力的项目证据或 metric 让前两句不显得空

3 件事压在 2 行 200 到 300 个字符内, 这就是好 Summary。

❌ 错误写法 (空泛的「关于我」型 Summary):

> "Passionate and results-driven M.S. Computer Science student with a strong background in software engineering and a love for solving complex problems. Excited to apply my skills to make impact at innovative companies."

读者扫完: 我不知道你做什么的, 不知道你强在哪, 不知道你能解决什么问题。形容词全是空话, 没有任何辨识度。

✅ 正确写法 (定位 + 能力 + 证据型 Summary, 例如 John 的 AI Engineer variant):

> "M.S. Computer Science student building production AI systems. Hands-on experience designing two natural-language BI Agents on AWS Bedrock AgentCore, covering maternity-ward operations at a 6-hospital healthcare network and fraud-ops self-serve analytics at a B2B fraud-detection SaaS. Comfortable with Strand Agents, Bedrock Knowledge Base, semantic layers over Snowflake, multi-provider LLM abstraction, prompt evaluation harnesses, and end-to-end AWS CDK deployment."

读者扫完: 「这人在做 production AI, 具体做过两个 BI Agent 在医疗加 fraud 两个领域, 用过这些具体技术」, 6 秒画面成型, 决定继续往下读 bullet 验证。

---

## 3. 同一份 master 简历, N 份 Summary 并列摆着

这是本课程的关键工程设计。一份 master `resume.md` 不只挂一份 Summary, 而是**同时挂多份 Summary variant**, 每份对应一个 Job Family 方向。

John 的 master `resume.md` 在 [§1 Summary](../../students/john-doe/resume.md) 下面同时挂着:

- **Variant A, for AI Engineer roles**
- **Variant B, for Data Analyst roles**
- **Variant C, for Software Engineer roles**

3 个 variant 并列。投递时按目标 JD 选 1 个留下, 删另外 2 个, 同步删掉 §4 Experience 里跟这个方向无关的 Bullet Set, 派生出 `resume-role-1.md`。投另一家 Software Engineer 岗位时基于 Variant C 派生 `resume-role-3.md`。

**为什么 3 个 variant 全挂在同一份 master 上, 而不是单独写 3 份 master?**

- 一段经历的 bullet 母版只有一套, 散在 3 份 master 之间会割裂、漂移、互相不同步
- 3 个 variant 并列摆着, 你一眼能看出**「这个人在 3 个方向上各自讲的是不是同一个事实, 只是侧重不同, 不是 3 个完全不同的人」**。这是真实定位, 不是欺骗。
- 维护成本最低: 改完一个 variant, 别的 variant 你抬眼就能看到要不要联动改
- 跟 [05-resume-matrix](../05-resume-matrix/README-cn.md) 教的「1 份 master + N 份派生」一脉相承: master 是源头, 派生由删减生成

---

## 4. summary-writer 保姆级使用指南

### 4.1 退一步看: 写 Summary 本质上也是把信息给 AI

跟 09 §4.1 同款分析。

**改简历 Summary 这件事的本质是: 你把信息给 AI, AI 给你一行定位标签**。具体是:

- **输入** (信息): 你的 master `resume.md` (里面已经写好的 Bullet Set 加 Education 加 Skills 是这次定位的全部证据) + **目标 Job Family** (例如「AI Engineer」「Data Analyst」「Software Engineer」) + **可选**目标 JD (有 JD 时 Summary 会更针对; 没有 JD 时是 Job Family 通用版)
- **输出** (1 段 2 行的 Summary) 同样 3 个落地选项:
  1. 直接吐到聊天框, 你手动复制
  2. 写到一个单独文件
  3. **直接合并写入你的 master `resume.md` §1 Summary 段, 作为一个新的 Variant** (本课程选这个)

理由跟 09 §4.1 完全一样: N 份 variant 并列摆着好对比、好维护, 跟「1 + N」设计一脉相承。

所以 `summary-writer` 这个 skill 的运行机制是: **跟你互动决定一个 Job Family 方向 + 拍板 1 段 2 行的 Summary, 然后直接 Edit 你的 master `resume.md`, 在 §1 Summary 下新增一个 `### Variant X, for <Job Family> roles` 段**。

### 4.2 实操: 怎么用

**什么时候用**: master `resume.md` 里 §4 Experience 已经有几个 Bullet Set, 你准备针对某个 Job Family 方向写一份 Summary 加进去。

**用之前准备什么**:

- master 简历路径 (例如 [`students/john-doe/resume.md`](../../students/john-doe/resume.md))
- 你想写的 Job Family 方向 (例如「AI Engineer」「Data Analyst」「Backend Engineer」「Data Engineer」)
- (可选) 一份你最想要的目标 JD, 让 Summary 更针对它
- 拍板要把 master 里**哪几个 Bullet Set 算到这个方向的派生简历里** (不是所有 Bullet Set 都属于一个方向)。这一步 skill 会主动帮你提议, 你确认就行

**怎么调用**:

```
请调用 /summary-writer skill。
我的 master 简历: students/john-doe/resume.md

请帮我写一份针对 "AI Engineer" 方向的 Summary, 加进 §1 Summary 段。
方向归属: master 里的 Bullet Set 2 (Cedar Ridge AI emphasis), Bullet Set 4 (NovaRisk AI emphasis) 这两个属于 AI Engineer 这个方向。
(可选) 目标 JD: students/john-doe/experiences/.../qualify-for-Cascadia-.../job-description.md
```

**过程中会发生什么**:

1. skill 读你 master `resume.md`, 找到 §1 Summary 段确认现有 variant 数, 找到 §4 Experience 里你点名的几个 Bullet Set 拿到证据。
2. skill 从这几个 Bullet Set 里抽出**反复出现的关键词**: 技术栈 (AWS Bedrock / Strand Agents / Snowflake / ...)、问题领域 (production AI agents / clinical analytics / fraud-ops / ...)、强度信号 (规模、精度、上线、用户数)。
3. skill 起草初版 Summary (2 行, 200 到 300 字符), 显式标出 3 个组成部分 (Identity / Strength / Evidence):
   > "Draft (Variant D, for AI Engineer roles):
   > - Identity: M.S. Computer Science student building production AI systems.
   > - Strength + Evidence: Hands-on experience designing two natural-language BI Agents on AWS Bedrock AgentCore, covering maternity-ward operations at a 6-hospital healthcare network and fraud-ops self-serve analytics at a B2B fraud-detection SaaS.
   > - Tech list: Comfortable with Strand Agents, Bedrock Knowledge Base, semantic layers over Snowflake, multi-provider LLM abstraction, prompt evaluation harnesses, and end-to-end AWS CDK deployment.
   >
   > Looks 280 chars, fits 2 lines. Match the existing Variants in your master?"
4. 你 push back, skill 调整。常见 push back:
   - 「这个 Identity 太弱, 'M.S. CS student' 在湾区一抓一大把。换成 'AI Engineer building production LLM agents in healthcare and fintech' 那种更聚焦」
   - 「Tech 列表太长, 6 个 buzzword 看着像列简历 Skills 段, 砍到 3 个最关键的」
   - 「Evidence 那一段 maternal-ward 是医疗, B2B fraud-detection 是金融, 跨行业有跨度感, 留着; 但 multi-provider LLM abstraction 这个细节属于实现层, 进 Skills 段不进 Summary」
5. 收敛到 1 段 2 行的版本, skill **直接 Edit 你的 master `resume.md`**, 在 §1 Summary 段末尾追加一个 `### Variant X, for <Job Family> roles` 段。X = 现有 variant 数加 1, 同时确保字母编号连续 (Variant A / B / C / D)。
6. **紧接着 Variant 段下面用 markdown blockquote (`>`) 写一段「Rationale for this Variant」**, 把 Identity 选择 / 关键名词 / 动词 / tech-list 排序的理由讲清楚, 同时记录这个 Variant 锚定了 master 里哪几个 Bullet Set (派生时这几个要一起留)。这块 rationale 是给你以后回看用的, 派生 role-specific 简历时整段删掉。
7. skill 在聊天里只发一段简短的话提示「Variant X 写好了, 改了第 X-Y 行, 请 git diff 验证」, **详细解说不在聊天里复述, 都在文件里**。

**输出结果长什么样** (新增进 `resume.md` §1 Summary 末尾, Variant 加紧接着的 Rationale 整体写入文件):

```markdown
### Variant D, for AI Engineer roles

M.S. Computer Science student building production AI systems. Hands-on experience designing two natural-language BI Agents on AWS Bedrock AgentCore, covering maternity-ward operations at a 6-hospital healthcare network and fraud-ops self-serve analytics at a B2B fraud-detection SaaS. Comfortable with Strand Agents, Bedrock Knowledge Base, semantic layers over Snowflake, multi-provider LLM abstraction, prompt evaluation harnesses, and end-to-end AWS CDK deployment.

> **Rationale for this Variant** (internal commentary; strip from any submitted resume)
>
> **Identity choice**: chose "M.S. Computer Science student building production AI systems" over generic "Software Engineer". The Bullet Sets actually show 2 shipped production AI agents, so this Identity is defensible. Avoided "AI Engineer" (claims more years than your level supports) and avoided "Aspiring AI Engineer" (signals junior anxiety).
>
> **Verbs**: "designing" in the Strength clause matches your actual case (you designed the architectures, didn't just implement). Avoided "led" (no team leadership in case).
>
> **Key nouns**: "production AI systems" packages shipped + on real users + on real infrastructure. "natural-language BI Agents" matches exactly the noun used in your Bullet Sets, keeping wording consistent. "semantic layers over Snowflake" signals you understand a non-trivial architectural pattern (invites a good follow-up question).
>
> **Tech-list ordering**: front-loaded "Strand Agents, Bedrock Knowledge Base" because they appear in 3 of 4 AI-direction Bullet Sets and are increasingly searched-for by hiring managers. "AWS CDK" at the end as a deploy-side signal.
>
> **Anchored on Bullet Sets**: 2 (Cedar Ridge AI emphasis), 4 (NovaRisk AI emphasis). When deriving a Variant D resume, keep these two Bullet Sets and drop the others.
>
> **Notes**: if you later derive a role-specific resume from this master, delete this rationale block.
```

**翻车点**:

- 上来就写 Summary, 不先确认「这个方向对应 master 里的哪几个 Bullet Set」。结果 Summary 写完跟 §4 Experience 里实际能留下的 Bullet Set 对不上, 一个说自己是 AI Engineer 一个全是 Software Engineer 项目, 派生时撕裂。**先选证据再写定位标签**。
- 用形容词堆砌 (passionate / results-driven / innovative / dynamic / fast-learning) 凑字数。这些词在 ATS 和 hiring manager 眼里是噪音, 全删, 用具体名词 (technologies / projects / metrics) 替代。
- 第一人称代词 (I / my)。Summary 默认是无主语第三人称语气, 跟 bullet 一样。
- Skill 写完不 git diff 直接结束。Edit 文件 AI 有概率漏 / 错位, 跑完总是看一眼 diff。
- 派生 role-specific 简历时**忘了删 Rationale blockquote**。这块是给你自己内部看的, 投递时一定删干净。

---

## 5. summary-reviewer 保姆级使用指南 (可选, 微调用)

跟 09 的 `bullet-reviewer` 同款定位: 当你**真心想要的某家公司**, JD 用的关键词跟你 master 里这个方向的 Variant 写得不完全一致, 用 `summary-reviewer` 做轻量微调。

### 输入输出本质

- **输入**: master `resume.md` 里**已经写好的某个 Summary Variant** (例如 Variant A) + 一份新的目标 JD
- **输出**: 针对这份 JD 的 before / after 微调建议, 你看完拍板之后**让 skill 直接 Edit `resume.md` 里那个 Variant 的措辞**, 或者新增一个 JD-targeted variant

**什么时候用**: master Summary 已经在 master 里, 现在要投一个**强匹配但 JD 用词略有不同**的具体岗位。

**怎么调用**:

```
请调用 /summary-reviewer skill。
master 简历: students/john-doe/resume.md
我想针对一份新 JD 微调 Variant A (AI Engineer)。
新 JD 路径: <某家公司 JD 路径>
请扮演这家公司的 hiring manager, 给我 before / after 微调建议。
确认之后直接 Edit master 里那个 Variant 的对应行。
```

**过程中会发生什么**: skill 扮演这家公司的 hiring manager:

1. 分析这个 JD 的核心 buzzword 和强调点
2. 6 秒扫描你 Variant A 在他眼里的第一印象 (interested / 需要调整 / 不对口)
3. before / after 微调建议: 哪几个词换、哪个 phrase 提到 Summary 最前
4. 让你拍板「**in-place 改 Variant A**」还是「**新增 Variant A-Cascadia 这种 JD-targeted variant**」(默认 in-place, 除非改动超过 30%)
5. Edit master, git diff 验证

**翻车点**:

- 反复用 reviewer 改同一个 Variant, master 越改越乱。建议: in-place 改适合小调整 (换 1 到 2 个关键词); 大调整应该新增 JD-targeted variant 留底 master 原版
- 用 reviewer 改方向。reviewer 是微调措辞, 不是换 Job Family。如果发现你 Variant A 跟目标 JD 完全不对口, 应该让 `summary-writer` 写一个新方向的 Variant, 不是在这里硬扭

> 注: `summary-writer` 和 `summary-reviewer` 跟前面 7 个 skill (5 个 qualify-* / mini-project-* + 2 个 bullet-*) 之间是**弱耦合**关系。输入是磁盘上的文件 (master `resume.md` + 可选 `job-description.md`), 不依赖前面 skill 的运行时状态。完全可以**不跑 09**, 自己手写几条 bullet 进 master, 直接调 `summary-writer` 写 Summary。反过来, 跑完 09 拿到 master Bullet Set 之后, 你也可以**不用 summary-writer**, 照着 §2 那 3 件事 (Identity / Strength / Evidence) 手写。skill 只是把流程工业化加质量底线托底。

---

## 6. 看一眼 John 实际的 3 份 Summary variants

打开 John 的 master 简历 [`resume.md`](../../students/john-doe/resume.md), 翻到 §1 Summary。你会看到 3 个 Variant 并列:

- **Variant A, for AI Engineer roles**: Identity 是 "building production AI systems", evidence 是 2 个 BI Agent 项目, tech 列表聚焦 LLM agent / Bedrock / Snowflake / CDK
- **Variant B, for Data Analyst roles**: Identity 是 "focused on Data Analytics for AI and risk products", evidence 是 semantic layer / UAT / 仪表盘, tech 列表聚焦 SQL / Python / pandas / Looker / Tableau
- **Variant C, for Software Engineer roles**: Identity 是 "building distributed backend services", evidence 是 Go feed ranker 加 AI 系统, tech 列表聚焦 Go / Python / gRPC / Kubernetes / AWS CDK

3 个 Variant 描述**同一个人**, 但从 3 个完全不同的角度。

把 Variant A 和 Variant B 对照看, 你能看到一个明显的规律: 同一段 Cedar Ridge MaternaPulse 经历, 在 Variant A 里被描述成「LLM agent on AWS Bedrock」, 在 Variant B 里被描述成「semantic layer over Snowflake」。同一个项目, 同一个产物, 两个角度都是真的, 只是侧重不同。这跟 09 §6 看的 Bullet Set 2 (AI emphasis) 加 Bullet Set 3 (Data Analytics emphasis) 是一个套路: **同一段经历, N 个角度, 都进 master, 派生时按 JD 留 1 个**。

也注意 3 个 Variant 的 **tech 列表完全不同**。这是有意的: 派生简历时, §3 Skills 段也按 Variant 删减 (Variant A 的派生只留 AI/ML 加 Cloud 加 Data Infra 几行, Variant B 的派生只留 Data Analytics 加 Data Infra 几行)。Summary、Bullet Set、Skills 三块在派生时一致删减, 拼出来才是这个方向的一份内部自洽的简历。

---

## 7. 这一步在整套工作流里的位置

到这里你已经走完了 06 到 10:

- 06 教项目素材三条路
- 07 / 08 跑完 6 阶段链路, 拿到 case
- 09 把 case 压成 Bullet Set, 直接 Edit 进 master `resume.md` §4 Experience
- 10 (本节) 从 master 已经写好的 Bullet Set 反推 N 份 Summary variant, 直接 Edit 进 master `resume.md` §1 Summary

到这里 master `resume.md` 已经**写完了**: §1 多份 Summary variant、§2 Education、§3 Skills 全表、§4 多个 Bullet Set。下一节 [11-submit-and-collaborate](../11-submit-and-collaborate/README-cn.md) 教你怎么**从这份 master 派生出针对具体 JD 的派生简历** (`resume-role-N.md`), 怎么把 master 加派生简历放到 GitHub 加 Google Doc 上跟导师双轨协作 review。

---

## 8. 导师寄语

很多人写 Summary 的时候卡在「我应该说自己 passionate 还是 results-driven」这种形容词选择, 然后这两个词都说了, 还加了 innovative / proactive / hands-on, 一段 Summary 5 个形容词都用上, 写完自己都不好意思读。

形容词不是 Summary 的内容。**具体的身份、具体的能力、具体的证据**才是。

Summary 的难点不在写, 而在**先把方向选准**。你只有看着 master 里已经写好的 Bullet Set 才能选准方向, 反过来「先想一个酷的 Summary 再去硬凑 Bullet Set」永远会失败。这就是为什么 09 在 10 前面。

下一节继续。
