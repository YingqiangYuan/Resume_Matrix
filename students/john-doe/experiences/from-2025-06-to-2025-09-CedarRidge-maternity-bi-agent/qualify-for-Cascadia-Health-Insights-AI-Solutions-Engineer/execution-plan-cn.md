# Gap Fill Plan, Cascadia Health Insights AI Solutions Engineer

本文档基于上一步的 gap 分析，把 9 个 gap 转化为 9 个 mini-POC（每个 POC 是一个聚焦的 skill-learning 小项目，不是假装的业务项目）和对应的 tutorial 文档索引。计划周期是 2026 年 3 月到 5 月共 12 周，对齐 3 月中旬的 Cascadia 面试节点。每个 POC 都对应一个独立的练习目录 `pocs/poc-NN-xxx/` 和教程目录 `tutorials/NN-xxx.md`。

需要先把"mini-POC"这个词在本文档里的含义钉死。

mini-POC 在本框架里特指 skill-learning 性质的练习项目，目标是把一个具体技能（一个框架、一种数据格式、一套合规要求）从"读过文档"推进到"亲手写过、跑过、踩过坑"的程度。

它不需要假托一个真实业务场景，也不需要嵌入 Cascadia 客户的具体业务语境。每个 mini-POC 的语料和数据可以是 Wikipedia、Synthea 合成数据这种公开素材，只要能逼出对应技能即可。

这一点对于 John 这种已经在 CedarRidge 做过真实 BI 项目的学生尤其重要，避免他在 spare time 又造一个假项目重复练习同样的业务技能。

---

## 1. 执行原则与时间线

整个 gap fill 计划压缩在 2026 年 3 月初到 5 月底的 12 周里，目的是让 John 在 3 月中旬的 Cascadia 技术电面和 4 月初的 onsite 之前，能就 9 个 gap 中的每一个都讲出"我做过、踩过坑、知道下次怎么改"这个层级的故事，而不是停留在"读过 doc"。

这里要诚实承认的是，9 个 gap 不可能在 12 周内全部做到 production-grade 水平。3 个 Core gap（POC-01 / POC-02 / POC-03）要做到能上手写代码、能讲清取舍，但 POC-04（CDK）和 POC-07（合规审计）这类 gap 只需要 ramp 到 "可信的面试级"，不必造一套真正的多账户落地方案。

时间安排上遵循三个原则。

第一，先做 entanglement 高的 POC，因为一个 POC 解锁多个 gap，单位时间收益最高。

第二，先做面试 onsite 系统设计题最可能问到的栈（Strand Agents、RAG eval、Bedrock AgentCore），把这条主线在 Week 1 到 Week 6 内完成。

第三，把 client-facing writing 这类软技能 POC 拆碎贴在每个硬技能 POC 的尾巴上，每完成一个 POC 都顺手写一份 1 到 2 页的 client-facing 风格 design doc，到 Week 12 时自然累积出 8 到 10 页样本。

John 当前的现状是上一份在 CedarRidge 做的 maternity BI agent 项目已经踩通了语义层、Snowflake、Charge Nurse 用户访谈这条链路，所以 POC-05（Semantic Layer YAML）和 POC-08（client-facing writing）有现成素材可以复用和重写，不必从零起步。这两块的时间预算可以压到 2 到 3 天。剩下的预算用在 LLM agent 栈（Strand、Bedrock、RAG）和 AWS infra（CDK）上，这是 John 离 production-grade 距离最远、面试官最可能深挖的部分。

每周固定 12 到 15 小时的投入预算（晚上 + 周末），3 月到 5 月共 144 到 180 小时。这个预算和 9 个 POC 的工作量大致匹配，但不留缓冲。如果 4 月份面试 onsite 的反馈让某个方向需要补救，要砍掉优先级最低的 POC-09（Python 代码质量）的精修部分，保留主干。

最后还要点明三件事，避免 John 在执行过程中被"完美主义"拖死。

第一，每个 POC 都要在 demo-ready 状态停手，不追求 99 分，70 到 80 分能讲出故事就转入下一个 POC。这是因为 Cascadia 的面试官关心的是"你怎么思考、怎么取舍"，而不是你的 RAG hit rate 比 baseline 高几个点。

第二，每个 POC 完成时要立刻录一段 3 到 5 分钟的口头讲解视频上传到自己的私人 YouTube 或本地存档，用来在后续 mock interview 之前快速复习，并且暴露"我以为我会讲，结果讲不清"的死角。

第三，每周末要花 30 分钟做一次进度回顾，回顾的载体是一个简单的 markdown 日志，记录"做完什么、卡在哪、下周改什么"，这个日志本身就是 Cascadia onsite 的 "comfort with ambiguity" 故事素材库。

时间线本身要和 Cascadia 招聘流程的真实节奏对齐。

Cascadia 的 JD 文末写明流程是 recruiter screen 30 分钟、take-home Python 和 SQL 3 小时（一周内完成）、技术电面 60 分钟、onsite 半天 4 面，整个过程典型 3 到 4 周。

倒推回去，3 月中旬递简历的话，4 月中旬左右进 onsite。因此本计划的硬节点是 Week 4 之前完成 Strand Agent 主干、Week 6 之前完成 RAG eval baseline、Week 8 之前 AgentCore endpoint 能演示。

