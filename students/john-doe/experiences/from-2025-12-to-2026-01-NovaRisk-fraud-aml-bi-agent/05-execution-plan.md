# 05 — 执行计划

## 一、排序原则

本项目采用 **风险优先 + Audit-deadline 双约束驱动** 的排序逻辑。

**为什么是风险优先**：BI Agent 项目最大的不确定性集中在 **NL → SQL 准确率** 上。Snowflake 上的 20 张表里有跨表业务不变量（冠军路由、分析师-案件同租户、`alert.is_true_positive` ↔ `alert_status` 的严格映射等，详见 ER 文档 §7.10），LLM 很容易在这些边界上出错；如果到 Phase 3 才发现 accuracy 上不去，整个项目就废了。因此 **Phase 1 必须用 5 条 P0 query 端到端打通**，把准确率风险尽早暴露。

**为什么是 Audit-deadline 驱动**：2026-11-30 GA 是硬截止（SOC2 Type II 评估窗口 12-01 起算，FinCEN 现场 2027-Q1）。每个 Phase 的产出都必须给后续 Phase 留足 buffer，不允许 Phase 间叠加压缩。

**不采用其他排序**：
- 不采用"价值优先"（先做 Rachel 最高频的请求）—— 因为价值最高的不一定是技术风险最高的
- 不采用"依赖驱动"（按 architecture 依赖图严格拓扑序）—— 因为最大的依赖瓶颈（Snowflake / KB / CDK）能并行铺开

## 二、整体时间线

| Phase | 时间 | 涉及模块 | 核心目标 | Owner |
|-------|------|----------|----------|-------|
| **Phase 0 — Foundations** | 2026-07-13 → 2026-08-09（4 周）| Snowflake ANALYTICS schema + KB corpus 接入 + CDK skeleton + 8 个核心 metric YAML + Strand Agent 最简版 | 核心 5 人并行 |
| **Phase 1 — E2E Happy Path** | 2026-08-10 → 2026-09-13（5 周）| 5 条 P0 query 端到端打通 + evaluation golden set v1 + Rachel demo | John + Kevin 主力 |
| **Phase 2 — Coverage Expansion** | 2026-09-14 → 2026-10-18（5 周）| 扩到全部 P0（含合规类）+ Visualization Tool + multi-turn + Compliance UAT | 核心 5 人 + David |
| **Phase 3 — Eval & Audit Hardening** | 2026-10-19 → 2026-11-22（5 周）| Audit trail 100% + Eval CI 自动化 + Observability dashboard + SOC2 dry-run | John + Wei 主力 |
| **GA Rollout + Buffer** | 2026-11-23 → 2026-11-30（1 周）| Production cutover + on-call（SOC2 正式走查与一周 staging soak 已前移到 Phase 3 最后工作周；本周含 Thanksgiving，按假期 buffer 处理）| 全员 on-call |

> **里程碑 / demo 日期约定**：各 Phase 按自然周对齐、名义结束于周日（如 Phase 0 → 08-09 周日）；但每个 Phase-exit 的 go/no-go review 与 stakeholder demo 实际在该 Phase 最后一个工作日、即**前一个周五**进行——**M1 = 08-07、M2 = 09-11、M3 = 10-16、M4 = 11-20**，与 Doc 02 §五"stakeholder demo 每两周五"的 cadence 一致。GA sign-off 在 11-30（周一）。下文各 Phase 标题保留自然周区间，里程碑日期以本约定为准。

## 三、各阶段详述

### Phase 0 — Foundations（2026-07-13 → 2026-08-09，4 周）

**入口条件**：
- John Doe 入职完成 onboarding，AWS / Snowflake / GitHub 账号开好
- Maya Chen 把过去一年的 ad-hoc query Notion 整理交付到共享文件夹
- AWS account 配额开好（Bedrock model access、AgentCore Runtime preview enrollment）

