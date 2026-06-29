# Cedar Ridge 实习对 Cascadia AI Solutions Engineer 岗位的 Gap 分析

> 本文档基于 John Doe 当前简历基线（Cedar Ridge 妇产科 SQL 报表实习 + UW M.S. CS 在读课程）与 Cascadia Health Insights AI Solutions Engineer (New Grad) JD 的对照诊断。目的是给出诚实审计，为后续 fill plan 与 mini POC 设计提供输入。本文不输出"是否能拿到这个 offer"的乐观判断，只描述差距。

## 1. 相关性诊断

把 Cedar Ridge 这段实习按 "Cascadia 这条 JD 的关键能力栈" 做相关性切片，结论分三类。

直接相关的部分占比小。SQL 写作（含 Snowflake 表结构理解）、医疗运营场景的语境暴露（OB 病房、charge nurse、产后住院时长、高危产妇标记这些 metric 都是 Cascadia 客户场景里会复用的临床运营话题）、与一位资深 analyst 的口径对齐过程（Hannah 的 SQL review）这三件事是直接相关的。SQL + Snowflake 命中 JD 的 Required 项，临床运营 metric 的语境暴露算 FHIR/HL7 之外的间接行业背景。

间接相关的部分。Excel 报表流程意味着 John 见过 "需求方拿到数据后实际怎么用" 的下游环节，这对未来做 UAT（User Acceptance Testing）有一点心理预热，但不是工程能力。"愿意问问题" 的反馈说明 ambiguity tolerance 这一项有微弱信号，但没有可证伪的产出物。

不相关的部分占比大。LLM Agent、RAG、AWS Bedrock / AgentCore、CDK、semantic layer YAML 设计、HIPAA/TJC/SOC 2 audit trail 文档、client-facing 长文档这六大块在 Cedar Ridge 这段经历里完全没有出现。John 在那三个月里没有写过一行 LLM 调用代码，没碰过 AWS 部署，没写过一份对外文档。

把三类相加，整体相关性结论是 **偏弱**。Cedar Ridge 给 John 留下了一个 "医疗 SQL 实习生" 的标签，离 Cascadia JD 要求的 "AI Solutions Engineer" 还差一个完整的技术栈和一个完整的客户交付循环。如果不做拔高，这段经历能给 Cascadia 面试官的信号大致是 "他能写 SQL、见过 Snowflake、懂一点医疗术语"，仅此而已。这不足以撑起 JD 里 Strongly Preferred 的任何一项。

---

## 2. Gap 拆解概览

下面这张表把全部 9 个 gap 按严重度排开，是后续 fill plan 的总目录。

| # | Gap | 严重度 | JD 原文证据 | John 当前状态 | 缺什么 | 3 个月可闭合度 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | LLM agent framework (Strand Agents) | 🔴 Core | "hands-on experience with at least one LLM application framework. Strand Agents, LangChain, LlamaIndex..." | 零暴露，ML/NLP 课在 plan 中未开始 | 一个从 0 搭起来的 agent 项目 + 评估数据 | 可以闭合到 "能讲清楚 + 有代码样本" |
| 2 | AWS Bedrock AgentCore Runtime | 🔴 Core | "Deploy and operate the agent infrastructure on AWS (Bedrock, AgentCore Runtime, Lambda, ECS...)" | 仅有 Lambda 课级别暴露 + Cloud Practitioner 证书 | Bedrock model invoke + AgentCore 部署的实操路径 | 可以闭合到能跑通 demo，无法到生产级 |
| 3 | RAG 实现 + evaluation harness | 🔴 Core | "experience with at least one retrieval-augmented-generation (RAG) implementation including evaluation. We will probe how you measured RAG quality." | 零暴露 | 一套从 chunking 到 retrieval 到 eval metric 的端到端路径 | 可以闭合到 "能讲方法论 + 有数据" |
| 4 | AWS CDK (Python) | 🔴 Core | "Familiarity with AWS CDK (Python) is a meaningful plus because every Cascadia deployment ships through CDK." | 只见过 console + CloudFormation 片段 | 一个 CDK 部署的真实 stack | 可以闭合到 entry 水准 |
| 5 | Semantic layer YAML 设计 | 🟡 Important | "codify their internal metric definitions into the semantic layer YAML. Resolve definitional conflicts before they become production confusion." | 写过 SQL 但没设计过 semantic layer | 把 15 条 SQL 抽象成 metric 定义集合的工程化思路 | 可以闭合到能展示一份 YAML 样本 |
| 6 | FHIR / HL7 健康数据格式 | 🟡 Important | "Preferred: prior exposure to healthcare data formats (FHIR, HL7) or comparable regulated data environments" | 零暴露 | FHIR Patient / Encounter / Observation 资源的基本读写 | 可以闭合到能讲、能跑通 demo |
| 7 | HIPAA + TJC + SOC 2 audit trail | 🟡 Important | "Author audit-trail and compliance documentation aligned to HIPAA, TJC hand-off communication standards..." | 零暴露 | 一份 audit trail 的设计文档 + 实际产生 audit log 的代码 | 可以闭合到 "知道术语 + 写过一份模板" |
| 8 | Client-facing 书面表达 | 🟡 Important | "roughly ten to fifteen pages of client-facing documentation per quarter; this is not negotiable." | 零暴露，只有内部 SQL 报表 | 一份 10 页量级的客户交付文档样本 | 可以闭合到能展示一份样本 |
| 9 | Python 生产级代码质量 | 🟠 Nice-to-have | "strong Python proficiency including at least one production-grade or production-adjacent codebase. We will ask to discuss a code sample" | 课程级 + 2-3 个小项目 | type hint、测试、CI、模块化、log 的工程实践 | 通过 1-3 闭合时附带闭合 |