这三个节点恰好是 Cascadia onsite 4 面里 system design 一面最可能追问的素材，错过任何一个都会导致 onsite 阶段缺锚点。

另外要承认的一个现实是 John 不能停掉现有 CedarRidge 的本职工作，9 个 POC 是 evening + weekend 兼职性质。

这意味着本计划的实际容错空间几乎为零，必须高度纪律性地保护周末 4 小时和工作日 2 小时这两个时段。

任何一周失守 2 次以上就要做 retrospective，找到是工作冲突还是动力问题，对症调整。如果是动力问题，建议优先做 POC-08（写 client-facing one-pager）这种产出快、反馈快的小任务，重建节奏。

预算分配上还有一条隐性原则：4 个 Core POC 的总预算（48 天）一定要占满 12 周里前 8 周的主时段，不能往后挪。

原因是 4 个 Core POC 是 Cascadia 面试官最大概率深挖的栈。如果挪到 Week 9 之后，万一面试官在 Week 8 末就提出加面或调整流程，John 会陷在"主菜还没做出来"的窘境。

Important 和 Nice-to-have 这 5 个 POC 可以挤到 Week 9 之后做，因为即使做得不够深，面试时也能用"我列在了 next 30 day plan 里"这种说法兜回来。而 Core POC 是兜不回去的。

本计划没有覆盖的部分也要明确说清楚，避免 John 误以为这份文档是 onsite 准备的全部。简历改写、Cascadia 面试题型针对性刷题、recruiter 沟通话术、薪资 negotiation 准备这四件事不在本计划范围内，是 onsite 倒数 2 周的另一个工作流。Mock interview 的具体题库选择、行为面试 STAR 故事整理也不在这里，会在另一份独立文档里处理。本计划只聚焦"把 9 个 gap 转化成可展示的产出物"这一件事。

---

## 2. POC 优先级矩阵

下面这张表按"对 Cascadia 面试成功的影响"、"与其他 gap 的纠缠度"、"时间成本"三个维度给 9 个 POC 排序。Entanglement 这一列是 John 个人最看重的维度，意思是这个 POC 完成后会顺带覆盖几个其他 gap，比如 POC-03（RAG + eval）做完后基本上 POC-01（Strand Agents）和 POC-02（Bedrock AgentCore）的一半也跟着解决了，因为这三件事在 Cascadia 的 Insight Assistant 架构里是同一条数据流的三段。

| POC | Gap 名称 | 严重度 | 面试影响 | Entanglement（解锁哪些 gap） | 估时 | 综合排序 |
| --- | --- | --- | --- | --- | --- | --- |
| POC-03 | RAG + evaluation harness | 🔴 | 极高 | 解锁 POC-01 / POC-02 / POC-05 的检索语义 | 18 天 | 1 |
| POC-01 | Strand Agents | 🔴 | 极高 | 解锁 POC-02 / POC-03 的 agent 主干 | 12 天 | 2 |
| POC-02 | AWS Bedrock AgentCore Runtime | 🔴 | 高 | 解锁 POC-04 的 infra 模板 | 10 天 | 3 |
| POC-04 | AWS CDK Python | 🔴 | 高 | 解锁 POC-02 / POC-07 的部署链路 | 8 天 | 4 |
| POC-05 | Semantic Layer YAML | 🟡 | 高 | 解锁 POC-03 的语义检索语料 | 5 天 | 5 |
| POC-08 | Client-facing writing | 🟡 | 高 | 每个 POC 完成后贴尾写 1 页文档 | 4 天 | 6 |
| POC-06 | FHIR / HL7 | 🟡 | 中 | 解锁 POC-07 的 PHI 字段识别 | 6 天 | 7 |
| POC-07 | HIPAA + TJC + SOC 2 审计 | 🟡 | 中 | 解锁 POC-02 的 audit trail 设计 | 5 天 | 8 |
| POC-09 | Python 生产级代码质量 | 🟠 | 中低 | 横向作用于全部 POC 的代码仓库 | 4 天 | 9 |

矩阵里有两条线值得单独说。第一条线是 POC-03 → POC-01 → POC-02 这个三连，是 Cascadia agentic analytics 平台的实际架构骨架，三个一起做的时候可以共用一个 corpus、一个 eval set、一个 agent 主程序，所以放在 Week 1 到 Week 6 集中突破。第二条线是 POC-04 → POC-02 → POC-07，CDK 的 stack 同时承载 AgentCore Runtime 部署和 audit log 落地，写一个 stack 同时验证三块知识。

POC-08 和 POC-09 是横向能力，不独立排周，而是贴在每个硬 POC 的尾巴上。这样做的好处是文档样本和代码质量样本是真实项目的副产品，不是凭空造的"为了写文档而写文档"。

读这张表时还要留意一件事。"面试影响"这一列的判断口径不是"通用市场认可度"，而是"Cascadia 这个具体 JD 文本和这个具体 onsite 流程下的影响"。

比如 POC-07（合规审计）在通用 AI Engineer 面试里几乎不会被考，但 Cascadia 的 onsite 有一段 client-facing case study，案例几乎一定会牵涉 PHI 处理或 audit 设计，所以它在本表里被定为"中"而不是"低"。