**做什么**（并行 4 条线）：
1. **Snowflake ANALYTICS schema**（Marcus + John）：在已有 RAW 之上建 ANALYTICS view 层；建 BI agent 用的 read-only service account + row access policy；写 12 家客户的 client_id 隔离测试用例
2. **Semantic Layer v0**（John + Maya）：把 business context §9 的 8 个核心指标公式写成 YAML（含 `false_positive_rate`、`confirmed_fraud_per_1k_txn`、`decline_rate`、`high_risk_exposure_usd`、`approval_rate`、`ai_drafted_share`、`avg_hours_to_close`、`p99_latency`）；Maya 逐条签字
3. **KB Corpus v0**（John + Kevin）：business context §1–9 切分上传 Bedrock KB；先用 default chunking 跑通端到端；retriever 调参留到 Phase 2
4. **CDK Skeleton + Agent Bootstrap**（Wei + Kevin）：CDK 部署一个空的 AgentCore App + MCP endpoint 到 dev account；Kevin 写一个 echo-tool 的 Strand Agent 跑通 NL → tool → response 链路

**交付物**：
- ANALYTICS schema + row access policy 通过 cross-tenant 测试
- 8 条 metric YAML，全部 Maya 签字
- Bedrock KB 跑通端到端 retrieval（即使 chunking 没调）
- CDK Skeleton 在 dev 部署成功；echo-agent 可被 UI 调用

**为什么这样排**：四条线无相互阻塞，并行铺开能在 4 周内打好整个项目的"地基"；每一条线的产出是 Phase 1 的硬前置。

### Phase 1 — E2E Happy Path（2026-08-10 → 2026-09-13，5 周）

**入口条件**：Phase 0 全部交付物通过 M1 review。

**做什么**：选 **5 条 P0 query** 端到端打通——
- REQ-01（规则误报率，SQL Q5）
- REQ-02（分析师关案时长，SQL Q6）
- REQ-03（超期高优案件，SQL Q18）
- REQ-04（每日交易告警趋势，SQL Q9，**含 chart**）
- REQ-07（每客户决策分布，SQL Q3）

这 5 条覆盖了 Rachel Donovan 80% 的日常请求模式，技术上涵盖：聚合 + 过滤、日期时间序列、窗口函数、Boolean 标志过滤、含可视化的 LEFT JOIN。

同时建立：
- **Evaluation harness v1**（John）：基于 business context §SQL queries 文档实际跑出的数字做 ground truth；每次 PR 自动跑 5 条 query
- **Strand Agent system prompt v1**（Kevin）：明确禁止"凭印象编数字"，强制引用 KB snippet，定义输出格式
- **LLM provider 切换层**（Kevin）：环境变量切换 OpenAI / Gemini / Claude on Bedrock，dev 环境跑 demo provider

**交付物**：
- 5 条 P0 query 在 staging 环境，SQL accuracy ≥ 85% on golden set
- Rachel Donovan 在 **2026-09-11（周五）** demo 上**第一次用 BI Agent 替代邮件请求**
- evaluation report v1 提交 Priya

**为什么这样排**：5 条 query 已经覆盖了技术风险面，能让我们在 5 周内确认 "accuracy 假设" 成立；同时给 Rachel 一个早期 demo 收集反馈，避免 Phase 2 才发现验收口径不对。

**日历**：本 Phase 含 **Labor Day（09-07，周一，US + CA 普遍放假）**，实际工作日 24 天（非 25）；M2 review / demo 安排在 09-11（周五），已避开假日。

### Phase 2 — Coverage Expansion（2026-09-14 → 2026-10-18，5 周）

**入口条件**：M2 review 通过；Rachel demo 反馈纳入 backlog。

**做什么**：
- **扩到全部 P0**（含 REQ-05 制裁名单 SAR、REQ-06 structuring 搜索）—— David Kowalski 在本 Phase 进入 UAT
- **Visualization Tool** 上线：支持柱状 / 折线 / 累计 / 排序前 N 四种 chart type，PNG 经 S3 pre-signed URL 返回
- **Multi-turn 对话**：支持 "再按客户拆一下" / "去掉 Pioneer 这家" 这种 followup
- **Semantic Layer 扩展**：把 P0 涉及的指标全部补齐；新增 dimension（按 fraud_type / decision / case_status 等切分）
- **KB Retriever 调参**：测试 chunk size 512 vs 1024、topK 3 vs 5、metadata filter（doc_type = glossary / metric / section）—— 决策依据：Recall@5 on 业务问题 →（KB snippet 配对）golden set
- **Demo Chat UI v1**（John）：Next.js + Vercel；支持文字 / table / 图片渲染、CSV 下载、错误状态展示
- **PII 脱敏管道**（John + Wei）：任何含 full_name 的检索结果，在喂给 LLM 之前哈希前 6 位

