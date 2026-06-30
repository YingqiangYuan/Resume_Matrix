# Feed Ranking Microservice

> Timeline: June 2026 to September 2026. Company: Pulse Social. Role: Software Engineering Intern, Backend team. Industry: consumer social.

## 1. Background

Pulse Social is a 200-person consumer social product with roughly 8 million monthly active users. Its main battleground is the Home Feed, the stream of posts you see scrolling the moment you open the app.

The order of those posts is not random. A program scores and ranks every post that could be shown to you and hands the top results back to the phone. That program is the Feed Ranker. The quality of its decisions is basically the source of either "this app feels great" or "this app keeps getting worse" in user reviews.

At Pulse, this Feed Ranker had grown organically inside a Python monolith. A monolith is a large application where all features live in one codebase and have to ship together as a single unit. After several years, Pulse's ranking logic, its post-fetching logic, and its content moderation logic were all tangled together. Buried in there were also a few half-dead experiments that nobody wanted to delete because nobody remembered who owned them.

The symptoms were predictable. Latency spikes at peak hours were common, with p99 hovering around 220ms. The p99 latency number describes the response time of the slowest 1 percent of requests. Engineers care about it the most because those slow requests are exactly the ones users feel and then complain about.

The release process was just as bad. Shipping a new ranking model often took several days. The whole monolith had to be redeployed, a test suite of hundreds of checks unrelated to ranking had to run, and someone had to babysit the rollout. The data science team had a backlog of model ideas they could not ship because this release tax was too high.

The backend Tech Lead wanted to pull the ranking piece out of the monolith and turn it into a clean microservice. A microservice is a small independently deployed service that does one thing. It is the modern alternative to the older "one big app does everything" style.

The plan itself was straightforward. Write it in Go for predictable performance and a small memory footprint. Split the work into composable stages (candidate generation, ranking, post-processing) so each stage could be reasoned about and tested in isolation. Expose a gRPC API. Roll it out by percentage gradually, compare with the monolith path in production, and keep the option to roll back instantly.

I joined the backend team as an intern and this rewrite became my main project. I reported to the Tech Lead and paired with two senior backend engineers on architecture decisions.

That positioning matters. This rewrite was not a side experiment. It was the team's main bet for the quarter, watched by leadership, with a real shutdown deadline already set for the old monolith path. The cost of having an intern own this is that I got trust well beyond what is normal for the level, and I also carried responsibility well beyond what is normal for the level.

---

## 2. What I Built

The service is written in Go and split into three composable stages. Every Feed request walks through them in order.

The first stage is the Candidate Generator. Before you can score and rank posts, you need to decide which posts are eligible to be shown to a given user. That is candidate generation. Most of the "what do I even have to show this person" logic lives here.

My implementation pulls candidates from three sources: the user's follow graph (who this person follows), a topic affinity index (topics this person has interacted with frequently), and a small set of operations-curated picks (items the content team wants to amplify). Each source runs in parallel and returns up to a few hundred candidates, which are then merged, deduplicated, and handed to the ranker.

Each source is an implementation of the same Go interface. This sounds like a small detail, but the effect is large. Adding a new source becomes a "change one file and you are done" task, no full team Code Review needed. Later, the content team shipped a "trending in your city" source on their own with only one engineer needed for Review.

The second stage is the Ranker. It loads a model from an internal Model Registry and uses that model to score each candidate. I deliberately kept the model interface very small: take a list of candidates, return a list of scored candidates. The reasoning behind this is both technical and organizational. I did not want the data science team to have to coordinate a backend release every time they swapped a model.

During my 12 weeks they shipped two new models through this path with zero backend involvement. That is the biggest workflow win of the project. The corresponding bullet on my resume reads "refactored the model release path so swapping models no longer touches the backend." It sounds lighter than the rewrite itself, but my mentors said the team felt "unshackled." That is what they were referring to.

The third stage is the Post-Processor. It handles deduplication, blocklist filtering, diversity (avoiding five posts on the same topic in a row), and a "recently seen" filter (pushing down posts the user already scrolled past in the last few hours). The "recently seen" filter is implemented with a Redis Sorted Set so I can do a "read and update one user's history" operation in about 1 millisecond. We had actually collected a category of user complaint shaped like "didn't I just see this one?" The diversity logic and the "recently seen" filter were aimed exactly at that.

For the API I designed a gRPC contract. gRPC is a way for two services running on different machines to call each other's functions over the network. It is faster and more strictly typed than the older REST and JSON style most websites use, which matters when an internal backend has to serve several thousand requests per second.

The contract defines three RPCs: GetFeed (called every time a user opens or refreshes the Feed), RecordImpression (called as the user scrolls past each post, so the system knows what was actually seen), and HealthCheck (called by the Load Balancer to decide whether a given service instance is still alive). Three internal client services hooked into this interface: the Mobile API Gateway (the service the phone app talks to directly), a recommendation testing Harness for data science, and an internal Admin tool (used by content operations to investigate user reports of "why am I seeing this post").

