# 教程 07：专业英文沟通（技术工作的一页纸英文写作）

> 严重度：🟠 Nice-to-have（wrapper）｜ 对应 POC-07 ｜ 前置：你已完成至少一个硬 POC（比如 POC-01 SQL 对账）

## 这篇教程要解决什么

Virtu 这个 Trading Ops Analyst 岗位的 JD 里有一句软性要求，很多人扫一眼就跳过了：

> "communicate information precisely and with agility to parties both internal and external"

翻译过来：**把信息精确、敏捷地传达给公司内部和外部的人**。在交易运营岗，这不是加分项，这是日常。对账出了 break，你要在五分钟内用一段英文说清楚"哪笔、差多少、我怀疑是什么原因、下一步谁去查"。托管行、券商、内部 desk 都在等你这段话。写得啰嗦、写得含糊、写出中式英语长句，对面就要来回问三轮，事情就拖了。

你已经有很强的底子：带过 20 多个实习生、组织周会、向上汇报。这些都是硬核的沟通经验。**唯一的缺口是语言**：这些经验全是在中文语境里练出来的。这篇教程不教你"怎么沟通"（你会），教你"怎么把你已经会的沟通，用地道简洁的英文书面表达出来"。

方法很简单，也很省事：**每做完一个 POC，就给它贴尾写一份英文一页纸（one-pager）**。一箭三雕：

1. 练英文书面表达；
2. 逼自己把技术工作讲给"不懂细节的人"听（这正是 Ops Analyst 的核心技能）；
3. 攒下一叠 writing sample，面试时可以直接甩出来。

学完这篇，你要能做到 POC-07 的三条成功标准：

- 每份一页纸能被一个不懂技术细节的人 **90 秒读懂**；
- 用词精确、无冗余、**无中式英语长句**；
- 每份都有一个**可执行的 next step**。

深度打磨会放到后面的 mock interview。这篇是起步，先把架子搭对、把最常见的毛病改掉。

---

## 第一部分：一页纸的四段式结构

别自己发明结构。一个久经考验的模板，四段，按这个顺序写：

```
Problem   →   Approach   →   Output / Result   →   Next Step
（问题/背景）  （做法）        （产出/结果）          （下一步）
```

这个顺序不是随便排的。它符合读者的脑子：先告诉我**你在解决什么问题**（不然我不知道为什么要读），再告诉我**你怎么做的**，然后**结果是什么**，最后**接下来干嘛**。读者读完每一段，脑子里的问题恰好被下一段回答。

下面逐段拆开讲：每段写什么、写多长、用什么句式。

### 1. Problem（问题 / 背景）：2 到 3 句

**写什么**：这个 POC 在解决什么问题？为什么这个问题重要？给一句最小背景，让完全不懂的人也能进入状态。

**写多长**：2 到 3 句，绝不超过 4 句。这是最容易写长的一段，克制。

**句式**：用一般现在时陈述现状，用一句话点出痛点。

范例句：

> Trading operations teams reconcile internal position records against custodian statements every day. When the two sides disagree, the difference is called a break, and each break must be found and explained before the book can close.

注意：这里没有一个技术术语是读者查不到的。`break` 这个词一出现就顺手解释了（"the difference is called a break"）。这就是"精确"的第一层：**术语要么不用，要用就当场定义**。

### 2. Approach（做法）：3 到 5 句

**写什么**：你具体做了什么？用了什么工具？做了哪些关键设计决策？

**写多长**：3 到 5 句。这是正文主体，但也别堆细节，只讲**决策**，不讲每一行代码。

**句式**：用一般过去时（你已经做完了）。每句一个动作，动词开头的信息密度最高。

范例句：

> I built three reconciliation queries in PostgreSQL. For position reconciliation, I used a FULL OUTER JOIN so that records existing on only one side are not silently dropped, because a one-sided record is itself a break. For cash, I compared balances with a tolerance threshold. For trades, I filtered on settlement date to handle the T+1 cycle.