---

## 3. Core gaps deep dive

下面 4 个 gap 是 Cascadia 这条 JD 的硬技术门槛。任何一个没补上，technical phone screen 都过不了。

### 3.1 🔴 Gap 1：LLM agent framework (Strand Agents)

JD 在 "Education, Experience" 一节明确写 Strongly Preferred "hands-on experience with at least one LLM application framework. Strand Agents, LangChain, LlamaIndex, Haystack, or comparable. We use Strand Agents in production"。Landscape 03-role §3 进一步指出，一周里写代码占 35-45%，其中 agent 配置是核心。也就是说这不是 "见过就行" 的加分项，而是日常 40% 工作量的载体。

为什么这点对这个角色尤其重要：Cascadia Insight Assistant 本身就是一个 agent 产品，AI Solutions Engineer 的日常工作不是"使用 agent"，而是"按客户场景定制 agent 行为"。这要求工程师能读懂 framework 源码层的抽象、能区分 tool 调用 vs 单纯 prompt、能调试 multi-step reasoning 卡住的位置。光看过 ChatGPT API 调用不算。

John 的当前状态是零暴露。M.S. CS 课表里 NLP 在 plan 中但未开始，Cedar Ridge 实习没碰过任何 LLM API。即使是 LangChain 这种最大众的框架，他也没有可以拿出来讨论的代码样本。

缺的是一个完整的 agent 项目：从定义 tool、到管理 prompt、到处理多轮对话状态、到 trace 调用链。Cascadia 既然在生产环境用 Strand Agents，那么 demo 项目应该至少选 Strand 或 LangChain 其中一个深做，不要两个都做半截。闭合标志是 John 能在 60 分钟 technical phone screen 里讲清楚 "我为什么选这个 tool 抽象、agent 在什么情况下会卡住、我怎么排查"。配套的工件应该包括：可运行的 agent 代码仓、一份至少 20 个 case 的 conversation log、一份解释 "为什么我让这个 tool 接受这个 schema" 的设计 note。

### 3.2 🔴 Gap 2：AWS Bedrock AgentCore Runtime

JD 的 Accountability 一节里 "Deploy and operate the agent infrastructure on AWS (Bedrock, AgentCore Runtime, Lambda, ECS, CloudWatch)" 是动词 "Deploy and operate"。Landscape 03-role §3 也指出 CDK + AWS 部署是写代码的另一大块。AgentCore Runtime 是 Bedrock 上面专门跑 agent 的托管服务，2024 年下半年才推出，所以全市场都新，但 Cascadia 已经在用，意味着面试官会问。

John 当前有 AWS Cloud Practitioner 证书和课程里一点 Lambda 暴露，这只能算 "听过 AWS"。Bedrock 和 AgentCore 完全没碰过。