For deployment I wrote a Kubernetes Helm Chart. Kubernetes is the industry-standard system for "automatically run many copies of many services across many machines." If a machine dies, Kubernetes will start a replacement somewhere else with no human in the loop. A Helm Chart is a packaged "recipe" that tells Kubernetes exactly how to run the service.

I attached the HPA (horizontal pod autoscaler) to both CPU and QPS so the cluster could scale with real traffic. I used zap for structured JSON logging and exported Prometheus metrics following the industry-standard Golden Signals (request rate, error rate, latency distribution, saturation).

The piece I cared about the most was OpenTelemetry tracing wired through every stage. The moment latency went up, we could open one Dashboard and see immediately whether candidate generation, the model call, or post-processing was the culprit. In the monolith, an incident usually started with "Feed got slow," and someone would spend hours grepping through millions of log lines to find the cause. With per-stage traces, the same diagnosis took minutes. The service runs in two AWS regions behind a regional Load Balancer.

For correctness and load, I wrote unit and integration tests up to 84 percent line coverage, plus a k6 load test suite that replays two weeks of recorded production traffic amplified to 1.5x peak. k6 is a load testing tool where you describe traffic in code and dial up the intensity. We did not pick 1.5x at random. The thinking was that if the new service can handle that, we have built in headroom for the next year of organic growth.

Before any real user traffic touched the new service, I ran a two-week Shadow deployment. A Shadow deployment means the new service and the old service run in parallel. Real production traffic is sent to the new service too, but the new service's response is thrown away. Users still get the old service's response. That lets you test latency, error rate, and output differences against real traffic without affecting any user. An offline Diff job compared the Feeds returned by both sides and flagged cases where the difference was big enough to be worth human review.

Week one of Shadow caught a real bug. The new ranker returned an empty Feed for roughly 0.2 percent of users. Those turned out to be brand-new accounts with no follows and no topic history. The candidate generator had nothing to pull, the ranker had nothing to score, and the result was an empty list. I added an "operations cold-start" fallback to the Candidate Generator: if the other two sources return fewer items than a threshold, top up from the operations pool. The bug never reached a real user.

Week two of Shadow surfaced something more subtle. The model call occasionally exceeded 200ms because the feature fetch for some candidate sets hit a slow path. I added a per-request budget cap plus a parallelized feature fetch step, and the long tail came back into the normal range. Neither problem was reproducible in Staging with synthetic data. They only surfaced with the shape of real production traffic, which is exactly what Shadow deployment is for.

After Shadow came a careful percentage rollout: 1 percent, 5 percent, 25 percent. At each step we let things sit, watched dashboards for several days, and confirmed latency, error rate, and business metrics were clean before pushing to the next step. The Tech Lead and I agreed up front: no matter how good the numbers look, we never push two steps in a single day. The worst incidents usually surface a day or two after a change, once traffic shape shifts.

---

## 3. Outcomes

The service reached 25 percent rollout in 8 weeks. Numbers measured during rollout:

The service peaked at 4,500 QPS (queries per second) across both regions combined. In plain terms: at the busiest hour of the day, about 4,500 users per second were loading the Feed, and the autoscaler held up.

For comparison, the monolith path it replaced was already near its scaling ceiling at the same traffic. The new service still had about 50 percent headroom before HPA would need another tune.

End-to-end p99 latency held under 60ms. The monolith path it replaced sat at 220ms. So for the slowest 1 percent of requests (the ones users actually feel as lag), the new path was nearly four times faster.

Session length in A/B testing improved by 6 percent, significant at p<0.01. The A/B test on Feed works like this: half the users get the new ranker, half stay on the old one. Then we look at whether the new-ranker group on average spends more time in the app.

The +6 percent means the new-ranker group stayed about 6 percent longer per session on average. The p<0.01 means the chance this result is pure coincidence is very low (less than 1 percent chance of being a false positive from random sampling). The main driver was the diversity post-processing catching repeat-topic issues the monolith missed. Internally at Pulse, +6 percent session length on Home Feed is a number nobody had moved in over a year. It ended up being the headline metric in every external-facing readout of the project.

The data science team shipped two new ranking models through the model interface without backend involvement, validating the original "keep the model interface as thin as possible" design decision.

Before the rewrite, swapping a model required about a week of backend engineering work for the integration. After the rewrite, the data science team can do it themselves in a few hours. That single change rewrote the cadence at which the entire company can experiment on Feed quality.

MTTR (mean time to recovery) on Feed incidents dropped by about 40 percent. The team attributed this to per-stage tracing making root cause identification much faster than digging through the monolith. The shorter recovery time means fewer users see a degraded Feed during incidents, which translates directly into fewer support tickets.

The Tech Lead invited me back next summer to push the rollout from 25 percent to 100 percent. Beyond rollout itself, next summer's plan is to apply the same "staged pipeline" template to pull two more pieces out of the monolith, so this rewrite ends up as a pattern other engineers can copy rather than a one-off effort.

---

## 4. Tech Stack

Go, gRPC, Protocol Buffers, Redis, Apache Kafka, PostgreSQL, Kubernetes (via Helm), Docker, AWS (EKS, ElastiCache, RDS, ALB), Prometheus, Grafana, OpenTelemetry, zap (logging), k6 (load testing), GitHub Actions.