注意 "so that ... because ..." 这个结构：**它把"我做了什么"和"我为什么这么做"绑在一句里**。面试官最想看的就是这个"为什么"。啰嗦的写法会把它拆成两句、还加一堆废话；地道的写法一句搞定。

### 3. Output / Result（产出 / 结果）：2 到 4 句，尽量量化

**写什么**：产出了什么可以拿出手的东西？如果有数字，一定放数字。

**写多长**：2 到 4 句。

**句式**：一般过去时或现在完成时。**能量化就量化**，数字比形容词有说服力一百倍。

范例句：

> The queries flagged all 47 injected breaks in the synthetic dataset with no false positives. Each break is written to a persistent ledger with an aging counter, so a break open for three days is visible at a glance.

对比一下不量化的写法："The queries worked well and found the breaks."（读者心里：多少个？准不准？有没有误报？），一个 "47" 和一个 "no false positives"，可信度天差地别。

### 4. Next Step（下一步）：1 到 2 句

**写什么**：接下来要做什么？这一步必须**具体、可执行**，不能是"继续优化"这种空话。

**写多长**：1 到 2 句。

**句式**：用 "Next, I will ..." 或 "The next step is to ..."。

范例句：

> Next, I will extend the aging ledger to match breaks that clear across multiple days, which produces the core of the case study's stage-three reconciliation engine.

> 💡 **母语中文者最常见的英文毛病**：next step 写成 "In the future, we can do more optimization and improvement to make the system better and more perfect." 这句话四个毛病：将来时态含糊（in the future 到底啥时候）、we 是谁（应该是 I）、"optimization and improvement" 和 "better and more perfect" 是同义反复的凑字。地道写法只需要一个**具体动词 + 一个具体对象**。

---

## 第二部分：中式英语 vs 地道简洁英语（核心）

这一部分是全教程的重头。母语中文的人写英文，毛病是有规律的、可预测的。下面 12 组对照，全部来自技术写作的真实场景。每组给"改前 → 改后 → 为什么"。你不用背，读懂逻辑就行。

### 对照 1：长句套长句 → 拆成短句

> ❌ **改前**：Because the internal records and the custodian records may have differences which are caused by many reasons such as timing or manual errors, so it is necessary for us to build a reconciliation system that can find these differences automatically and report them to the team.
>
> ✅ **改后**：Internal and custodian records often disagree, usually because of timing gaps or manual errors. A reconciliation system finds these differences automatically and reports them.

**为什么**：改前是一个 40 词的巨型句，"Because ... so ..." 还是中文语法（英文里 because 和 so 不能同时用）。改后拆成两句，每句一个意思。**英文的节奏是短句**，一句话超过 25 个词就该考虑拆。

### 对照 2：Because ... so ... 双连接词

> ❌ **改前**：Because the data is synthetic, so I can inject known breaks.
>
> ✅ **改后**：Because the data is synthetic, I can inject known breaks.（或者：The data is synthetic, so I can inject known breaks.）

**为什么**：中文说"因为...所以..."两个词都要。英文只能留一个。`because` 引导原因从句，`so` 引导结果，二选一。这是最高频的语法错，一眼就暴露是非母语。

### 对照 3：被动语态堆砌 → 主动语态

> ❌ **改前**：Three queries were built by me, and breaks were detected by the FULL OUTER JOIN, and a ledger was created to store the results.
>
> ✅ **改后**：I built three queries. The FULL OUTER JOIN detects breaks, and a ledger stores the results.

**为什么**：被动语态"was done by"读起来软、绕、没力气。技术写作里，**你做的事就大方用 "I"**，工具做的事就让工具当主语（"the JOIN detects"）。主动语态短、直接、有担当感。Amazon 的写作文化明确反对无谓的被动。

### 对照 4：名词化过度（把动词变成名词）