缺的是一条 "用 Bedrock invoke Claude / Nova 模型 + 用 AgentCore 部署 agent + 用 CloudWatch 看日志" 的实操路径。闭合到 entry 级别意味着 John 能讲 "Bedrock 和直接调用 Anthropic API 的区别在哪、AgentCore 比自己跑 ECS 省了什么"。完全闭合到生产级（IAM 细颗粒度、跨账号、KMS 加密、VPC endpoint）对 New Grad 不现实，3 个月内做不到，也不必做。

值得注意的是 AgentCore Runtime 在 2024 年下半年才 GA，市场上对它有经验的工程师非常少。这反而是 John 的机会：花两周读官方文档 + 跑通一个 demo，就能在面试中把这一项从 "完全不会" 拉到 "比一般 New Grad 都强"。这种新兴技术的早期投入是性价比最高的。

### 3.3 🔴 Gap 3：RAG 实现 + evaluation harness

JD 写得很重："experience with at least one retrieval-augmented-generation (RAG) implementation including evaluation. We will probe how you measured RAG quality." 注意 "We will probe" 这个词，意味着面试官会主动追问 "你怎么衡量 retrieval 召回率、怎么衡量 generation 的 faithfulness、ground truth 怎么来"。Landscape 03-role §4 把 evaluation harness 列为四大主要交付物之一。

John 当前是零暴露。SQL 报表写作不会触及任何向量检索、embedding 选型、chunking 策略的概念。

缺的是一个端到端 RAG 项目，含：corpus（最好是医疗领域文本，比如 ACOG 临床指南）、embedding model 选型、chunking 策略、retrieval top-k、generation prompt、evaluation harness（含 retrieval 的 recall@k、generation 的 faithfulness、可选 LLM-as-judge）。闭合标志是 John 能在面试中展示一张 evaluation 结果表，并解释 "为什么把 chunk size 从 512 改到 256 之后 recall 上升了但 faithfulness 下降了"。

evaluation harness 这一块是 Cascadia 比一般 LLM 应用公司更强调的：JD 用 "We will probe how you measured" 这种带威胁性的措辞，意味着面试官会拿出实际数字反问 "你这个 0.78 的 recall 是怎么算的、ground truth 集多大、有没有 held-out test set"。John 必须准备到能直接对答数字。

### 3.4 🔴 Gap 4：AWS CDK (Python)

JD 原文 "every Cascadia deployment ships through CDK" 这句加重词比一般 Preferred 强。Landscape 03-role §3 也写 CDK 是日常代码栈的一部分。Cascadia 客户多、deployment 多、每次都靠 CDK 模板拉起来，意味着新人入职第一周大概率就要读 CDK 代码。

John 当前只见过 AWS console 和零散的 CloudFormation YAML，没写过 CDK 代码。

缺的是一个能跑起来的 CDK Python stack：定义 Bedrock 调用所需的 IAM role、Lambda、S3 bucket、CloudWatch log group，通过 `cdk deploy` 真实部署到自己的 AWS 账号。闭合标志是 John 能讲 "CDK 比 CloudFormation 的 abstraction 抽到哪一层、Construct vs Stack vs App 的区别"。不需要做到深度（不需要 custom L3 construct、不需要 CDK Pipelines），entry 级别够用。

由于 CDK 是 Python，而 John 的 Python 本身也需要拔高（Gap 9），CDK 项目天然成为 Python 工程化的载体。fill plan 把 CDK 项目按 production-grade Python 标准（type hint、pytest、CI）来写，就能一鱼两吃。

---

## 4. Important gaps deep dive

下面 4 个是 JD 的 Strongly Preferred 和 Preferred，面试中会被探。这些缺口不会直接挂掉简历，但会决定面试官对 John 是 "新人能教" 还是 "新人完全没准备" 的印象。

### 4.1 🟡 Gap 5：Semantic layer YAML 设计

JD 的 Accountability 第三条 "Partner with each client's senior clinical analyst to codify their internal metric definitions into the semantic layer YAML. Resolve definitional conflicts before they become production confusion." Landscape 03-role §3 指出 semantic YAML 是核心交付物之一。

John 在 Cedar Ridge 写过 15 条 SQL，每条对应一个 metric（床位余量、医护排班、产后住院时长等），但他没把这些 metric 抽象成一份可被 agent 消费的 semantic layer 定义。换句话说，他做了 "查询" 但没做 "口径工程化"。

