# Feed Ranking Microservice

> Period: 2026-06 to 2026-09. Company: Pulse Social. Role: Software Engineer Intern, Backend Team. Industry: Consumer Social.

## 1. Context

Pulse Social is a 200-person consumer social product with about 8 million monthly active users. The home feed had grown organically inside a Python monolith, with the ranking logic tangled together with the post-fetch logic and the moderation logic. Latency spikes were common at peak, p99 sat around 220ms, and the team could not roll out a new ranking model without a multi-day deploy.

The Backend Tech Lead wanted a clean ranking microservice extracted from the monolith. The plan was to write it in Go, separate candidate generation from ranking from post-processing into composable stages, expose a gRPC API, and ship it behind a percentage rollout so the team could compare it against the monolith path live.

I joined as an intern on the Backend Team and was given the rewrite as my primary project. I reported to the Tech Lead and pair-worked with two senior backend engineers on architecture decisions.

---

## 2. What I Did

I built the service in Go, structured into three composable stages.

The Candidate Generator stage pulls posts from three sources: the user's follow graph, a topic-affinity index, and a small set of editorially curated items. Each source is its own implementation of a common interface, so adding a new source is a one-file change.

The Ranker stage scores candidates with a model loaded from a model registry. I kept the model interface minimal (a vector of scored items in, a vector of scored items out) so the data-science team could swap in new models without coordinating a deploy with backend. They shipped two new models during my 12 weeks using this path.

The Post-Processor stage handles deduplication, blocklist filtering, diversity policies, and the recently-seen filter (backed by Redis sorted sets).

For the API, I designed a gRPC contract with three RPCs: GetFeed, RecordImpression, and HealthCheck. Three client services (the mobile API gateway, a recommendation-test harness, and an internal admin tool) integrated against it.

For deployment, I built a Kubernetes Helm chart with HPA on CPU and request-per-second, structured JSON logging via zap, Prometheus metrics, and OpenTelemetry tracing wired through every stage so we could see which stage caused a latency spike. The service ran in two regions behind a regional load balancer.

For correctness and load characteristics, I wrote unit and integration tests reaching 84% line coverage, and a k6 load-test suite that replayed two weeks of production traffic at 1.5x peak. I shadow-deployed the service against the monolith for two weeks before any user traffic, comparing returned feed deltas via an offline diff job.

---

## 3. Outcomes

Shipped behind a 25% rollout in 8 weeks. Outcomes measured during the rollout window:

- Served 4,500 requests per second at peak across the two regions.
- p99 latency under 60ms end to end (the monolith path it replaced sat at 220ms).
- +6% session length lift in the A/B test, statistically significant at p<0.01, primarily driven by the diversity post-processor catching repeated topics that the monolith path missed.
- Two new ranking models shipped by the data-science team through the model interface without backend involvement.
- Mean time to recovery on incidents involving the feed reduced by roughly 40%, attributed to the per-stage tracing surfacing root cause faster.

The Tech Lead asked me to return next summer to lead the rollout to 100%.

---

## 4. Tech Stack

Go, gRPC, Protocol Buffers, Redis, Apache Kafka, PostgreSQL, Kubernetes (with Helm), Docker, AWS (EKS, ElastiCache, RDS, ALB), Prometheus, Grafana, OpenTelemetry, zap (logging), k6 (load testing), GitHub Actions.