> ❌ **改前**：The implementation of the reconciliation of the two tables was done through the utilization of a FULL OUTER JOIN.
>
> ✅ **改后**：I reconciled the two tables with a FULL OUTER JOIN.

**为什么**：改前把 reconcile（动词）变成 reconciliation（名词），把 use 变成 utilization，句子被 "of...of...of" 串成一条虫。这叫"名词化"（nominalization），是学术腔和中式英语共有的病。**能用动词就别用它的名词形式**：`reconcile` 而不是 `perform the reconciliation of`。

### 对照 5：冠词 the / a 乱用

> ❌ **改前**：I wrote query to find break in position data.
>
> ✅ **改后**：I wrote a query to find breaks in the position data.

**为什么**：中文没有冠词，所以这是永远的痛点。三条最实用的规则：
- 第一次提到、单数可数名词，用 **a/an**：`a query`, `a break`。
- 特指双方都知道的东西，用 **the**：`the position data`（就是我们在说的这份数据）。
- 泛指一类东西、用复数就**不加冠词**：`breaks are differences`（break 这类东西泛指）。

冠词错不影响别人看懂，但它是"非母语标签"。慢慢练，读得多了会有语感。

### 对照 6：时态混乱

> ❌ **改前**：I use PostgreSQL and I build three queries and they find 47 breaks.
>
> ✅ **改后**：I used PostgreSQL and built three queries. They found 47 breaks.

**为什么**：描述"我已经做完的 POC 工作"用**一般过去时**（used, built, found）。描述"这个系统平时是怎么运作的、一个普遍事实"用**一般现在时**（The JOIN detects breaks）。一页纸里 Approach/Result 段基本是过去时，Problem 段的背景陈述是现在时。别在一句里混着跳。

### 对照 7：模糊词 → 精确词 / 数字

> ❌ **改前**：The queries found a lot of breaks very fast and the result is quite good.
>
> ✅ **改后**：The queries found all 47 breaks in under two seconds with no false positives.

**为什么**：`a lot of` / `very fast` / `quite good` 全是模糊词，读者无法验证。JD 要的是 "precisely"。**把每个模糊词换成一个可验证的事实**：多少个（47）、多快（under two seconds）、准不准（no false positives）。这是"精确沟通"最直接的体现。

### 对照 8：domain 词用错 ，reconcile / reconciliation

> ❌ **改前**：I compared the two tables to find the different data.
>
> ✅ **改后**：I reconciled the two tables to identify breaks.

**为什么**：`compare` 太泛，`the different data` 更是外行话。在交易运营里，"把两侧账目核对、找出不一致"这个动作有专门的词：**reconcile**（动词）/ **reconciliation**（名词）。用对 domain 词，对面立刻知道你是内行。注意搭配：`reconcile A against B`（拿 A 对 B 核对），`a break`（一处不一致），`to identify / investigate / clear a break`（发现 / 调查 / 消解一处不一致）。

### 对照 9：domain 词用错 ，settle / settlement / T+1

> ❌ **改前**：After the trade is done, we need to wait one day for the money and shares to really move, and I made the query consider this one-day waiting.
>
> ✅ **改后**：I filtered on settlement date to account for the T+1 cycle, where a trade settles one business day after it is executed.

**为什么**：改前是把 settlement 这个概念"用大白话绕着讲"，又长又露怯。行业里"成交后款券实际交割"就叫 **settle / settlement**，"成交后第 N 个工作日交割"就是 **T+1 / T+2**。`a trade is executed`（成交）→ `it settles`（交割），这条动词链要记熟。用对了，一句话干净利落。

### 对照 10：There be 开头的空话

> ❌ **改前**：There are three queries that were written by me to do the reconciliation work.
>
> ✅ **改后**：I wrote three reconciliation queries.