同理 POC-08（client-facing writing）在大部分技术岗里都是软性加分项，但因为 Cascadia 把"15 pages of client-facing documentation per quarter"明确写入 must-have，本表把它升到"高"。这种基于 JD 文本逐条对照的口径，是这张矩阵和通用学习路线图的根本区别。

Entanglement 这一列还可以用一张依赖图的视角来读。

POC-01 是 agent 主干，POC-02 是 agent 部署到 AWS 的 runtime 层，POC-03 是 agent 内部的检索 + 评测层，三者共用同一份 Wikipedia 语料和同一份 Q-A 黄金集，是物理上耦合的。

POC-04 是 POC-02 的部署载体，POC-04 写的 CDK stack 就是 POC-02 的 infra-as-code 描述，两者不能割裂。

POC-05 是 POC-03 的"高级版"，把朴素文本检索换成结构化语义检索，写 YAML 时的口径和 POC-03 的 generation prompt 要保持一致。

POC-06 和 POC-07 串成"医疗数据合规链路"，POC-06 解决数据怎么解析，POC-07 解决解析后怎么留痕，两个连在一起讲故事比单独讲都更有说服力。

最后 POC-08 和 POC-09 在依赖图里是"覆盖在所有节点上的薄膜"。POC-08 是每个硬 POC 的产出物，POC-09 是每个硬 POC 的代码壳。把 POC-08 和 POC-09 想成是"输出层"而不是"独立节点"，更容易理解为什么它们不该排独立的周。这种依赖关系不止是排期意义上的方便，更是 onsite system design 一面里可以直接画在白板上的"我学习架构"。把这张图画出来本身就是一个加分故事。

---

## 3. Core POCs

下面是 4 个 Core POC 的详细定义。Core 的判定标准是 JD 里出现频率高、面试 system design 环节几乎一定会问、且 John 当前完全没有 hands-on 经验的方向。这 4 个 POC 在 12 周里占用了大约 60% 的总预算（48 天里约 28 天），是计划的真正主干。

每个 Core POC 在文档里都按统一结构写：第一条是 skill 定义，第二条是输入 / setup，第三条是产出物，第四条是成功标准，第五条是和 Cascadia JD 的连接点，第六条是时间预算，第七条是 tutorial 索引，第八条是踩坑或衍生练习。

这个结构和后面 Important POC 完全一致，便于横向对照。Cascadia onsite 上如果被要求"挑一个项目深讲"，John 可以按这 8 条快速搭出 3 分钟讲述骨架。

---

### POC-01: Build a tiny Strand Agent over a Wikipedia corpus

- 这个 POC 练习的核心 skill 是 Strand Agents 框架的基本编程模型：怎么定义 tool、怎么注册到 agent loop、怎么处理多轮对话状态。Strand 是 Cascadia 在 production 用的框架，JD 里明确写"we use Strand Agents in production"，所以这是非问不可的栈。
- 输入数据集用 Wikipedia 上 50 篇关于 PNW 医疗地理（Seattle / Portland / Spokane 等）的文章导出 markdown，存到本地 SQLite。选这个语料是因为内容稳定、规模小、和后续 POC-03 的 RAG 评测可以复用同一份语料。
- 期望产出物是一个 100 行左右的 Python 脚本和一个 README，agent 可以回答"Seattle 有几家三级甲等医院"这类基于语料的问题。脚本里要明确分出 tool 定义层、agent runtime 层、CLI 入口层三块，方便后续扩展。
- 成功标准是 agent 能正确路由到 search_corpus 这个 tool、能在 multi-turn 对话里保持上下文、能处理"语料里没有"的兜底情况。三件事缺一不可，因为 Cascadia 面试官最常考的 agent 工程问题就是这三个。
- 和 JD 的连接点：JD 里 "comfortable with one LLM application framework" 是 strongly preferred，而 Strand 是该框架的首选。这个 POC 解决的就是这条要求。
- 时间估算 12 天，分布：3 天读 Strand 文档和例子，4 天写第一版能跑通的代码，3 天迭代 tool 设计和 prompt，2 天写 design doc 和录一段 5 分钟的讲解视频。
- 对应 tutorial 文档：`tutorials/01-strand-agents-quickstart-cn.md`，教程会带 John 从 Strand 装包、定义第一个 tool、加入 memory、到把 agent 跑成 CLI 这四个阶段。
- 易踩坑点提示：Strand 的 tool 注册是 decorator-based，第一次容易把 type hint 写错导致 schema 推导失败；推荐先把 tool 函数签名写到能通过 mypy strict 模式，再注册到 agent。这条经验在 onsite 讲 debugging story 时是非常具体的细节锚点。

---

### POC-02: Stand up a minimal Bedrock AgentCore Runtime deployment

