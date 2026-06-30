# POC-01, Building a Wikipedia QA Agent with Strand Agents

> This is the first of 9 POCs in the gap-fill plan. It maps to the gap: Core LLM agent framework (Strand Agents). Companion tutorial: [../../tutorials/01-strand-agents-quickstart.md](../../tutorials/01-strand-agents-quickstart.md).

## 1. What This POC Practices

This POC drills the four core moves of the Strand Agents framework. Install the package and get a hello agent running, define a first tool the LLM can call, give the agent memory so it can recall earlier turns of a conversation, and wrap the whole thing as a CLI program runnable from a terminal. Those four moves form the minimum skeleton behind every agent inside the Cascadia Insight Assistant. Once you own that skeleton, the rest of the complexity is just layered tools and layered prompts on top.

No business context is needed, and no hospital data is involved. The Wikipedia corpus exists so evaluation has a clean ground truth, which makes it easy to judge whether the agent is answering correctly.

## 2. Inputs and Starting Point

- A small corpus of 10 Wikipedia articles (pick 10 technical topics yourself, for example LLM, Vector Database, Snowflake, RAG, FHIR). Download them and store as `corpus/*.md`.
- A 50 question golden set of Q-A pairs (handwritten by you). Each question maps to a clear answer that can be found in the corpus.
- Python 3.12 environment, virtualenv managed with `uv`. LLM provider is the OpenAI API (the cheapest option for the demo stage).

## 3. Expected Deliverables

- An agent runnable from the CLI. Type `python -m poc01 ask "What is RAG?"` and get back a text answer plus the names of the cited Wikipedia articles.
- A `RUN.md` recording the hit count against the 50 question golden set (target, 42 / 50 = 84% accuracy or higher).
- A code walkthrough (about one page) explaining how the agent loop, tool definition, and memory implementation each work. This walkthrough doubles as a client-facing writing sample for POC-08.

## 4. Acceptance Criteria

- The agent correctly handles at least one context-dependent follow-up (something like "Tell me more about that"), proving the memory module is live.
- At least 80% of responses carry a citation (the cited article name), proving the RAG retrieval loop is healthy.
- The evaluation script finishes 50 questions inside 60 seconds, with p95 latency under 6 seconds per question.
- Code passes `ruff check` and `mypy` with zero errors, proving you also pulled the POC-09 production hygiene into this work.

## 5. How It Maps to the Cascadia JD

The Cascadia JD says "Strongly preferred, hands-on experience with at least one LLM application framework. Strand Agents, LangChain, LlamaIndex, Haystack, or comparable. We use Strand Agents in production." Once this POC is done, John can say in an interview "I built a RAG agent on Strand that hits 84% accuracy on a 50 question golden set, and I can walk you through the tradeoffs I made around Strand's tool registration mechanism, memory backend choice, and retry strategy inside the agent loop." That is the level of detail a Hiring Manager will keep digging into.

## 6. Time Estimate

- Day 1 to 2, install the package, get a hello agent running, read the Strand official quickstart and the 4 example programs in the source tree.
- Day 3 to 5, define the first tool (Wikipedia retrieval) and get single-turn QA running.
- Day 6 to 8, add memory (use Strand's built-in conversation memory) and get multi-turn context working.
- Day 9 to 10, wrap the CLI, write the evaluation script, run the golden set, and tune the prompt to push hit rate from the initial 60 to 70% up to 84% or higher.
- Day 11 to 12, write the walkthrough document, clean up the code, and get past ruff and mypy.

Total of 12 days at 1.5 hours per day, 18 hours in all.

## 7. Connections to Other POCs

- The corpus and Q-A golden set from this POC get reused by POC-03 (RAG plus eval harness). POC-03 layers stricter retrieval metrics and more sophisticated evaluation on top, so the output of POC-01 is the input to POC-03.
- The agent backbone from this POC gets reused by POC-02 (Bedrock AgentCore Runtime). POC-02 deploys the same agent onto AgentCore Runtime, proving an agent that runs locally can also ship to the cloud.
- The walkthrough written at the end of this POC is the first writing sample for POC-08 (client-facing writing).

## 8. Known Pitfalls (heads-up in advance)

- Strand's OpenAI provider has occasional bugs around streaming token counts in the 0.x versions. Your evaluation script should not rely on the token usage field for cost estimation. Use wall-clock time instead.
- Wikipedia articles contain `<ref>` tags that pollute LLM context. Remember to strip them with regex during preprocessing.
- The golden set will have your own bias baked in when you write it. Ideally have someone else blind-review one pass and rewrite any fuzzy items ("about" or "kind of" style questions) into precise ones.