**为什么**：`There is / There are` 开头往往是在拖延，真正的主语和动作被推到后面。**把真正的主语提到句首**：谁做了什么。7 个词说清 15 个词的事。

### 对照 11：连接词误用（however / but / and 的位置和搭配）

> ❌ **改前**：I used INNER JOIN first, but however it dropped the one-sided records, so and I changed to FULL OUTER JOIN.
>
> ✅ **改后**：I first used an INNER JOIN, but it silently dropped the one-sided records. I switched to a FULL OUTER JOIN.

**为什么**：`but however` 是叠床架屋（两个都表转折，留一个），`so and` 不成词。规则：**一个转折关系只用一个连接词**。`however` 用在句首要跟逗号（However, ...），`but` 用在句中连接两个分句。拿不准时，把长句拆成两个短句，连接词的坑就绕过去了。

### 对照 12：礼貌客套 / 自我贬低（中式邮件腔）

> ❌ **改前**：I am not an expert, but I tried my best to do a small POC, and I hope it can be helpful. Please kindly correct me if there is anything wrong.
>
> ✅ **改后**：This POC demonstrates a working reconciliation approach. Feedback is welcome.

**为什么**：中文邮件习惯先谦虚一轮再说事。英文技术沟通里，过度的自我贬低（"I am not an expert"）会**削弱你自己的可信度**，读者会真的按你说的打折。自信、直接地陈述你做了什么。要请人反馈，一句 "Feedback is welcome." 就够了，不用 "please kindly"。

---

### ✅ 改写小练习（附参考）

自己动手改这三句，改完再看参考答案。别偷看。

**练习 A**（长句 + 被动 + 名词化）：
> The detection of the breaks was performed by the utilization of a query which was written by me in order to make the comparison of the two tables possible.

<details>
<summary>参考答案</summary>

> I wrote a query to reconcile the two tables and detect breaks.

（22 词砍到 11 词：动词化 detect，主动语态 I wrote，去掉 "in order to make ... possible" 的空转。）
</details>

**练习 B**（模糊词 + 冠词 + 时态）：
> Query find many break and it is very quick, the result look good.

<details>
<summary>参考答案</summary>

> The query found all 47 breaks in under two seconds, and the results look correct.

（补冠词 the query / the results，模糊的 many/very quick/good 换成可验证事实，动词补时态 found。）
</details>

**练习 C**（because...so + 客套）：
> Because I am a beginner, so my POC maybe have some problem, but I think because the data is fake so I can control the break, this is a little useful maybe.

<details>
<summary>参考答案</summary>

> Because the data is synthetic, I can inject known breaks and verify the queries against a ground-truth ledger. This makes the results reproducible.

（删掉全部自我贬低和 maybe，修掉 because...so，把真正有价值的信息 ，合成数据可控、可对真值验证 ，提出来讲。）
</details>

---

## 第三部分：完整范文（SQL 对账 POC 的英文一页纸）

下面是给 POC-01（SQL 对账）写的完整 one-pager，约 270 词。先整篇读一遍，再看逐段讲解。

---

> **One-Pager: SQL-Based Trade & Position Reconciliation (POC-01)**
>
> **Problem**
> Trading operations teams reconcile internal records against custodian and broker statements every business day. When the two sides disagree, the difference is called a break, and every break must be found and explained before the book can close. Doing this by eye does not scale, so the work needs to be expressed as repeatable queries.
>
> **Approach**
> I built three reconciliation queries in PostgreSQL against a synthetic dataset with known, injected breaks. For position reconciliation, I used a FULL OUTER JOIN so that a record existing on only one side is surfaced rather than silently dropped, because a one-sided record is itself a break. For cash, I compared balances against a tolerance threshold to avoid flagging rounding noise. For trades, I filtered on settlement date to respect the T+1 cycle, where a trade settles one business day after it is executed. Each break is written to a persistent ledger with an aging counter.
>
> **Output**
> The queries flagged all 47 injected breaks with no false positives, validated against a ground-truth ledger produced by the data generator. The aging counter makes a break that has stayed open for three days visible at a glance, which is what an operations analyst watches for.
>
> **Next Step**
> Next, I will extend the ledger to match breaks that clear across multiple days, so that a break appearing on one day and resolving the next is closed automatically. This produces the core of the case study's stage-three reconciliation engine.