缺的是把 SQL 抽象成 metric definition 的工程意识：metric name、business definition（一句话能让护士长理解）、SQL expression、grain（粒度，按 patient / encounter / day）、filter、dependency、owner。闭合标志是 John 能拿出一份覆盖 Cedar Ridge 那 15 条 SQL 的 YAML 文件，并能解释 "如果护士长的 LOS 定义和医生的 LOS 定义不一样，我会怎么把这个冲突写进 YAML 而不是埋掉"。

这一项的特殊好处是它强烈复用 Cedar Ridge 这段经历的语境优势。其他候选人做 semantic layer 项目只能用合成数据，John 可以直接说 "我在 Cedar Ridge 写过 15 条这样的 SQL，现在我把它们形式化"。这是面试中最自然的故事弧，fill plan 应该优先放大这个角度。

### 4.2 🟡 Gap 6：FHIR / HL7 健康数据格式

JD 写 Preferred "prior exposure to healthcare data formats (FHIR, HL7)"。Landscape 01-industry §5 指出 2020 年 ONC 互操作性规则强制 FHIR 采用，整个行业的数据格式默认就是 FHIR。Cascadia 的 Insight Assistant 处理临床数据时大概率会用 FHIR Resource。

John 在 Cedar Ridge 看的是 Snowflake 里已经 normalized 过的 OLAP 表，没碰过原始 FHIR JSON。

缺的是对 FHIR 核心 Resource（Patient、Encounter、Observation、Condition、MedicationRequest）结构的基本理解，以及对 HL7 v2 message 的存在感（即便不深做，至少要能说 "我知道 HL7 v2 是 pipe-delimited 文本，主要老系统在用，FHIR 是 RESTful + JSON 的新一代"）。闭合标志是 John 能在 client-facing case study 面试里讲 "这个 query 我会从 Encounter resource 里拿 length-of-stay，从 Patient.birthDate 算年龄"。

实际操作上 Synthea（开源 FHIR 合成数据生成器）能在 5 分钟内造出几千份 patient bundle，FHIR 学习曲线相对平缓。这一项不应该成为 fill plan 的瓶颈，安排 1 周时间足够。

### 4.3 🟡 Gap 7：HIPAA + TJC + SOC 2 audit trail 设计

JD 的 Accountability 第六条 "Author audit-trail and compliance documentation aligned to HIPAA, TJC hand-off communication standards, and client-specific governance requirements." Landscape 03-role §3 指出客户文档（含 audit 与 HIPAA 说明）占工作时间 10-15%。

John 当前零合规暴露。Cedar Ridge 实习不涉及合规设计，他签的 NDA 也只是访问权限层面。

缺的是 "audit trail 在 LLM agent 上下文里到底要记什么" 的设计语言：每次 agent 调用记 user identifier、timestamp、prompt、retrieved chunks、tool calls、final response、是否触及 PHI、retention policy。TJC handoff 标准（医疗交班通信的合规要求）这一块对 New Grad 不要求深做，但要知道术语。闭合标志是 John 能拿出一份 1-2 页的 audit trail 设计 spec，并能讲 "为什么 PHI redaction 要发生在 log write 之前而不是之后"。

合规这一项的特殊性在于它的 "深度可调"。新人不需要懂 HIPAA 全文条款，只需要懂工程师能落地的那一小块（log 设计、PHI redaction、retention、access control）。fill plan 不要让 John 陷入读 HIPAA 法律条文的兔子洞，控制在 10-15 小时投入即可。

### 4.4 🟡 Gap 8：Client-facing 书面表达

JD 把这一条写成 Required 而不是 Preferred："roughly ten to fifteen pages of client-facing documentation per quarter; this is not negotiable." Landscape 03-role §3 和 §4 都把 client docs 列为四大交付物之一。

John 写过的东西到目前为止只有：课程作业 PDF、SQL 注释、给 Hannah 的内部 Excel 报表。他从来没写过一份面向非技术读者（charge nurse、compliance officer）的长文档。

缺的是 "把技术决策翻译成临床运营 stakeholder 能读懂的语言" 的能力，含结构（背景、目标、方案、风险、签字栏）、语气（不卑不亢、克制不卖弄）、视觉（mermaid 流程图、表格、关键术语 glossary）。闭合标志是 John 有一份 8-10 页的 sample post-deployment write-up，可以放进 portfolio 在 on-site case study 里直接展示。

提醒一点：Cascadia 的 on-site case study 环节大概率会让 John 现场写一段给 client compliance officer 的解释段落，不会让他直接展示已经写好的文档。也就是说光有 portfolio 不够，他需要把 client-facing 写作变成肌肉记忆。fill plan 应该安排他每周写一份 1-2 页的客户向短文练手，而不是只产出一份大文档就交差。