- 这个 POC 的核心 skill 是 AWS Bedrock AgentCore Runtime 的实际部署经验：怎么把一个 agent 定义注册到 AgentCore、怎么配 session 管理、怎么读 CloudWatch 里 agent 的 trace。AgentCore 是 2024 年底才 GA 的服务，市场上有手实操经验的人非常少。
- 输入设置：复用 POC-01 的 Strand agent 定义，把它打包成 AgentCore 兼容的 runtime 格式，部署到一个个人 AWS 沙箱账号。语料和 tool 不变，重点切换在 runtime 这一层。
- 期望产出物是一个能从 AWS CLI invoke 的 AgentCore endpoint、一个 session log 截图集、一份"AgentCore vs 自部署 Lambda agent"的取舍对比短文（约 1 页）。这份对比文档在面试 system design 环节直接能用。
- 成功标准是 endpoint 能稳定响应、session state 在 30 分钟内能被正确读回、CloudWatch 里能看到 trace span。Cascadia 的运营是按 client 隔离 AgentCore session 的，所以 session 隔离一定要做对。
- 和 JD 的连接点：JD 列出的部署技术栈是 "Bedrock, AgentCore Runtime, Lambda, ECS, CloudWatch"，AgentCore 在第二位，是当前市场上最稀缺的 hands-on 经验。
- 时间估算 10 天，分布：2 天读 AWS doc 和 limit 说明，3 天搭基础部署，3 天调通 session 和 trace，2 天写对比文档和成本估算。
- 对应 tutorial 文档：`tutorials/02-bedrock-agentcore-runtime-cn.md`，教程会覆盖 AgentCore 的概念模型、IAM 配置、session lifecycle、和常见错误码。
- 风险提示：AgentCore 调用的 Bedrock 模型按 token 计费，沙箱账号要设 budget alarm，否则一次循环调用 bug 可能在几小时内产生几十美元的账单。这条踩坑经验同时是 cost-aware engineering 的故事素材。

---

### POC-03: Build a RAG pipeline with a measurable evaluation harness

- 这个 POC 的核心 skill 是 RAG 系统的端到端实现 + 可量化 evaluation。重点不在 retrieval 的精巧，而在 eval harness 的存在本身：能不能产出 hit rate、MRR、faithfulness 这几个数字，并能解释每个数字的口径。
- 输入设置：用 POC-01 的 Wikipedia 50 篇语料，加上一份手工构造的 50 条 Q-A 黄金集（每条标注期望的支持段落 ID）。embedding 用 OpenAI text-embedding-3-small 或 Bedrock Titan，vector store 用 ChromaDB 本地版。
- 期望产出物是 retrieval 子系统、generation 子系统、eval 子系统三个 Python 模块，一份 eval 报告 markdown 表（baseline、chunk size 调整、reranker 加入三组对比），一份"我会怎么把这套 eval 套到 Cascadia 客户语料上"的 1 页应用方案。
- 成功标准是三个：retrieval hit@3 能从 baseline 提升 10 个百分点以上、能讲清 faithfulness 指标的定义和局限、能写出 5 个有意义的 eval set 拓展方向。Cascadia 的 RAG 面试题大概率绕 eval 转，所以 eval 比 retrieval 本身更重要。
- 和 JD 的连接点：JD 里 "experience with at least one retrieval-augmented-generation (RAG) implementation including evaluation. We will probe how you measured RAG quality" 这句话几乎是 POC-03 的直接定义。
- 时间估算 18 天，分布：2 天读 RAG eval 文献（RAGAS、Ares 等），4 天写 retrieval 和 generation，5 天构造 Q-A 集和 eval harness，4 天跑 3 组对照实验，3 天写 eval 报告。
- 对应 tutorial 文档：`tutorials/03-rag-with-eval-harness-cn.md`，教程会重点讲 eval set 怎么构造、什么指标值得追、什么指标是噪音。
- 容易被忽略的细节是 eval set 的"反例"覆盖：要刻意构造 10 条语料里没有答案的问题，看 agent 是否能稳定输出 "I don't know"。Cascadia 临床场景里 hallucination 的成本极高，面试官常追问这一点。

---

### POC-04: Provision a Lambda + S3 + IAM stack via AWS CDK Python

- 这个 POC 的核心 skill 是 AWS CDK Python 的写法：怎么用 Construct 组合资源、怎么配 IAM Policy、怎么把 stack 跑通 cdk deploy / cdk destroy 的完整 lifecycle。Cascadia 的所有部署都过 CDK 模板，没有 CDK 经验等于不能上工。
- 输入设置：从零起一个 CDK Python 项目，目标资源是一个 Lambda（Python runtime）、一个 S3 bucket、一个 DynamoDB 表、相应的 IAM Role 和 Policy。Lambda 的功能是读 S3 文件、写 DynamoDB，逻辑越简单越好。
- 期望产出物是 `app.py` + `stacks/main_stack.py` + `tests/test_stack.py`（用 CDK assertion 模块），一份 `cdk synth` 出来的 CloudFormation 模板，一份 README 讲怎么部署和销毁。
- 成功标准是：cdk deploy 一次成功、cdk destroy 不留垃圾资源、test_stack 里至少有 3 条 assertion（IAM Policy 不含 wildcard、S3 bucket 加密开启、Lambda timeout 不超过 30 秒）。
- 和 JD 的连接点：JD 里 "Familiarity with AWS CDK (Python) is a meaningful plus because every Cascadia deployment ships through CDK"，这是 meaningful plus 但也是日常工作必须的能力。
- 时间估算 8 天，分布：1 天读 CDK Python doc，3 天写 stack 和测试，2 天调通部署，2 天迭代 IAM 最小权限和写 README。
- 对应 tutorial 文档：`tutorials/04-aws-cdk-python-quickstart-cn.md`，教程会带从 cdk init 到 cdk deploy 的完整流程，重点讲 IAM 最小权限和测试模式。
- 衍生练习：在第一个 stack 跑通后，把它拆成 2 个 stack（一个网络层、一个应用层）做跨 stack 引用，这是 Cascadia production 部署最常见的拓扑，问到的概率比单 stack 高。

