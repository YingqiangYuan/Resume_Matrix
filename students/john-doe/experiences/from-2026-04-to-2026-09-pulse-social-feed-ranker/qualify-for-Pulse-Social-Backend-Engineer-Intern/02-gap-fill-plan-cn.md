# Pulse Backend Engineer Intern 的 Gap Fill Plan

> 这是 08 教学示例的 abbreviated 版本。真实 workflow 里 `qualify-gap-plan` 第二部分会产出 ~350 行的完整版含 POC 优先级矩阵 + 教程目录索引 + 12 周时间线。这里只保留 POC 主干和时间线骨架。完整版可参考 CedarRidge 那份 [02-gap-fill-plan-cn.md](../../../from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/02-gap-fill-plan-cn.md)。

## 1. 执行原则与时间线

计划周期: 2026-02-01 到 2026-04-30 共 12 周, 每周 12-15 小时, 共约 144-180 小时。对齐 2026-05 中下旬 Pulse 内推面试节点 (take-home 提交 + on-site)。

3 个 🔴 Core gap 做到 "面试能讲清楚 + working demo", 3 个 🟡 Important gap 做到 "概念清楚 + 200 行示例代码", 不强求 production-grade。

## 2. POC 优先级矩阵

| POC | Gap | 严重度 | 估时 | Entanglement (解锁哪些 gap) | 综合排序 |
|---|---|---|---|---|---|
| POC-01 | Go 语言基础 + 一个生产级小项目 | 🔴 | 18 天 | 所有其他 POC 的语言载体 | 1 |
| POC-02 | gRPC + Protocol Buffers | 🔴 | 8 天 | 复用 POC-01 骨架, 解锁 POC-03/04/05 | 2 |
| POC-03 | Kubernetes + Helm 基础 | 🔴 | 10 天 | 给 POC-02 提供部署载体 | 3 |
| POC-04 | Redis sorted sets + 缓存模式 | 🟡 | 5 天 | 给 POC-02 加缓存层 | 4 |
| POC-05 | 微服务边界 (拆服务) | 🟡 | 5 天 | 把 POC-02 拆两个 service 互调 | 5 |
| POC-06 | OpenTelemetry tracing + Prometheus | 🟡 | 5 天 | 横切所有 POC | 6 |

## 3. 关键 POC 简述

每个 POC 的本质是 **学技能的小项目**, 不是假装的业务项目。无需企业背景, 用 NYC 公开数据集或 Wikipedia corpus 之类做语料。完整版每个 POC 会有 5-10 个 bullet 详细说明。

- **POC-01 Go production**: 用 Go 实现一个 CLI 把 Cedar Ridge SQL 报表项目重写一遍, 全套 production hygiene (test + lint + CI + cobra)。
- **POC-02 gRPC**: 把 POC-01 加上一个 gRPC server, 暴露查询接口, 写一个简单的 Go client 测试。
- **POC-03 K8s + Helm**: Helm chart 部署 POC-02 到 minikube, 学 pod / service / ingress 概念。
- **POC-04 Redis**: 给 POC-02 加 "最近 N 次查询" 缓存层, 用 Redis sorted sets。
- **POC-05 微服务边界**: 把 POC-02 拆成 query-service + index-service 两个 service, 用 gRPC 互调。
- **POC-06 Observability**: 全部 POC 集体加 OpenTelemetry + Prometheus + structured logging (zap)。

## 4. 教程目录索引 (stub)

| POC | 教程文件 (待 qualify-coach 真正生成) |
|---|---|
| POC-01 | `tutorials/01-go-production-hygiene-cn.md` |
| POC-02 | `tutorials/02-grpc-protobuf-cn.md` |
| POC-03 | `tutorials/03-kubernetes-helm-basics-cn.md` |
| POC-04 | `tutorials/04-redis-cache-patterns-cn.md` |
| POC-05 | `tutorials/05-microservice-boundary-design-cn.md` |
| POC-06 | `tutorials/06-otel-prometheus-cn.md` |

`pocs/` 和 `tutorials/` 子文件夹现在是空的, 它们会在 `qualify-coach` 实际跑学习 session 时被填充。

## 5. 12 周时间线骨架

- **Week 1-4 (POC-01)**: Go 语法 + 标准库 + 一个真实 CLI 项目。每周 cadence: 周一三五各 3 小时, 周末整段 5-6 小时。
- **Week 5-6 (POC-02)**: gRPC + Protobuf 集成到 POC-01。
- **Week 7-8 (POC-03)**: minikube 上把 POC-02 部署起来, 学 Helm chart 写法。
- **Week 9 (POC-04 + POC-05 并行)**: 给 POC-02 加 Redis 缓存 + 拆成两个 service。
- **Week 10 (POC-06)**: 横切所有 POC 加 observability 层。
- **Week 11-12 (面试准备)**: 用所有 POC 做 take-home 模拟, mock interview (用 `qualify-mock-interview` skill) 2-3 轮, 暴露薄弱点补课。

## 6. 失败 / 缓解

- **风险**: Go 学习曲线比预期陡 (Go 的 interface / goroutine / channel 习惯跟 Python 差别大), POC-01 可能超时。**缓解**: Week 4 末做一次诚实评估, 如果 POC-01 还没到 "demo-ready", 砍掉 POC-06 整段, 把它的预算挪给 POC-01。
- **风险**: minikube 在 John 的 8GB MacBook 上跑不稳, K8s 实操受阻。**缓解**: 申请 UW CS 学院的免费 GCP credits 用真集群。
- **风险**: 自学没有 reviewer, 写出的 Go 代码非 idiomatic。**缓解**: 每完成一个 POC 把代码扔进 Pulse internship 申请的同步 cover letter 里, 顺带请熟人帮看, 或者用 `qualify-coach` 走一遍代码 review。