---

### 逐段讲解：为什么这么写

**标题**：`One-Pager: SQL-Based Trade & Position Reconciliation (POC-01)`。名词短语，不写成句子。带上 POC 编号方便归档。用了 domain 词 reconciliation，一眼看出是内行活。

**Problem 段**：三句。第一句陈述行业现状（现在时）。第二句当场定义 break ，这是给"不懂细节的人"读的关键动作。第三句点出痛点（"by eye does not scale"），交代为什么值得做。全段没有一个读者查不到的术语。

**Approach 段**：这是最长的一段（约 100 词），但每一句都在讲一个**决策**，不讲代码。注意三个 "so that ... because ..." / "to ..." 结构，每个都把"我做了什么"和"为什么"绑在一起 ，这正是面试官想挖的东西。FULL OUTER JOIN 那句尤其重要：它展示了你踩过 INNER JOIN 会漏掉单侧记录的坑（POC-01 里明确点名的最常见错误），并说清了取舍。T+1 顺手定义，外部读者也能跟上。

**Output 段**：两句，全是可验证的事实。"all 47 ... no false positives ... validated against a ground-truth ledger" ，数字 + 验证方法。最后半句 "which is what an operations analyst watches for" 把技术产出翻译成岗位价值，这一步很关键：它让 Ops 面试官看到你懂这活儿的意义，不只是会写 SQL。

**Next Step 段**：具体、可执行、有时序（"one day and resolving the next"），并且点明它通向 case study 的下一阶段。不是"继续优化"的空话，是一个明确的、下周就能动手的动作。

**整体**：一般读者 90 秒能读完并读懂；技术读者能看到取舍深度；Ops 面试官能看到岗位价值。三层读者各取所需 ，这就是一页纸要达到的效果。

---

## 第四部分：朗读自检法（Amazon narrative 文化）

Amazon 内部不用 PPT，用六页纸的叙述性备忘录（narrative memo），开会前全员先默读。这套文化催生了一个极简但极有效的自检法：**写完大声读出来**。

原理：你的眼睛会自动"脑补"跳过卡壳的地方，但你的**嘴和耳朵不会**。**凡是读起来卡顿、绕口、需要回头重读的句子，就是要改的句子。** 中式英语长句的通病 ，从句套从句、一口气喘不上来 ，在朗读时会立刻暴露。

具体操作步骤：

1. **写完先放一放**，去喝杯水，隔几分钟回来（趁热读会自动脑补）。
2. **出声读**，正常语速，不要默读。可以小声，但嘴要动、耳朵要听见。
3. **标记卡壳点**：哪句需要换气两次、哪句读到一半得回头看主语是谁、哪个词拗口，就在旁边划一道。
4. **逐个改**：卡壳几乎总是因为句子太长或结构绕。**首选拆句** ，一个长句拆成两三个短句，八成的卡壳当场消失。
5. **再读一遍**，直到从头到尾一口气顺下来、不用回头。

补充一招：**用系统自带的朗读功能（macOS 的 VoiceOver / "朗读所选文本"）让机器读给你听**。机器不会脑补，它会一板一眼地念，念到别扭的地方你一耳朵就能听出来。对非母语者尤其管用。

> 💡 **母语中文者最常见的英文毛病**：写的时候脑子里是中文，逐字翻译成英文，就会产出"语法对但没人这么说"的句子。朗读法是最便宜的解药 ，你不需要懂全部语法规则，你只需要能听出"这话读起来别扭"。多数母语者也是靠语感而非规则写作的。

