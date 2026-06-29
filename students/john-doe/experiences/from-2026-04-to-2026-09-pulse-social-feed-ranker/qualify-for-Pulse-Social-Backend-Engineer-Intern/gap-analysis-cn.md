# Pulse Backend Engineer Intern 的 Gap 分析

> 这是 08 教学示例的 abbreviated 版本。真实 workflow 里 `qualify-execution-plan` 会产出 ~200 行的完整版含每个 gap 的"为什么这个对岗位重要"和"closing 之后能讲什么"。这里保留主干, 让学生看到 08 阶段 2 的产出形态。CedarRidge 文件夹有 200 行完整版示例 ([gap-analysis-cn.md](../../../from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/gap-analysis-cn.md))。

## 1. 相关性诊断

总体相关性：**基本不相关, 偏向 0 起点**。John 的 [resume-old.md](../../../../resume-old.md) 里没有任何 distributed systems / Go / 微服务 / Kubernetes / 生产级后端 相关经历, 只有 web 全栈课程项目 (Capstone Expense Tracker) 和 Cedar Ridge SQL 报表实习 (数据报表方向, 跟后端工程不沾边)。

JD 的最低门槛 ("strong proficiency in at least one systems-oriented language" 加 "ramp on Go quickly") 通过 fluent Python + 短期 Go 突击是可以达到的, 但所有 "Strongly preferred" 项 (Go production, gRPC, Kubernetes, Redis, message queue, AWS 部署) 几乎全空。

结论: 直接投, 简历过初筛概率低; take-home 通过率低。要把成功率推到合理水平, 必须用 3 个月时间手工补齐 backend mental model 加 1 个端到端 mini 项目作为 take-home 谈资。

## 2. Gap 拆解概览

| Gap | 严重度 | JD 证据 | 当前状态 | 缺失 | 3 个月 closeable? |
|---|---|---|---|---|---|
| Go 生产级代码 | 🔴 | "strong proficiency in at least one systems-oriented language. Go is what we use" | 课程级 Python, 0 Go | 语法 + 标准库 + production hygiene 全空白 | 可达 interview-credible, 不到 production-grade |
| gRPC + Protocol Buffers | 🔴 | implicit from JD's microservice mentions | 0 接触 | 完全空白 | 可达 demo 级 |
| Kubernetes + Helm 基础 | 🔴 | "Strongly preferred" K8s | 0 接触 | 完全空白, 不知道 pod / service / ingress 区别 | 可达 demo 级 |
| Redis / 内存数据库 | 🟡 | "Strongly preferred" Redis | 课程项目用过 PostgreSQL | 没用过 Redis 做生产路径 | 可达 |
| 微服务架构 vs monolith | 🟡 | implicit | 只做过 monolith web app | 没拆过单体, 没设计过服务边界 | 可达概念 + 1 个 POC |
| 可观测性 (tracing / metrics / structured logs) | 🟡 | implicit "Strongly preferred" | 0 接触 | 完全空白 | 可达 demo 级 |

## 3. 跨 gap 关联性

- POC-01 (Go production) 是其他所有 POC 的语言载体, 必须先做。
- POC-02 (gRPC) 复用 POC-01 的代码骨架, 紧跟其后做最划算。
- POC-03 (K8s) 给 POC-02 提供部署载体, 三件事可视为一条主线。
- POC-04 (Redis) 给 POC-02 加缓存层, 可与 POC-03 并行。
- POC-05 (微服务边界) 把 POC-02 拆成两个 service, 是 POC-02 的自然延伸。
- POC-06 (Observability) 横切所有 POC, 在最后 1-2 周集中加。

## 4. 诊断结论

可行, 但前提是 3 个月 (2026-02 到 2026-05) 每周至少 12 小时纯投入, 优先级集中在 🔴 三项。完成后 take-home 通过率从估计 ~10% 提到 ~50-60% 是合理预期。

## 5. 写给 fill plan 的 handoff 笔记

- 优先 🔴 三项 closeable 到 "面试能讲清楚 + 一个 working demo", 不强求 production-grade。
- 🟡 三项 closeable 到 "看完一篇官方文档 + 写过 200 行示例代码", 不强求深度。
- 时间预算 12 周 × 12 小时 = 144 小时, 给 fill plan 用作硬约束。
- John 没有 AWS account 额度做 production 部署 ($0 budget), 所有 K8s 实操用 minikube + 学生 free tier。