---

## 4. Important POCs

Important 这一档是 4 个 POC，对应 Cascadia JD 里 strongly preferred 和 preferred 那一段的能力要求。

它们的共同特征是"做好可以显著加分、不做也不会一票否决"，所以时间预算总和控制在 20 天以内，平均每个 5 天。

这一档里 POC-05 和 POC-08 有 CedarRidge 项目的现成素材可以复用，所以执行起来会比 POC-06 和 POC-07 轻松一些。

需要提醒的是，Important 这一档的 POC 在 onsite 上的"问到概率"远大于"考深度"。

面试官会拿这一档作为切入点，比如开场问"你做过 FHIR 吗？"。如果答"做过 Patient/Encounter/Observation 三种 resource 的解析"，面试官就会顺势深入；如果答"没做过"，面试官就会换个话题。

所以这一档的目标是"每个 POC 都能有一个 30 秒的开场介绍 + 2 分钟的深入故事"，不是把每个都做到精。

---

### POC-05: Design a Semantic Layer YAML over a Snowflake-shaped schema

- 这个 POC 的核心 skill 是 semantic layer 的 YAML 写法和元数据组织，重点不在 Snowflake 语法本身，而在"怎么把一个临床运营指标（比如 average length of stay）形式化成 entity、measure、dimension 三件套"。这是 Cascadia 工作日常的核心动作。
- 输入设置：用 Synthea 生成的合成医院数据，导入本地 PostgreSQL（不必真上 Snowflake，schema 形状一致即可），围绕 5 个临床运营指标写 semantic YAML：ALOS、bed occupancy、readmission rate、handoff completion、order turnaround time。
- 期望产出物是一份 200 到 300 行的 semantic.yaml、一份配套的"指标定义口径文档"（每个指标对应一段 1 段话的英文定义）、一份"和 CedarRidge 项目用过的口径有什么不同"的对照表。
- 成功标准是：5 个指标都能在 YAML 里被一段 SQL 表达式拼出来、每个指标的 grain 和过滤条件被显式声明、能讲清"为什么不用 view 而要用 semantic layer"这一题。
- 和 JD 的连接点：JD 里 "Partner with each client's senior clinical analyst to codify their internal metric definitions into the semantic layer YAML" 是日常职责的核心动词。
- 时间估算 5 天，因为 CedarRidge 项目有现成素材可以复用，主要工作是重写、规整、补对照表。
- 对应 tutorial 文档：`tutorials/05-semantic-layer-yaml-design-cn.md`，教程会讲 cube 形态、metric layer 形态、纯 view 形态三种做法的取舍。
- 复用说明：CedarRidge 项目里写过的 maternity 指标 YAML 可以直接搬过来作为第 6 个指标补在尾巴上，作为"在职项目素材"和"练习项目素材"的桥梁，在面试讲故事时可以衔接两段经历。

---

### POC-06: Parse FHIR Bundle JSON from Synthea data into a normalized table

- 这个 POC 的核心 skill 是 FHIR 资源模型的实操理解：怎么读 Patient、Encounter、Observation 三种 resource 的 JSON 结构，怎么把嵌套字段拍平到关系表，怎么处理 reference 字段（"Patient/abc-123"这种）。
- 输入设置：用 Synthea 生成 100 个合成病人的 FHIR Bundle JSON 文件，写一个 Python 脚本把三种 resource 抽取到 PostgreSQL 三张表里，保留 reference 关系。
- 期望产出物是抽取脚本、三张表的 schema DDL、一份"FHIR vs CedarRidge 用的自定义 schema"的对照短文。这份短文在面试讲述医疗数据经验时直接能用。
- 成功标准是：100 个 Bundle 都能解析无报错、reference 字段能正确关联、能讲清 FHIR 里 valueQuantity、valueCodeableConcept、valueString 这几种值类型的区别。
- 和 JD 的连接点：JD 里 "prior exposure to healthcare data formats (FHIR, HL7)" 是 preferred 级别，不是 must-have，但讲不出 FHIR 会显得医疗经验单薄。
- 时间估算 6 天，包括读 FHIR R4 spec 关键章节、写解析脚本、踩 reference 类型的坑。
- 对应 tutorial 文档：`tutorials/06-fhir-bundle-parsing-cn.md`，教程会聚焦 R4 的 3 种核心 resource 而不是覆盖全部 145 种。
- 范围控制提示：FHIR 的资源数量极多，新人最容易陷在"读完整本 spec 再动手"的陷阱里。本 POC 严格限制只处理 3 种 resource，宁可在面试时讲"我只做了 Patient/Encounter/Observation 但讲得清楚"，也不要假装覆盖全部。

---

### POC-07: Draft an audit trail design aligned to HIPAA + TJC + SOC 2

