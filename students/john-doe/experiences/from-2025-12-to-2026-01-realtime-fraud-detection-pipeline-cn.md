# 实时欺诈检测 Pipeline

> 时间：2025-12 到 2026-01。公司：Forge Trading。岗位：数据工程合同工。行业：金融服务。

## 1. 背景

Forge Trading 是一家中等规模的衍生品经纪商。Compliance 团队当时跑的是一套老的规则引擎做交易欺诈检测：大概 80 条手写的 if-else 规则，每天会标记约 0.8% 的交易。在这些被标记的交易里，约 78% 是假阳性，意味着 Compliance 团队大量的时间花在排查正常交易上。

团队想试一套 ML 系统来辅助（不是替代）规则引擎，目标是降低假阳性的负担。Head of Data 找我做一个为期六周的寒假合同项目，负责搭流式基础设施和初版模型。我和两位资深数据工程师以及一位量化分析师一起工作。

项目范围一开始就划得很窄：搭好 Pipeline、训出一个 baseline 模型、在生产交易流上做 Shadow 运行、出一份是否值得继续投入的评估报告。

---

## 2. 我做了什么

Pipeline 从 Forge 内部的订单管理系统接出每一笔交易事件，写进 AWS MSK 上的一个 Kafka Topic。我写了一个 Spark Structured Streaming 任务，从 Topic 读取数据，把每笔事件和六个滚动特征聚合（1、5、30 分钟的交易频次，平均名义金额，地理异常分，商户风险分）做 Join，再用一个 XGBoost 模型打分。

Feature Store 我建在 Snowflake 上。每个聚合特征由一个独立的 Spark 任务每 30 秒重新计算一次，然后回填到一个低延迟的 Redis 缓存里，给流式打分服务读。所有六个特征的定义我都用 dbt 模型文档化，让下游分析师能清楚每个数到底是怎么算出来的。

我用 Kafka 的事务能力（Producer 侧）加上幂等 Sink（Consumer 侧用 event-id 做 Upsert 的 Key）实现了精确一次（exactly-once）的语义。这一点很关键，因为老 Pipeline 在压力下每周都会丢 1 到 2 条事件。

模型方面，我和量化分析师一起做了大约 30 个特征。我用六个月的标注历史交易数据（约 400 万条）训了一个 XGBoost 模型，并用 Optuna 做了贝叶斯调参。最终模型在 Holdout 月份上 AUC 是 0.94。

为了监控漂移，我接入了 PSI（population stability index）跟踪 Top 10 特征。Shadow 跑的期间触发了三次告警，其中两次分析师确认是上游真实的协变量漂移，于是我们提前重训，没有等模型在生产里掉性能。

调度方面用了 Airflow，每晚的重训 DAG 用 Great Expectations 数据质量契约把关。这 18 条契约挡掉了两次因为上游商户数据 Schema 变更导致的坏重训。

---

## 3. 产出

系统在生产交易流上 Shadow 跑了两周。下面这些是给 Head of Compliance 的最终报告里写的：

- 峰值处理 12,000 events/s，老的 Pipeline 最高大概 5,000。
- Holdout 上 AUC 0.94。在选定的工作点上，召回率与规则引擎持平（Shadow 期间没有漏掉欺诈），但假阳性标记率下降了 35%。
- 两周的 Shadow 窗口里抓到了 120 万美元规则引擎漏掉的真实欺诈交易。
- 六周生产运行期间零数据丢失，基线是每周大约两条。
- Great Expectations 契约挡掉了两次坏重训。

我离开之后团队批准了把这套系统转成实际打分。

---

## 4. 技术栈

Python、Apache Kafka（AWS MSK）、Apache Spark Structured Streaming、XGBoost、Optuna、Snowflake、dbt、Redis、Apache Airflow、Great Expectations、AWS（MSK、EKS、S3、IAM）、Docker、Terraform、Prometheus、Grafana。
