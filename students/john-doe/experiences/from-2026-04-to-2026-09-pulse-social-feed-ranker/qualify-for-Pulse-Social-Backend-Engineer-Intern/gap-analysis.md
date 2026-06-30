# Gap Analysis for the Pulse Backend Engineer Intern Role

> This is the abbreviated version of the 08 teaching example. In the real workflow, `qualify-execution-plan` produces a roughly 200-line full version covering "why this matters for the role" and "what you can talk about once closed" per gap. Here we keep the trunk so students can see the output shape of Stage 2 in 08. The Cedar Ridge folder has the full 200-line example ([gap-analysis.md](../../../from-2025-06-to-2025-09-CedarRidge-maternity-bi-agent/qualify-for-Cascadia-Health-Insights-AI-Solutions-Engineer/gap-analysis.md)).

## 1. Relevance Diagnosis

Overall relevance: **largely unrelated, closer to a 0 baseline**. John's [resume-old.md](../../../../resume-old.md) has zero distributed systems, Go, microservice, Kubernetes, or production backend experience. Only a web full-stack course project (Capstone Expense Tracker) and the Cedar Ridge SQL reporting internship (a reporting role, not backend engineering).

The JD's minimum bar ("strong proficiency in at least one systems-oriented language" plus "ramp on Go quickly") is reachable through fluent Python plus a short Go ramp. But every "Strongly preferred" item (Go production, gRPC, Kubernetes, Redis, message queue, AWS deployment) is essentially blank.

Conclusion: applying directly gives a low chance of passing the initial screen and a low take-home pass rate. To push success rate to a reasonable level, John needs three months to manually build up a backend mental model plus one end-to-end mini project to use as take-home talking material.

## 2. Gap Breakdown

| Gap | Severity | JD evidence | Current state | What is missing | Closeable in 3 months? |
|---|---|---|---|---|---|
| Production-grade Go | Core | "strong proficiency in at least one systems-oriented language. Go is what we use" | Course-level Python, zero Go | Syntax, standard library, production hygiene all blank | Reachable at interview-credible level, not production-grade |
| gRPC + Protocol Buffers | Core | implicit from JD's microservice mentions | zero exposure | Completely blank | Reachable at demo level |
| Kubernetes + Helm basics | Core | "Strongly preferred" K8s | zero exposure | Completely blank. Cannot distinguish pod, service, and ingress | Reachable at demo level |
| Redis / in-memory databases | Important | "Strongly preferred" Redis | Used PostgreSQL in a course project | Never used Redis in a production path | Reachable |
| Microservice vs monolith architecture | Important | implicit | Only built monolith web apps | Never split a monolith, never designed a service boundary | Concepts plus 1 POC reachable |
| Observability (tracing, metrics, structured logs) | Important | implicit "Strongly preferred" | zero exposure | Completely blank | Reachable at demo level |

## 3. Cross-Gap Entanglement

- POC-01 (Go production) is the language carrier for every other POC. Must come first.
- POC-02 (gRPC) reuses POC-01's code skeleton, so doing it right after is the most efficient.
- POC-03 (K8s) provides the deployment carrier for POC-02. The three pieces can be seen as a single throughline.
- POC-04 (Redis) adds a cache layer to POC-02 and can run in parallel with POC-03.
- POC-05 (microservice boundary) splits POC-02 into two services. A natural extension of POC-02.
- POC-06 (observability) cuts across every POC. Add it concentratedly in the final 1 to 2 weeks.

## 4. Diagnosis Conclusion

Feasible, on the condition that for three months (2026-02 to 2026-05) John puts in at least 12 hours per week of pure focus, with priority on the 3 Core items. After completion, take-home pass rate should reasonably move from an estimated ~10 percent to ~50 to 60 percent.

## 5. Handoff Notes for the Fill Plan

- Prioritize the 3 Core items, closeable to "I can explain it clearly in interview plus a working demo." Production-grade not required.
- The 3 Important items, closeable to "read one official doc plus 200 lines of sample code." Depth not required.
- Time budget 12 weeks x 12 hours = 144 hours, for the fill plan to treat as a hard constraint.
- John has no AWS account budget for production deployment ($0 budget). All K8s practice uses minikube plus student free tier.