- 这个 POC 的核心 skill 不是写代码，而是写一份"如果 Cascadia 客户的 compliance officer 来 review，他能看懂"的审计设计文档。重点是把 HIPAA Privacy Rule、TJC handoff 标准、SOC 2 CC7 三类要求映射到具体的 log 字段和保留策略。
- 输入设置：以 POC-02 的 Bedrock AgentCore 部署为底座，假想这是一个客户 production 环境，写一份"audit trail design doc"，列出每条 log 要记什么字段、存哪里、保留多久、谁能访问。
- 期望产出物是一份 3 到 4 页的 design doc，包含字段表、retention 表、access control 表、incident response playbook 一节。所有引用要明确标出 HIPAA 164.312 哪一条、TJC handoff 哪一项。
- 成功标准是：3 个合规框架的关键条款都能映射到具体设计、能讲清 PHI 字段在 log 里要怎么 redact、能识别出"如果 audit log 本身丢了"的兜底方案。
- 和 JD 的连接点：JD 里 "Author audit-trail and compliance documentation aligned to HIPAA, TJC hand-off communication standards" 几乎是 POC-07 的直接对应。
- 时间估算 5 天，主要花在读 HIPAA / TJC / SOC 2 原文摘录、写文档、和 ChatGPT 模拟"合规官"做 dry run。
- 对应 tutorial 文档：`tutorials/07-audit-trail-hipaa-tjc-soc2-cn.md`，教程会以 checklist 形式给出最小可行 audit trail 字段表。
- 面试用法提示：这份文档在 onsite client-facing case study 环节是直接的"现成案例"，可以演示成"如果你是 Cascadia 的合规对接工程师，会怎么回应一个客户合规官的 review"。这种从 artifact 反推 conversation 的演练比空讲合规理论有力得多。

---

### POC-08: Produce a client-facing one-pager for each prior POC

- 这个 POC 的核心 skill 是把技术工作翻译成"non-technical clinical operations leader 能读懂的 1 页文档"。Cascadia JD 明确写要产 10 到 15 页 client-facing 文档每季度，这是硬性要求。
- 输入设置：把 POC-01 到 POC-07 的每个 POC 都贴尾写一份 1 页 client-facing one-pager，强制不用 jargon、强制有"为什么这件事对 Charge Nurse 重要"这一段。
- 期望产出物是 7 份 markdown one-pager（每份约 250 到 300 字），统一格式：问题、做法、产出、下一步。汇总成一个 portfolio PDF，在 onsite 时作为 writing sample 提交。
- 成功标准是：每份都能在 90 秒内被一个不懂 LLM 的人读懂、不出现"agent loop"或"vector embedding"这种术语、每份都有可执行的 next step。
- 和 JD 的连接点：JD 里 "strong written communication skills... roughly ten to fifteen pages of client-facing documentation per quarter; this is not negotiable" 这一条 must-have。
- 时间估算 4 天，分摊在每个硬 POC 完成后的半天里。
- 对应 tutorial 文档：`tutorials/08-client-facing-writing-templates-cn.md`，教程会给 4 个 one-pager 模板和 1 个反面例子。
- 自检方法：每写完一份就用语音读一遍录下来，回放时如果自己听到任何一句"卡顿"或"绕"，就回去重写那一段。这个方法源自 Amazon "narrative memo" 文化里的 read-aloud 实践，比纯文字校对发现问题更快。

---

## 5. Nice-to-have POC

Nice-to-have 这一档只有 POC-09，对应 JD 里 "production-grade or production-adjacent codebase" 这条要求。

把它放在 nice-to-have 而不是 important，不是因为不重要，而是因为这件事不能独立做。它本质上是"把前面 8 个 POC 收拾干净"，依赖前置 POC 的完成度。

Week 11 到 Week 12 是这件事自然成熟的时间窗。

另一个把它降级到 nice-to-have 的理由是，面试场景里"production-grade Python"更多是通过 code sample 来评估的，不是通过提问。

Cascadia 在 take-home Python and SQL 这一关会要 John 提交代码，那一关的评分维度才是这一项真正被考察的地方。

所以 POC-09 的目标是"让任何一份 take-home 都能交付得像样"，而不是"做出独立的展示点"。

---

### POC-09: Apply production-grade Python hygiene across the POC repo

- 这个 POC 的核心 skill 是把前面 8 个 POC 的散乱脚本统一收编到一个仓库里，加上 uv 包管理、ruff / mypy 静态检查、pytest 单测、GitHub Actions CI 这套"看着像生产代码"的外壳。重点不是修每一行代码，而是把 repo 收拾到能扔出来当 code sample 的状态。
- 输入设置：一个 monorepo，子目录就是 9 个 POC，根目录放 `pyproject.toml`、`.pre-commit-config.yaml`、`.github/workflows/ci.yml`。
- 期望产出物是绿色 CI badge、覆盖率不低于 40% 的单测、零 ruff warning 的代码、一份顶层 README 串联 9 个 POC 的故事线。
- 成功标准是：repo 链接发给 Cascadia 招聘官时，第一眼不会被"这是周末脚本"的感觉劝退、面试官点开任意一个子目录都能看到合理的结构。
- 和 JD 的连接点：JD 里 "strong Python proficiency including at least one production-grade or production-adjacent codebase. We will ask to discuss a code sample during the technical loop"，POC-09 让这个 monorepo 本身成为 code sample。
- 时间估算 4 天，集中在 Week 11 到 Week 12，因为前面的 POC 还在改动时不值得过早收紧 CI。
- 对应 tutorial 文档：`tutorials/09-python-production-hygiene-cn.md`，教程会给 uv + ruff + mypy + pytest + actions 这套最小组合的配置文件模板。
- 把 README 串联故事的方式建议参照 Anthropic / OpenAI 的 cookbook 风格：每个子目录上写一段 2 到 3 行的"why this exists"，让点开链接的招聘官在 30 秒内能 grasp 整个 portfolio 的脉络。

