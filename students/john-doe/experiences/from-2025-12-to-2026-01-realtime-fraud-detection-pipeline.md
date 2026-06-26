# Real-Time Fraud Detection Pipeline

> Period: 2025-12 to 2026-01. Company: Forge Trading. Role: Data Analytics and ML Contractor. Industry: Financial Services.

## 1. Context

Forge Trading is a mid-size derivatives broker. (A derivatives broker is a financial firm that helps clients buy and sell contracts whose value is tied to something else, like a stock or commodity price. Because money moves quickly and in large amounts in this business, fraud detection is a real and constant concern.)

The compliance team is the in-house group responsible for making sure trades follow regulations and that the firm is not being used for financial crime. When I joined, they were running a legacy rules engine for transaction fraud detection. That rules engine was a stack of roughly 80 hand-coded if-else rules, written over the years by various engineers and analysts.

Every day, the engine flagged about 0.8% of all transactions for human review. Of those flags, around 78% turned out to be false positives. (The trade looked suspicious to the rules engine, but a human analyst later confirmed it was perfectly fine.) The compliance analysts were drowning in clearance work on benign trades.

The team wanted to pilot an ML-based system to supplement, not replace, the rules engine. (A machine-learning model learns fraud patterns from historical data rather than from hand-written rules. The hope was that the model could catch the same real fraud while raising far fewer false alarms.)

The Head of Data brought me on for a six-week winter contract to lead the analytics and modeling work. I worked alongside two senior data engineers (who owned the streaming infrastructure) and a quantitative analyst (a math-and-stats specialist who designs trading signals and risk models).

The scope was deliberately narrow. Understand why the rules engine was firing so often. Train a baseline ML model. Evaluate it in shadow mode (running it in parallel with the live rules engine but not yet letting it affect real decisions). Produce a written recommendation on whether the approach was worth scaling.

---

## 2. What I Did

When I joined, no one on the team could clearly explain why the legacy rules engine kept flagging so many benign transactions. People had hunches, but no one had actually sat down with the data.

My first move was to pull six months of compliance flag history into Snowflake. (Snowflake is a cloud data warehouse, basically a giant SQL database designed for analytics rather than for handling live transactions.)

I wrote a series of SQL queries grouping the flags by merchant category, geography, time of day, and the analyst's final clearance decision. Within a couple of days the picture started to clear up.

The pattern that came out was sharp. Roughly 80% of analyst clearance hours were being spent on just three situations.

The first was cross-border travel transactions flagged on weekends. The second was holiday-season retail spending spikes. The third was a small set of merchants whose category codes had been misclassified upstream by a third-party data feed.

I turned the analysis into a one-page memo for the Head of Compliance, with charts showing each pattern and a suggested target list of rules to retire. That memo became the team's shared map of the problem and shaped the rest of the project.

Around the same time, I noticed the compliance team was making decisions off a weekly Excel spreadsheet that took an internal analyst nearly two days to assemble. By the time it landed in their inbox it was already a few days stale. I offered to replace it with a live dashboard.

I built that dashboard in Looker. (Looker is a business-intelligence tool, similar to Tableau, that lets non-technical teammates explore data with clicks and filters instead of writing SQL.) The dashboard tracked daily flag volume, false-positive rate, and analyst clearance time, broken down by the three patterns I had identified in the memo. It refreshed automatically every hour.

By the end of my engagement the compliance team was opening it during their morning standup, and the weekly Excel spreadsheet was quietly retired.

The streaming side of the pipeline was largely owned by the two senior data engineers. To give the context: every transaction event from Forge's internal order-management system was streamed through Apache Kafka on AWS MSK into a Snowflake table, at a peak rate of about 12,000 events per second. (Kafka is a high-throughput message broker that moves events between systems in real time.)

A Spark Structured Streaming job then joined each event against six rolling feature aggregates (transaction velocity over 1, 5, and 30 minutes, average notional, geo-anomaly score, and merchant-risk score) and scored the result with an ML model. I owned the analytics and modeling layer that sat on top of that stream.

For the model itself, I worked with the quantitative analyst to engineer roughly 30 features from the transaction history. Some of those features were straightforward counts and averages. Others were more nuanced, like a geo-anomaly score that measured how far a transaction's location was from the customer's usual pattern.

