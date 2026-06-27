# Feed Ranking Microservice

> Period: 2026-06 to 2026-09. Company: Pulse Social. Role: Software Engineer Intern, Backend Team. Industry: Consumer Social.

## 1. Context

Pulse Social is a 200-person consumer social product with about 8 million monthly active users. The flagship surface is the home feed, the scrolling list of posts you see the moment you open the app.

The order of those posts is not random. A piece of software scores every post that could possibly be shown to you, sorts them, and hands the top results back to the mobile app. That piece of software is called the feed ranker. The quality of its decisions is roughly what people mean when they say a social app "feels good" or "feels stale".

At Pulse, the feed ranker had grown organically inside a Python monolith. A monolith is a single large application where every feature lives in the same codebase and ships together as one unit. Over the years, the ranking logic at Pulse had tangled itself together with the code that fetched posts from the database, the code that did content moderation, and a few half-dead experiments that nobody wanted to delete because nobody remembered who owned them.

The symptoms were predictable. Latency spikes were common at peak hours. p99 latency sat around 220ms. p99 latency means the response time experienced by the slowest 1% of requests, which is the number engineers watch most closely because those slow requests are the ones users actually notice and complain about.

The deploy story was just as bad. Shipping a new ranking model usually meant a multi-day release because the team had to redeploy the entire monolith, run a long test suite that included a hundred things unrelated to ranking, and then babysit the rollout. The data-science team had a backlog of model ideas that they could not afford to ship because the deploy tax was too high.

The Backend Tech Lead wanted a clean ranking microservice extracted from the monolith. A microservice is a small, independently deployable service that does one job well. It is the modern alternative to the giant single-application style.

The pitch was straightforward. Write it in Go for predictable performance and a small memory footprint. Split the work into composable stages (candidate generation, ranking, post-processing) so each stage could be reasoned about and tested independently. Expose a gRPC API. Ship it behind a percentage rollout so the team could compare it against the old monolith path on live traffic, and dial it back instantly if anything looked wrong.

I joined as an intern on the Backend Team and was given the rewrite as my primary project. I reported to the Tech Lead and pair-worked with two senior backend engineers on architecture decisions.

That framing matters. The rewrite was not a side experiment. It was the team's main bet for the quarter, with leadership visibility and a real deprecation deadline on the old monolith path. The trade-off as the intern owning it was that I got unusually high trust and unusually high accountability for someone in that role.

---

## 2. What I Did

I built the service in Go, structured into three composable stages that ran in sequence for every feed request.

The first stage was the Candidate Generator. Before you can rank posts, you have to pick which pool of posts is even eligible to be shown to a given user. That step is called candidate generation, and it is where most of the "what could I possibly show this person?" logic lives.

My implementation pulled candidates from three sources: the user's follow graph (everyone they follow), a topic-affinity index (topics the user has engaged with in the past), and a small set of editorially curated items the content team wanted boosted. Each source ran in parallel and returned at most a few hundred candidates, which were then merged and de-duplicated before being handed to the ranker.

Each source was its own implementation of a common Go interface. That sounds like a small detail, but the practical effect was big. Adding a new candidate source became a one-file change rather than a code review across the whole team, and the content team eventually shipped a "trending in your city" source on their own with one engineer reviewing it.

The second stage was the Ranker. It loaded a model from an internal model registry and used that model to give every candidate a score. I deliberately kept the model interface as minimal as possible: a vector of items in, a vector of scored items out. The reason was political as much as technical. I did not want the data-science team to have to coordinate a backend release every time they wanted to try a new model.

During my 12 weeks they shipped two new models through this path with zero backend involvement, which was the single biggest workflow win of the project. The bullet on my resume is "rebuilt the model-deploy path so a model swap stopped touching backend at all", and it sounds smaller than the rewrite itself, but it is the change my mentors pointed at when they said the team felt unblocked.

The third stage was the Post-Processor. It handled deduplication, blocklist filtering, diversity policies (preventing five posts about the same topic from showing up in a row), and a "recently seen" filter that suppressed posts the user had already scrolled past in the last few hours. The recently-seen filter was backed by Redis sorted sets, which let me check and update per-user history in roughly a millisecond. We had measured a real user complaint pattern around "I just saw this post" and the diversity plus recently-seen pair was aimed directly at it.

For the API, I designed a gRPC contract. gRPC is a way for two services on different machines to call each other's functions over the network. It is faster and more strictly typed than the older REST/JSON style most websites use, which matters when you are inside a backend that has to serve thousands of requests per second.

The contract had three RPCs: GetFeed (called every time a user opens or refreshes the feed), RecordImpression (called as the user scrolls past each post, so the system learns what they actually saw), and HealthCheck (called by the load balancer to know whether a given instance of the service is still alive). Three internal client services integrated against it: the mobile API gateway (the service the phone app talks to), a recommendation-test harness used by data science, and an internal admin tool used by content ops to debug "why am I seeing this post" reports from users.