---

## 6. 教程目录索引

下面是 9 个 POC 对应的教程文档索引。教程本身是独立文档，不在本计划里展开，只列出归档位置。POC 的脚手架代码放在 `pocs/` 子目录下，每个 POC 一个子文件夹。

| POC | Gap 名称 | Tutorial 文件名 | POC 脚手架路径 |
| --- | --- | --- | --- |
| POC-01 | Strand Agents | `tutorials/01-strand-agents-quickstart-cn.md` | `pocs/poc-01-strand-agents/` |
| POC-02 | AWS Bedrock AgentCore Runtime | `tutorials/02-bedrock-agentcore-runtime-cn.md` | `pocs/poc-02-bedrock-agentcore/` |
| POC-03 | RAG + evaluation harness | `tutorials/03-rag-with-eval-harness-cn.md` | `pocs/poc-03-rag-eval/` |
| POC-04 | AWS CDK Python | `tutorials/04-aws-cdk-python-quickstart-cn.md` | `pocs/poc-04-cdk-python/` |
| POC-05 | Semantic Layer YAML | `tutorials/05-semantic-layer-yaml-design-cn.md` | `pocs/poc-05-semantic-yaml/` |
| POC-06 | FHIR / HL7 | `tutorials/06-fhir-bundle-parsing-cn.md` | `pocs/poc-06-fhir-parsing/` |
| POC-07 | HIPAA + TJC + SOC 2 审计 | `tutorials/07-audit-trail-hipaa-tjc-soc2-cn.md` | `pocs/poc-07-audit-trail/` |
| POC-08 | Client-facing writing | `tutorials/08-client-facing-writing-templates-cn.md` | `pocs/poc-08-one-pagers/` |
| POC-09 | Python 生产级代码质量 | `tutorials/09-python-production-hygiene-cn.md` | `pocs/poc-09-repo-hygiene/` |

教程文档的写作责任不在本 skill 范围内。本计划只锁定文件名和归档位置，确保后续教程作者和 POC 实现者引用一致。

命名规则上，tutorial 文件名以"NN-主题-关键词"的双连字符短语命名，主题名优先用英文技术词以便和官方文档术语对齐。POC 脚手架目录名同样用"poc-NN-主题"格式，避免出现中文目录路径在跨工具链时的兼容问题。

每个 POC 子目录里建议有 `README.md`（讲清这个 POC 在做什么）、`src/` 或 `app/`（实际代码）、`eval/` 或 `tests/`（评估或测试）、`docs/`（client-facing one-pager 等文档产出物）这四个子结构。

---

## 7. 执行节奏与里程碑

下面是 12 周（2026 年 3 月 2 日到 5 月 24 日）的周度排布。整体节奏遵循"先主干、后旁支、横向能力贴尾"的原则。Mock interview 在 Week 4、Week 8、Week 12 各一次，用来强制把当时的进度讲成故事。

| 周次 | 起止日期 | 主推 POC | 次要 POC（贴尾） | 里程碑 |
| --- | --- | --- | --- | --- |
| Week 1 | 03-02 ~ 03-08 | POC-01 Day 1-5 | 无 | Strand agent 第一版能跑 |
| Week 2 | 03-09 ~ 03-15 | POC-01 Day 6-12 | POC-08 写第一份 one-pager | Strand POC 完工 |
| Week 3 | 03-16 ~ 03-22 | POC-03 Day 1-7 | POC-08 第二份 | Q-A 黄金集 50 条到位 |
| Week 4 | 03-23 ~ 03-29 | POC-03 Day 8-14 | Mock interview #1 | RAG eval baseline 报告产出 |
| Week 5 | 03-30 ~ 04-05 | POC-03 Day 15-18 + POC-02 Day 1-3 | POC-08 第三份 | RAG eval 3 组对比完工 |
| Week 6 | 04-06 ~ 04-12 | POC-02 Day 4-10 | POC-08 第四份 | AgentCore endpoint 上线 |
| Week 7 | 04-13 ~ 04-19 | POC-04 Day 1-5 | POC-05 Day 1-2 | CDK stack 第一版 deploy 成功 |
| Week 8 | 04-20 ~ 04-26 | POC-04 Day 6-8 + POC-05 Day 3-5 | Mock interview #2 | semantic.yaml 完工 |
| Week 9 | 04-27 ~ 05-03 | POC-06 Day 1-6 | POC-08 第五份 | FHIR 解析脚本完工 |
| Week 10 | 05-04 ~ 05-10 | POC-07 Day 1-5 | POC-08 第六份 | audit trail design doc 完工 |
| Week 11 | 05-11 ~ 05-17 | POC-09 Day 1-2 | POC-08 第七份 | monorepo CI 绿、ruff 零警告 |
| Week 12 | 05-18 ~ 05-24 | POC-09 Day 3-4 | Mock interview #3 | portfolio PDF 和 demo 视频 |