---

## 5. Nice-to-have gaps deep dive

### 5.1 🟠 Gap 9：Python 生产级代码质量

JD 写 "strong Python proficiency including at least one production-grade or production-adjacent codebase. We will ask to discuss a code sample during the technical loop." 这一条名义上是 Required，但因为可以借着 Gap 1-4 的项目顺带交付，所以放在 Nice-to-have 处理（即不需要单独立项）。

John 当前的 Python 是课程级 + 2-3 个小项目。这意味着他大概率没系统用过 type hint、pytest、CI、log、模块化、依赖管理（uv / poetry）、Makefile 这一套工程实践。

缺的是把 Gap 1-4 的项目从 "能跑" 升级到 "能给别人看" 的工程化封装。闭合标志是 John 的 GitHub 上有一个 repo 至少满足：清晰的 README、type hint 全覆盖、pytest 至少 3 个测试、log 配置、CI 跑过、依赖锁定。这是 Cascadia 面试官在 "discuss a code sample" 环节会下意识扫的几件事。

要避免的陷阱：不要为了显得 "工程化" 而过度抽象（比如把一个 200 行的 demo 拆成 10 个 module 加各种 abstract base class）。面试官能一眼看出这种 over-engineering，反而扣分。合理的标准是 "如果同事来 review 这个 repo，他能在 10 分钟内看懂目录结构"。

---

## 6. 跨 gap 关联性

九个 gap 不是孤立的，它们之间有几条强耦合，fill plan 的项目设计要利用这些耦合一鱼多吃，而不是 9 个项目分头做。

LLM agent framework（Gap 1）和 RAG（Gap 3）天然绑定。一个 agent 项目里只要接了 retrieval tool，evaluation harness 就要同时覆盖 agent 行为和 retrieval 质量。这两个 gap 共享一个项目骨架最合理。

Bedrock/AgentCore（Gap 2）和 CDK（Gap 4）是部署链上的两环。CDK 写的就是把 Bedrock / Lambda / IAM role 拉起来的 infrastructure，所以一个 CDK stack 同时覆盖两个 gap。

FHIR（Gap 6）和 Python 生产级代码（Gap 9）也耦合。FHIR resource 是嵌套很深的 JSON，要把它干净地解析、validate、转 dataclass 就必须用 pydantic、type hint、单元测试这些工程实践。也就是说选 FHIR 作为 RAG corpus 的来源，能同时驱动 Gap 6 和 Gap 9。

Semantic layer YAML（Gap 5）和 audit trail（Gap 7）有间接耦合。两者都是 "用 YAML / JSON schema 把业务规则形式化" 这件事的不同面向。一份样本项目如果同时产出 metric YAML 和 audit log schema YAML，能体现 "John 知道用 declarative artifact 沉淀业务规则" 的工程口味。

Client-facing 文档（Gap 8）是最后的 wrapper。Gap 1-7 的任何一个项目都需要一份对外说明文档收尾。把这份说明写成 client-facing 风格（背景、方案、风险、合规），就能同时闭合 Gap 8。这意味着 Gap 8 不需要独立项目，只需要在其他项目结束时多花 1-2 天写一份 deliverable。

把上面这几条耦合合起来看，9 个 gap 实际上可以收敛成 2-3 个有机的 mini POC：一个 "Strand Agent + RAG over FHIR + eval harness" 项目（覆盖 1、3、6、9），一个 "CDK + Bedrock/AgentCore 部署" 项目（覆盖 2、4），一个 "Cedar Ridge 15 条 SQL 抽象成 semantic YAML + audit trail spec + client-facing write-up" 项目（覆盖 5、7、8）。这是给 fill plan 的隐含建议。

更进一步看，这三个 POC 之间也可以串成一个连贯的故事：POC 1 是 "技术能力 demo"（我能从零搭一个 RAG agent），POC 2 是 "工程交付能力 demo"（我能把 demo 部署到云上），POC 3 是 "业务能力 demo"（我能把临床场景的 metric 工程化）。三个合在一起就回答了 JD 想要看到的 "技术 + 工程 + 业务" 三位一体。这是面试时讲项目的天然叙事弧。

---

## 7. 诊断结论

把前 6 节的诊断收一下结。