**交付物**：
- P0 全部 7 条 REQ（REQ-01..07）+ 横切 3 条（REQ-19..21）端到端通过 evaluation
- Compliance Manager David Kowalski UAT 通过率 ≥ 80%（5 个 SAR/structuring 类 query 4 个 OK）
- Demo Chat UI 在 staging Vercel 上线

**为什么这样排**：先 P0 全覆盖再考虑 P1，因为 P0 不达标 = 项目失败；UI 在这个 Phase 上线是因为合规 UAT 需要真实交互式 demo，paper prototype 不够。

**日历**：本 Phase 含 **Columbus Day / 加拿大 Thanksgiving（10-12，周一）**，多伦多分部放假；M3 review 安排在 10-16（周五）。

### Phase 3 — Eval & Audit Hardening（2026-10-19 → 2026-11-22，5 周）

**入口条件**：M3 通过；P0 全部稳定。

**做什么**：
- **Audit Trail v1 → v2**（John + Wei）：从 Phase 0 已有的简单日志升级到 FinCEN-grade audit trail——每条记录可索引、可在 5 分钟内根据 metric / 时间窗 / user_id 调出（满足 CEO 的 5 分钟硬约束）
- **Evaluation CI 自动化**（John）：每个 PR 自动跑全套 P0 + P1 evaluation；fail-fast，accuracy 跌破阈值阻断 merge
- **Observability Dashboard**（Wei + John）：CloudWatch dashboard 至少 5 个 widget——tool 调用量、错误率、p95 latency、token 使用、estimated cost；PagerDuty alarm 接入
- **P1 部分实现**：REQ-08 协同攻击团伙、REQ-13 SLA 快照、REQ-10 CDD 评级有效性（共 3 条，剩 3 条 P1 留到 GA 后第一个 sprint）
- **SOC2 Dry-Run + 正式 readiness 走查**（David + Priya）：外聘 SOC2 审计师在本 Phase 中段做 2 天 dry-run 暴露 gap；并在 **Phase 3 最后一个完整工作周（11-16 → 11-20，全为工作日、刻意避开 11-23 那周的 Thanksgiving）** 做正式 control walkthrough，作为 12-01 SOC2 Type II 观察窗口开启前的 readiness 确认；两次问题清单都纳入 GA buffer
- **Production-like staging soak**（Rachel + Maya + David）：在 **Phase 3 最后一周（11-16 → 11-22）** 三位 pilot 用户在 staging 环境连续全量使用一周、目标"零 P0 故障"——这是 GA 的硬前置（见 §四 GA 判定第 2 条），把"用满一周"放在 GA 周之前完成，GA 周不再现攒
- **Production CDK Deploy**（Wei）：把整个 stack 部署到 production AWS account；建 staging → production 切流脚本

**交付物**：
- audit trail 100% 覆盖；CEO 5 分钟硬约束验证通过
- evaluation CI 全自动，阈值由 Priya 锁定（SQL accuracy ≥ 85%, answer accuracy ≥ 80%, citation rate 100%, hallucination ≤ 2%）
- Dashboard + alarm 上线
- SOC2 dry-run 报告，问题数 ≤ 5

**为什么这样排**：这个 Phase 的关键不是新功能，而是把已经能用的东西**变成可审计、可运维、可证伪**的产品级状态。SOC2 dry-run 必须在 GA buffer 之前做，不然 GA 那一周来不及修。**且 GA 周（11-23 → 11-30）正撞 Thanksgiving + Black Friday，实际只有 Mon–Wed 三个工作日，所以一切需要审计师 / stakeholder 在场的活动（正式 readiness 走查、一周 staging soak）都必须在本 Phase 收尾，GA 周只留 production cutover 与 on-call。**

