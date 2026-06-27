# POC-01: 用 Strand Agents 写一个 Wikipedia 问答 Agent

> 这是 gap-fill plan 里 9 个 POC 中的第一个。对应 Gap：🔴 LLM agent framework (Strand Agents)。对应教程：[../../tutorials/01-strand-agents-quickstart-cn.md](../../tutorials/01-strand-agents-quickstart-cn.md)。

## 1. 这个 POC 在练什么

练习 Strand Agents 框架最核心的四个动作：装包并跑起来、定义第一个 tool 让 LLM 能调用、给 agent 加上 memory（让它记住前几轮的对话）、把整个 agent 包装成一个 CLI 程序能在终端跑起来。这四个动作是 Cascadia Insight Assistant 内部所有 agent 的最小骨架，掌握了这套骨架后，剩下的复杂度都是在这上面叠 tool 和叠 prompt 而已。

不需要业务背景，也不需要医院数据。Wikipedia 文章是为了让评估有干净的 ground truth，方便判断 agent 答得对不对。

## 2. 输入与起点

- 一个 10 篇 Wikipedia 文章组成的小语料库（自选 10 篇技术主题，例如 LLM、Vector Database、Snowflake、RAG、FHIR）。文章下载下来存成 `corpus/*.md`。
- 一个 50 条 Q-A 的黄金集（自己手工写）。每条问题对应一个明确的、可在语料库里找到的答案。
- Python 3.12 环境，用 `uv` 管理虚拟环境。LLM provider 用 OpenAI API（demo 阶段最便宜的选项）。

## 3. 期望产出

- 一个能在 CLI 里跑的 agent，输入 `python -m poc01 ask "What is RAG?"`，输出文字回答加上引用的 Wikipedia 文章名。
- 一份 `RUN.md`，记录黄金集 50 题里的命中数（目标：≥ 42 / 50 = 84% accuracy）。
- 一份代码 walkthrough（约 1 页），说明 agent loop、tool 定义、memory 实现各自是怎么做的。这份 walkthrough 会同时作为 POC-08 的客户向写作样本。

## 4. 验收标准

- Agent 能正确处理至少 1 轮上下文相关的追问（比如 "Tell me more about that"），证明 memory 模块在工作。
- 至少 80% 的回答带 citation（引用的文章名），证明 RAG 检索回路通畅。
- 评估脚本能在 60 秒内跑完 50 题，p95 延迟 < 6 秒/题。
- 代码用 `ruff check` + `mypy` 0 报错，证明把 POC-09 的 production hygiene 也带进来了。

## 5. 跟 Cascadia JD 怎么对得上

Cascadia JD 里"Strongly preferred: hands-on experience with at least one LLM application framework. Strand Agents, LangChain, LlamaIndex, Haystack, or comparable. We use Strand Agents in production"。这条 POC 走完后 John 可以在面试中讲"我用 Strand 写过一个 50 题黄金集准确率 84% 的 RAG agent，对 Strand 的 tool 注册机制、memory backend 选型、agent loop 的 retry 策略都能讲清楚取舍"。这是 hiring manager 听完会想继续往下追问的程度。

## 6. 时间估算

- Day 1-2：装包、跑通 hello agent、读 Strand 官方 quickstart 和源码里的 4 个 example。
- Day 3-5：定义第一个 tool（Wikipedia 检索）、跑通单轮问答。
- Day 6-8：加 memory（用 Strand 内置的 conversation memory）、跑通多轮上下文。
- Day 9-10：包装 CLI、写 evaluation 脚本、跑黄金集、调 prompt 把命中率从初始的 60-70% 拉到 ≥ 84%。
- Day 11-12：写 walkthrough 文档、清理代码、ruff + mypy 过关。

总计 12 天，每天 1.5 小时，共 18 小时。

## 7. 跟其他 POC 的连接

- 这个 POC 的 corpus 和 Q-A 黄金集会被 POC-03（RAG + eval harness）复用。POC-03 在这之上加入更严格的检索指标和更复杂的评估方法，所以 POC-01 的产物是 POC-03 的输入。
- 这个 POC 的 agent 主干会被 POC-02（Bedrock AgentCore Runtime）复用。POC-02 把同一个 agent 部署到 AgentCore Runtime 上，证明本地能跑的 agent 能搬上云。
- 完成后写的 walkthrough 是 POC-08（客户向写作）的第一个样本。

## 8. 已知坑点（提前预警）

- Strand 的 OpenAI provider 在 0.x 版本里 streaming token 计数偶有 bug，evaluation 脚本里不要依赖 token usage 字段做 cost 估算，用 wall-clock 时间。
- Wikipedia 文章里的 `<ref>` 标签会污染 LLM 上下文，预处理时记得用 regex 清掉。
- 黄金集自己写的时候有偏差，最好让另一个人盲审一轮，把模糊的题目（"about" / "kind of" 这种）改成精确的题目。