For deployment I built a Kubernetes Helm chart. Kubernetes is the industry-standard system for running many copies of a service across many machines automatically. If one machine dies, Kubernetes spins up a replacement somewhere else without a human in the loop. A Helm chart is a packaged recipe that tells Kubernetes exactly how to run a given service.

I wired up horizontal pod autoscaling on both CPU and requests-per-second so the cluster would grow and shrink with real traffic, added structured JSON logging via zap, and exported Prometheus metrics for the standard golden signals (request rate, error rate, latency distribution, saturation).

The piece I cared about most was OpenTelemetry tracing running through every stage. When latency went up, we could open one dashboard and see immediately whether candidate generation, the model call, or post-processing was responsible. In the monolith, an incident usually started with "the feed is slow" and ended hours later with someone grep-ing through a million log lines. With per-stage traces, the same diagnosis took minutes. The service ran in two AWS regions behind a regional load balancer.

For correctness and load characteristics I wrote unit and integration tests reaching 84% line coverage, and a k6 load-test suite that replayed two weeks of recorded production traffic at 1.5x peak volume. k6 is a load-testing tool that lets you describe traffic in code and ramp it up to whatever volume you want. The 1.5x number was not a guess. We picked it so that if the new service held up under load tests, we had headroom for a year of organic growth.

Before any real user traffic hit the new service, I ran it in shadow deployment for two weeks. Shadow deployment means running the new service in parallel with the old one, sending it real production traffic, but throwing its responses away and using the old service's response for the actual user. That lets you measure the new service against reality (latency, error rate, output diff) without putting users at risk. An offline diff job compared the feeds the two paths would have returned and flagged the cases where they disagreed badly enough to investigate.

Shadow week one caught a real bug. The new ranker was returning empty feeds for roughly 0.2% of users, all of whom turned out to be brand new accounts with no follows and no topic history. Candidate generation produced zero items, the ranker had nothing to score, and we returned an empty list. I added an "editorial cold start" fallback to the Candidate Generator. If the other two sources returned fewer than a threshold number of items, we filled the rest from the editorial pool. The bug never reached a real user.

Shadow week two surfaced a quieter problem. The model call was occasionally taking longer than 200ms because of a slow path in how features were being fetched for the candidate set. I added a per-request budget and a parallel feature-fetch step, which brought the tail back under control. Neither of these issues would have been catchable in a staging environment with synthetic data. Both came out of shadow because shadow uses real production traffic shapes.

After shadow, we ran a careful percentage rollout: 1%, then 5%, then 25%. At each step we held the rollout flat for a few days, watched the dashboards, and only moved forward when latency, error rate, and the business metrics all looked clean. The Tech Lead and I agreed up front that we would never advance two stages in one day, no matter how good things looked, because the worst incidents tend to surface a day or two after a change as the traffic mix shifts.

---

## 3. Outcomes

The service shipped behind a 25% rollout in 8 weeks. Outcomes measured during the rollout window:

The service served 4,500 requests per second at peak across the two regions. In plain terms, that is roughly 4,500 user feed-load requests every second at the busiest hour of the day, sustained without the autoscaler running out of headroom.

For comparison, the monolith path it replaced was already running close to its scale ceiling at the same traffic level. The new service had room to absorb roughly another 50% before HPA would need to be retuned.

End-to-end p99 latency stayed under 60ms, down from 220ms on the monolith path it replaced. So for the slowest 1% of feed requests (the ones users actually feel as a stutter), the new path was almost four times faster.

The A/B test showed a +6% session length lift, statistically significant at p<0.01. An A/B test on a feed works like this: half the users get the new ranker, half stay on the old one, and you measure whether the new-ranker group spends more time in the app on average.

A +6% lift means new-ranker users averaged 6% more time per session, and p<0.01 means that result is very unlikely to be a coincidence (less than a 1% chance of seeing a gap this large by random luck). The diversity post-processor catching repeated topics that the monolith missed was the main driver. Internally, +6% session length on the home feed was a number nobody had moved in over a year, and it became the headline metric from the project.

Two new ranking models shipped by the data-science team through the model interface without backend involvement, which validated the original "minimal model interface" design call.

Before the rewrite, a model swap took a backend engineer roughly a week of integration work. After, it took the data-science team a few hours. That alone changed the rhythm of how the company experimented with feed quality.

Mean time to recovery on incidents involving the feed dropped by roughly 40%, which the team attributed to the per-stage tracing surfacing the root cause faster than the monolith ever could. The shorter that recovery window is, the fewer users see a degraded feed during an incident, so this number translates directly into fewer complaints to support.

The Tech Lead asked me to return next summer to lead the rollout from 25% to 100%. Beyond the rollout itself, the plan for next summer is to migrate two more pieces out of the monolith using the same staged-pipeline template, so the rewrite ends up being a pattern other engineers can copy rather than a one-off effort.

---

## 4. Tech Stack

Go, gRPC, Protocol Buffers, Redis, Apache Kafka, PostgreSQL, Kubernetes (with Helm), Docker, AWS (EKS, ElastiCache, RDS, ALB), Prometheus, Grafana, OpenTelemetry, zap (logging), k6 (load testing), GitHub Actions.