I trained an XGBoost model on six months of labeled historical transactions, about 4 million records. (XGBoost is a popular machine-learning algorithm that works well on tabular business data. It's the go-to tool for problems like predicting whether a transaction is fraud, whether a customer will churn, or whether a loan will default.)

I tuned the model with Bayesian search via Optuna. (Optuna is a library that intelligently searches for the best model settings instead of trying every combination by brute force.) The chosen model reached 0.94 AUC on a held-out month of data. (AUC is a score between 0 and 1 that measures how well a model separates the two classes. 0.5 is random guessing and 1.0 is perfect.)

Before the Head of Compliance would commit to switching to active scoring, she asked for a rigorous before-and-after comparison. I designed a retrospective study to answer her question. (A retrospective study re-runs both systems on historical data and compares the outcomes side by side, while controlling for confounding factors like time of day and merchant mix.)

I scored both the ML model and the rules engine on the same 4 million historical transactions. Then I computed the false-positive reduction with a 95% confidence interval using Python. (I used statsmodels for the confidence intervals, with pandas and scipy doing the data wrangling.)

I visualized the result as a paired bar chart with error bars, so the comparison was readable at a glance even for a non-technical executive.

I presented the study to the executive review committee. The headline number, a 35% false-positive reduction with a tight confidence interval, was the main reason the committee green-lit the model for active scoring.

To monitor drift once the model went live, I wired up PSI tracking on the top ten features. (PSI stands for population stability index, a statistical measure of how much a feature's distribution in live data has drifted away from the training data. A rising PSI warns you that the model might be looking at a different world than it was trained for.)

We had three alert events during the shadow run. In two of them, the analyst confirmed real upstream covariate shift.

We retrained ahead of any actual degradation in production, which was exactly the kind of catch the PSI tracking was supposed to enable.

The last piece of analytics work was for the compliance analysts themselves. These are the people who manually clear flagged transactions. They are deep domain experts but not coders. I wanted to give them something that respected their judgment but armed them with sharper signals.

I wrote a set of SQL exploration notebooks for them. (A notebook is a digital lab notebook where you mix queries, charts, and explanatory notes in one document.) The notebooks surfaced which transaction patterns correlated with confirmed fraud versus confirmed benign in the historical record.

One finding in particular caught their attention. When a geo-anomaly score crossed a certain threshold at the same time that a merchant-risk score crossed another, the combination was 12 times more likely to be real fraud than either signal seen on its own. The compliance team incorporated those findings into their manual review playbook so analysts knew which signals to focus on first.

---

## 3. Outcomes

The system ran in shadow mode against the live production transaction stream for two weeks. The final write-up to the Head of Compliance covered both the modeling outcomes and the analytics outcomes.

On the analytics side, the false-positive pattern memo gave the team a concrete target list of legacy rules to retire, addressing roughly 80% of wasted clearance hours.

The Looker dashboard was adopted as the daily standup reference for the compliance team within four weeks, and the slow Excel spreadsheet was retired.

The retrospective study reached the executive review committee and was cited as the deciding factor in approving the move to active scoring. The analyst playbook update, driven by the SQL exploration notebooks, gave the manual reviewers a clear ranking of which signal combinations to investigate first.

On the modeling and pipeline side, the system processed 12,000 events per second at peak, where the prior pipeline maxed out around 5,000. The model achieved 0.94 AUC on holdout. Recall at the chosen operating point matched the rules engine, meaning no fraud was missed in shadow, while the false-positive flag rate dropped by 35%.

Over the two-week shadow window, the model caught $1.2M in confirmed fraudulent transactions that the rules engine had missed. The team approved scaling to active scoring shortly after I left.

---

## 4. Tech Stack

Python (pandas, scipy, statsmodels, scikit-learn), SQL, XGBoost, Optuna, Snowflake, Looker, Jupyter, Apache Kafka (AWS MSK), Apache Spark Structured Streaming, dbt, Redis, Apache Airflow, Great Expectations, AWS (MSK, EKS, S3, IAM), Docker, Prometheus, Grafana.