---

## 第五部分：专业英文常用句式小抄

写作时可以直接抄的高频地道句型。按用途分类。

**开头 / 陈述背景（Problem 段）**
- `Trading operations teams reconcile ... every business day.`（陈述常规）
- `When X disagrees with Y, ...`（引出问题）
- `The challenge is that ...`（点出难点）
- `Doing this manually does not scale, so ...`（论证为什么要做）

**描述做法（Approach 段）**
- `I built / wrote / designed X to do Y.`（主动、动词开头）
- `For X, I used Y so that Z.`（做法 + 目的）
- `I chose A over B because ...`（展示取舍 ，面试官最爱）
- `To handle the edge case where ..., I ...`（展示考虑周全）

**过渡 / 转折**
- `However, this approach had a limitation: ...`（转折，注意 However 后加逗号）
- `As a result, ...`（因果）
- `In contrast, ...`（对比）
- `This matters because ...`（把技术点接到价值上）

**下结论 / 报告结果（Output 段）**
- `The queries flagged all N breaks with no false positives.`（量化结论）
- `This confirms that ...`（下判断）
- `In short, ...` / `The key result is ...`（收束）

**给建议 / 下一步（Next Step 段）**
- `Next, I will ...`（下一步，最简洁）
- `The next step is to ...`
- `I recommend ...ing, because ...`（对外给建议时用）
- `This unblocks / enables ...`（说明这一步的价值）

**对外邮件里请求 / 收尾**
- `Could you confirm whether ...?`（客气但直接地提问）
- `Please let me know if you see a break on your side.`（请对方核对）
- `Feedback is welcome.`（收尾，不用 please kindly）

---

## 第六部分：精确沟通的几条原则（面试 / 邮件通用）

一页纸练的是书面表达，但底层原则在面试口头回答、日常邮件里同样适用。四条，记住就够用：

**1. BLUF ，Bottom Line Up Front（先结论后细节）**
先说结论，再说过程。别学中文习惯"铺垫半天最后才点题"。
- ❌ "So I looked at the data, and then I noticed some patterns, and after some analysis, I found that... there is a break in account 12."
- ✅ "There is a break in account 12: internal shows 500, custodian shows 480. I am investigating the 20-share gap."

邮件也一样：第一句就是结论，后面才是支撑。忙人只读第一句。

**2. 一句一个意思（one idea per sentence）**
一句话只装一个意思。想说两件事，就写两句。这是治中式长句的根本大法，也直接呼应第四部分的朗读法。

**3. 量化一切（quantify）**
能给数字就给数字。"很多 break" → "47 个 break"；"很快" → "under two seconds"；"改善明显" → "误报从 12 降到 0"。JD 明说要 precisely，数字就是精确的载体。

**4. 避免模糊词（kill hedge words）**
`maybe / probably / some / a lot of / kind of / I think` 这类词稀释可信度。你确定的事就直接说；真不确定的，明确说清不确定在哪（"I am not yet sure whether the gap is a timing difference or a genuine break; I am checking the settlement dates."），这种**精确的不确定**反而显专业，比含糊的 maybe 强得多。

---

## 收尾：你的执行节奏

这个 POC 不占独立周，它是"贴尾"任务：

1. 每做完一个硬 POC（01 SQL 对账、02 domain、03 Python、05 自动化），就照四段式给它写一份 250 到 300 词的英文一页纸。
2. 每份写完，走一遍**朗读自检**，把卡壳的句子拆短、改顺。
3. 对着第二部分的 12 组对照，检查自己有没有犯那些典型毛病。
4. 四份攒到一起，就是一份可以带进面试的 **writing sample**。

不用一次写到完美。**先写对结构、去掉最刺眼的中式英语**，深度打磨留给后面的 mock interview。你的沟通底子已经很硬，这里补的只是语言这一层皮 ，贴着 POC 慢慢练，它会自己长出来。