John 当前 Cedar Ridge 这段经历对 Cascadia AI Solutions Engineer 岗位是 **有条件可工作**，不是无条件可工作。

无条件不可工作的理由：4 个 Core gap 全部缺失意味着即使 phone screen 给到他一道 "讲讲你做过的 LLM 项目" 的开放题，他都没东西可讲。这不是简历润色能解决的问题，必须有项目产出。

有条件可工作的理由：Cedar Ridge 留下的三个底子是真的，1）SQL + Snowflake 命中 JD 的 Required，2）医疗 OB 病房语境是可以复用的行业背景，3）"愿意问问题" 的反馈虽然弱但方向对。这三个底子配上接下来 3 个月针对 9 个 gap 的拔高，整体能从 "偏弱" 推到 "可投递可进 phone screen"。

条件列在下面：

- 必须在 3 个月内交付 2-3 个 mini POC（按 §6 的耦合方式合并），不是 9 个独立小练习。
- 必须有一份至少 8 页的 client-facing sample doc 进 portfolio，否则 Required 的 written communication 项过不了。
- Cedar Ridge 这段经历本身要从 "我写了 15 条 SQL" 重新框成 "我把妇产科的 15 个 metric 抽象成 semantic layer + 用 agent 做了自然语言 BI 试点 + 部署到 Bedrock"。这个重新框定的过程就是 fill plan 02 要做的事，本文档不展开。
- John 必须接受一个事实：3 个月内能做到的是 "能讲清楚、有 code sample"，不是 "生产级别"。面试官会区分 New Grad 和有 2 年经验的人，不会要求他生产级别。但 New Grad 之间会比 "谁的项目更完整、谁讲得更清楚"，所以质量比数量重要。

如果 John 不接受这些条件（比如时间不够、不愿意做项目、想靠简历包装绕过），那么诊断退到 **不可工作**，他应该把 Cascadia 这条线降级成 stretch goal，把主线放到不要求 LLM 经验的岗位上。

---

## 8. 写给 fill plan 的 handoff 笔记

下面几条是 execution-plan-cn.md 的作者应该带着的约束和优先级。

优先级排序：先做 Gap 1 + 3 + 6 + 9（合并成一个 RAG over FHIR agent 项目），因为这是 Cascadia phone screen 80% 概率会问的话题，缺这个直接挂。其次做 Gap 5 + 7 + 8（合并成一个 Cedar Ridge 拔高项目），因为这是 on-site case study 环节会展示的 portfolio。最后做 Gap 2 + 4（合并成一个 CDK 部署项目），优先级最低但闭合成本也最低，1-2 周可以做出 demo 级别。

时间预算约束：John 是 UW M.S. CS 在读，3 个月内必须同时跑课表（ML、NLP 这两门是 plan 中的，2026 春季学期会跑掉一部分时间）。fill plan 不能假设他每天 8 小时投入，建议按每周 15-20 小时的可投入时长规划。

技术栈锁定：Strand Agents 是 Cascadia 的生产框架，必须选 Strand 而不是 LangChain。即便 LangChain 资料更多，也要选 Strand。Bedrock 上的 model 选 Claude（Cascadia 大概率用这个家族），不要选 Nova 或 Llama。

预算约束：AWS Bedrock 调用不便宜，AgentCore Runtime 也是按用量计费的。fill plan 在设计项目时要把 token 用量控制在 USD 100 之内。CDK 部署只要不忘了 `cdk destroy`，几乎免费。

不要做的事：不要为了凑数做生产级 HIPAA 实施（New Grad 做不到也不需要做），不要选难度太高的 FHIR 场景（比如 Bulk FHIR、SMART on FHIR 这种 OAuth 集成），不要试图模仿真实 PHI 数据（合规风险高，用 Synthea 这种合成 FHIR 数据集即可）。

文档化是终态而不是过程：每个 mini POC 结束时必须产出一份 client-facing write-up（8-10 页），这份文档本身就是 portfolio。不要等到三个 POC 都做完再统一补文档，因为那时记忆已经淡了。

最后一句留给 fill plan 作者：本文档把诊断锁在了 "偏弱但可工作" 这个判断上。如果在 fill plan 的执行过程中发现 John 实际投入度不够、或者某个 Core gap 闭合不及预期，应该回头修改 §7 的条件列表而不是放任 fill plan 跑空。诊断与执行之间的反馈回路必须保持开放。
