# 临床病历摘要平台

> 时间：2025-06 到 2025-09。公司：MedSync Health。岗位：软件工程实习生，AI 团队。行业：医疗。

## 1. 背景

MedSync Health 是一家位于波士顿、50 人规模的临床 AI 创业公司。产品的目标是帮初级保健医生少花时间在下班后写病历，做法是把原始的 EMR 病历（Epic 和 Cerner 导出的 FHIR 格式 JSON）转成结构化的就诊摘要。

我入职的时候，团队已经有一个能跑的原型，但是离生产环境差得很远。Prompt 很脆，LLM 偶尔会幻觉出错误的药品剂量，数据摄入 Pipeline 是一个单文件 Python 脚本、偶尔会丢病历。一个 60 位医生规模的家庭医生集团的 Pilot 八周后就要启动，AI 团队 Lead 让我端到端负责整个重写。

我向 Tech Lead 汇报，紧密配合一位资深后端工程师和一位临床信息学顾问。

---

## 2. 我做了什么

我重写了系统的三层：摄入、摘要生成、对外服务。

摄入层方面，我把原来的单脚本 Pipeline 换成了事件驱动的架构。AWS Lambda 在前面挂着一个 SQS 队列，消费客户 HL7 网关推送过来的 FHIR Bundle JSON。每条病历会经过一步 PHI 去标识化（用 Microsoft Presidio，加上临床顾问写的一组正则规则），归一化到我设计的一个 14 实体的 PostgreSQL Schema 里，同时也归档到 S3 做冷存储。

摘要生成层方面，我设计了一套 Prompt 模板，输出格式用 Pydantic 模型做了强约束。输出 Schema 涵盖主诉、病史、评估、计划，以及一组引用源段落。我加了一条 Retry 链，第一次响应通不过 Schema 校验时会带着错误信息把 LLM 再问一次。为了抓幻觉，我写了一套评估工具，包含八个指标，其中两个（事实覆盖率、引用忠实度）会跑在我们的临床顾问手工标注的一份 300 条病历的 Gold Set 上。

为了让摘要更有依据，我搭了一个小的 RAG 层，检索内部的临床指南语料。检索器用 OpenAI Embedding 做 k-NN，向量存在 pgvector 里。Top 3 的指南条目会作为参考资料注入 Prompt。

对外服务层方面，我用 FastAPI 写了一个服务，对外就一个 summarize 接口，部署在 AWS ECS Fargate 上，前面挂 Application Load Balancer。我加了 Prometheus 指标、用 structlog 做结构化 JSON 日志、用 OpenTelemetry 做链路追踪。

---

## 3. 产出

实习结束的时候，系统已经在那个 60 位医生的 Pilot 站点跑上生产。

- 历史回填阶段处理了 250,000 条临床病历，生产环境每天大概 4,000 条新病历。
- 医生每条病例的查阅时间从基线的 12 分钟降到大约 4 分钟。这个数据是用 EMR 的用户行为埋点测的，统计的是 30 位医生在前后两周的对比。
- Pilot 第六周的医生满意度调查是 91%。主要的吐槽是摘要太长，于是我们随后上线了一个更短的版本。
- 结构化输出格式不合规率从原型阶段的 11% 降到生产阶段的 0.5% 以下，主要靠 Retry 链加 Prompt 里塞例子的组合。
- PHI 去标识化在内部测试集上的精确率是 99.4%，剩下两个假阳性是临床医生在审核时抓到的。

---

## 4. 技术栈

Python、FastAPI、LangChain、OpenAI GPT-4 API、Anthropic Claude API、Microsoft Presidio、PostgreSQL with pgvector、Pydantic、AWS（Lambda、SQS、ECS Fargate、S3、RDS）、Docker、Terraform、Prometheus、Grafana、OpenTelemetry、GitHub Actions。