Week 4 的 mock interview 重点考 RAG 工程和 agent loop 设计，对应 POC-01 和 POC-03 的进度。Week 8 的 mock interview 重点考 AWS infra 和 semantic layer，对应 POC-02 / POC-04 / POC-05。Week 12 的 mock interview 是 full loop 模拟，覆盖 system design、client-facing case study、code review 三段，对应所有 POC 的累积。

节奏上要警惕两个失败模式。

第一个是 Week 3 到 Week 5 这段 RAG eval 集中期容易超时，因为 eval set 构造比想象的慢。必须在 Week 3 周末做一次进度 checkpoint，如果 Q-A 黄金集没到 50 条就要直接砍到 30 条保进度。

第二个是 Week 7 的 CDK，因为是 John 完全没碰过的栈，要预留半天专门处理 AWS 账号权限和 bootstrap 这种"看起来不该卡但一定会卡"的环节。

三次 mock interview 的设置也要再细化一下。

Week 4 那次找一位有 LLM 应用经验的朋友扮演 senior engineer，重点考"你的 Strand agent 长什么样、agent loop 怎么 debug、tool 设计为什么选这几个"。

Week 8 那次找一位有 AWS 经验的朋友扮演 platform engineer，重点考"AgentCore vs Lambda 自部署的取舍、CDK stack 拆分逻辑、cost 控制思路"。

Week 12 那次是 full loop 模拟，最好找两位朋友分别扮演技术面和 client-facing 面。技术面考 system design 白板题，client-facing 面给一个临床场景模糊问题，看 John 能不能在 20 分钟内问出 3 个关键澄清问题、画出 1 张草图、产出 1 段口头提案。

除了 mock interview，还要嵌入两次外部"小考"。

第一次在 Week 5 末，把 POC-03 的 eval 报告发给一位有 RAG 经验的资深工程师做 30 分钟 review，目的是用外部目光识别盲点，不追求他给"好与不好"评价。

第二次在 Week 10 末，把 POC-07 的 audit trail design doc 发给一位有医疗合规背景的人做同样 30 分钟 review。

这两次 review 的反馈要逐字记入 weekly journal，是 onsite 回答"how do you incorporate feedback" 类问题的最直接素材。

12 周里还有两个产出物要在 Week 6 和 Week 11 各做一次中期总结，分别叫"半程 status report"和"终期 portfolio review"。

半程报告是一张 1 页 markdown，列已完成 POC、当前卡点、剩余预算分配，主要给自己看，强制 step back 评估方向是否正确。

终期 portfolio review 是一份 5 页 PDF，覆盖 9 个 POC 的产出物索引、关键 metric、5 张最重要的截图。这份 PDF 在 onsite 之前发给 recruiter 作为补充材料，也在 onsite 当天打印一份带过去。

两个总结都不是新工作，是对前面工作的"收口"动作。Week 6 的半程报告还有一个隐藏功能：它强制 John 在那个时间点"暂停冲刺、抬头看路"，这个动作如果没有外部触发就不会发生，但缺了它 POC 之间会缺乏粘合。Week 11 的 portfolio review 则是 onsite 物料的最终装订，需要把所有 one-pager 转 PDF、把代码 demo 录 GIF、把 architecture 草图誊抄到正式画图工具里，这些事各只要 2 到 3 小时但分别都不能省。

12 周结束后的状态目标是：4 个 Core POC 都有可运行代码 + 1 页 one-pager + 5 分钟讲解视频，5 个 Important / Nice-to-have POC 至少有可展示产出物，整个 monorepo 在 GitHub 上 public 可访问，portfolio PDF 包含 7 份 client-facing one-pager。

这个状态足以支撑 Cascadia 的 onsite 四面，包括 system design 和 client-facing case study 两个最关键的环节。

如果实际执行中出现严重偏差，触发条件和应急方案如下。

第一种情况是 Week 5 结束时 POC-03 的 eval baseline 还没跑出来。此时要立刻把 POC-03 缩成"只跑 retrieval hit@k，不做 generation faithfulness"，把节省的时间挪到 POC-02，因为 onsite 会问"你的 RAG 是怎么部署的"而不是"你的 faithfulness 是 0.83 还是 0.85"。

第二种情况是 Week 8 结束时 POC-04 的 CDK 还没 deploy 成功。此时要降级到"只写 stack 代码 + cdk synth 产 CloudFormation 模板，不真的 deploy"，因为 Cascadia 内部有标准 CDK 模板，面试官关心的是写法而不是部署成功。

第三种情况是 Week 10 后某个面试反馈说"我们最关心 X"，X 不在本计划里。此时要砍掉 POC-09 的精修，把 4 天预算挪给 X 的速成。

12 周结束之外，5 月底到 6 月这段时间还要留出 1 到 2 周做面试题型针对性补强，包括 system design 白板演练、take-home Python 题型练手、SQL window function 复习。这些不进入 POC 计划，但作为"考前一周"的固定动作纳入个人日历。整个 gap fill 计划的成败标准最终只由一件事衡量：Cascadia onsite 结束后 John 能不能拿到 offer，或者即使没拿到，他能不能在 debrief 时清楚说出"我哪一题答得不够好、下次怎么改"。这件事比 9 个 POC 全部 100 分完成都更重要。