### GA Rollout + Buffer（2026-11-23 → 2026-11-30，1 周）

> **日历提示**：本周含 **Thanksgiving（11-26 周四）+ Black Friday（11-27 周五）**，实际只有 Mon–Wed 三个工作日，且周三是感恩节前短工作日。因此所有需要审计师 / 全员在场的活动（SOC2 正式走查、一周 staging soak）都已在 Phase 3 最后工作周（11-16 → 11-20 / 11-22）完成，GA 周只做 production cutover + on-call，把假期与周末当成 deploy 后的天然 buffer。

- **Mon 11-23**：Production deploy 到 production AWS account + smoke test
- **Tue 11-24**：内部 GA cutover——Rachel + Maya + David 三位 pilot 用户从 staging 切到 production 全量用（staging 端已在 Phase 3 最后一周连续一周无 P0 故障）；随后逐步放量到整个 6 人 Fraud Ops 小队 + David
- **Wed 11-25**（感恩节前短工作日）：change freeze、production 监控、on-call 待命，不排任何审计师 onsite
- **Thu–Fri 11-26 / 11-27**（Thanksgiving + Black Friday）：法定假日，仅 on-call 监控 production，无排期活动
- **周末 11-28 / 11-29**：buffer，应对 Phase 3 SOC2 走查遗留的追问
- **Mon 11-30**：**GA sign-off**——production 自 11-23 起稳定运行、SOC2 走查无 blocker 即正式 GA；若出现 blocker，Priya + Daniel + David 当天决策最多推 1 周到 2026-12-07（fallback，仅在出现 blocker 时启用）

## 四、关键里程碑

| ID | 时间 | 判定标准 | 不通过怎么办 |
|----|------|----------|----------------|
| **M1** | 2026-08-07 (Fri) | (1) CDK skeleton 能在干净 AWS account 一次 deploy 成功；(2) 8 条 metric YAML Maya 全部签字；(3) Bedrock KB 跑通 retrieval 端到端；(4) ANALYTICS schema 通过 cross-tenant 隔离测试 | 任一项不过：Priya 评估根因；若是工时不足，把 Phase 1 中 5 条 query 砍到 3 条；若是技术阻断（如 AgentCore Runtime preview 没批），升级到 Daniel 与 AWS account team |
| **M2** | 2026-09-11 (Fri) | (1) 5 条 P0 query SQL accuracy ≥ 85% on golden set；(2) Rachel demo 反馈"可以代替我现在 3 条邮件请求"；(3) p95 e2e latency ≤ 12 秒（Phase 1 阈值，比最终 8 秒宽）| 若 accuracy < 85%：扩 semantic layer 或加 few-shot；Phase 2 必须解决，否则升级 |
| **M3** | 2026-10-16 (Fri) | (1) P0 全部 REQ（含 REQ-19/20/21）端到端通过；(2) Compliance UAT 通过率 ≥ 80%；(3) Visualization Tool 上线；(4) Demo UI 在 Vercel 可访问 | 若合规 UAT 不过：把不过的 query 单独排查（多半是口径问题，找 Maya）；不影响 M4 但要列出修复时间表 |
| **M4** | 2026-11-20 (Fri) | (1) Eval CI 自动跑全套，阈值通过；(2) audit trail 100% 覆盖 + 5 分钟硬约束验证通过；(3) Dashboard + alarm 上线；(4) SOC2 dry-run + 正式 readiness 走查（11-16 → 11-20）问题数 ≤ 5；(5) Rachel/Maya/David 的 staging soak（11-16 起）已跑满 Mon–Fri 无 P0 故障、并继续到 11-22 | 若 audit trail 漏：升级到 Priya，停 GA 推进直到修复；其他问题进 GA buffer 修 |
| **GA** | 2026-11-30 (Mon) | (1) production deploy 成功；(2) Rachel/Maya/David 三人已在 Phase 3 最后一周（11-16 → 11-22）于 staging 连续全量用一周无 P0 故障（GA 周不再现攒该窗口）；(3) SOC2 正式走查（Phase 3 内完成）无 blocker 级问题 | 若有 blocker：Priya + Daniel + David 当天决策推迟 GA，最多推 1 周到 2026-12-07 |

## 五、John Doe 的工作分配

| Phase | 工作分配 | 与他人协作节点 |
|-------|----------|----------------|
| **Phase 0** | 30% Semantic Layer YAML 起草 + 20% KB Corpus 切分 + 20% Strand Agent learning + 30% ANALYTICS schema 配合 Marcus | 每周两次 Maya 1 小时（口径）；每周 Kevin 2 次 office hour |
| **Phase 1** | 50% Evaluation harness + golden set 数据 + 20% Semantic Layer 扩展 + 20% prompt iteration 配合 Kevin + 10% demo 准备 | M2 demo 必须自己讲 |
| **Phase 2** | 40% Compliance UAT 跟单（口径细化）+ 25% Visualization Tool 配合 Kevin + 20% KB retriever 调参 + 15% Demo UI（Next.js）| Compliance 每周 1 小时 |
| **Phase 3** | 50% Audit trail v2 + Eval CI + 30% SOC2 dry-run 准备材料 + 20% Observability dashboard 配合 Wei | SOC2 审计师走查时全程在场 |

整个项目的工作量大约 **70% 编码 + 20% 与 Maya/David 对齐口径 + 10% 文档**。

## 六、风险与缓解

| 风险 | 概率 | 影响 | 缓解 | 关联里程碑 |
|------|------|------|------|-----------|
| **NL → SQL accuracy 在复杂跨表查询上 < 85%**（如 Q7 协同团伙、Q14 冠军 vs 挑战者、Q16 真正 7 日滚动）| 高 | 高 | Phase 1 早期暴露；缓解：扩 semantic layer 的 join_path 字段、加 few-shot 例子、必要时为最难的 2-3 条 query 上 SQL repair 后处理 | M2 |
| **Audit trail 漏数据点** | 中 | 高（SOC2 / FinCEN 不通过）| Phase 0 就把 logging schema 锁定；fail-the-request 策略；Phase 3 用 100 个 NL 调用做覆盖验证 | M4 |
| **p95 latency > 8 秒** | 中 | 中 | Phase 2 中观测；缓解：Snowflake warehouse 从 XS 升 S、KB retriever 加 in-memory cache、prompt 缩长 | M3 |
| **Rachel UAT 反馈大改（"我其实想要的是 X 不是 Y"）** | 中 | 中 | M2 之前先做 paper prototype 走查（Phase 1 第 2 周）；M2 反馈进 backlog，Phase 2 第 1 周决定接哪些 | M2 |
| **SOC2 审计师追加要求超出预期** | 中 | 中 | M4 dry-run 暴露；GA buffer 应对；最坏推 GA 1 周 | GA |
| **AgentCore Runtime preview / Bedrock model access 批不下来** | 低 | 高 | Phase 0 第 1 周必须确认；不批就 fallback 到 ECS Fargate + 自建 MCP server（Priya + Wei 已有备选方案） | M1 |
| **John Doe 单点故障**（病假 / 离职）| 低 | 高 | Kevin 在 mentor office hour 中保持对 John Doe 工作的并行可见性；所有 PR 必须 Kevin review；关键文档 source-of-truth 在 Notion | 全程 |
| **Agent 生成 SQL 绕过 row access policy，或 PII（full_name / email 原文）泄露进 LLM prompt** | 低 | 高（跨租户数据泄露 / 合规事故）| SfTool 强制 SELECT + 单语句校验；row access policy 在 Snowflake 侧兜底（即便 SQL 越权也读不到他租户行）；PII 脱敏管道在 LLM 入口（保留 hash 前 6 位）；Phase 2/3 用对抗性测试（越权 client_id、注入人名）做红线验证 | M3 / M4 |
| **New Grad 学习曲线**：John Doe 在 Strand / Bedrock / Snowflake 上 ramp-up 偏慢，拖累 Phase 0/1 进度 | 中 | 中 | Phase 0 已预留 20% Strand learning；Kevin 每周 2 次 office hour；必要时把 Phase 1 的 5 条 query 砍到 3 条（与 M1 fallback 联动）| M1 / M2 |
