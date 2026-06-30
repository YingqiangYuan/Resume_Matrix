# Tutorial 01: Strand Agents Quickstart

> This is the companion tutorial for POC-01. Read this first before diving into the POC, otherwise the framework concepts will scare you off at the door.

## 1. What Strand Agents Is, and What It Is Not

Strand Agents is a lightweight agent framework AWS open-sourced in 2024. Its core position is that an agent is just "LLM plus tools plus memory plus loop," with no extra fanfare. That stance contrasts with LangChain (more full-stack, heavier) and LlamaIndex (more retrieval-focused). Cascadia picked Strand because the codebase is small, it has native AWS Bedrock support, and the production observability hooks are designed more cleanly than LangChain's.

Strand is **not** a RAG framework (even though you can use it to build RAG). It is **not** an LLM provider abstraction layer (even though it supports OpenAI, Anthropic, and Bedrock). It is **not** a workflow orchestrator (even though the agent loop looks like orchestration). Confusing those three layers is the most common rookie mistake.

## 2. Install and Your First Hello Agent

Install with `uv`, run `uv pip install strands-agents`. Python 3.10 or higher is required. Then write the minimum hello.

```python
from strands import Agent
from strands.models import OpenAIModel

agent = Agent(
    model=OpenAIModel("gpt-4o-mini"),
    system_prompt="You are a helpful research assistant.",
)
print(agent("What is RAG in one sentence?"))
```

You know this snippet works when stdout shows a one-sentence answer. If it complains that `OPENAI_API_KEY` is missing, go to platform.openai.com, create a key, store it in a `.env` file, and load it with `python-dotenv`.

## 3. Conceptual Model, the Agent Loop

Strand's agent loop works like this.

1. The user submits a chunk of text.
2. The LLM decides whether to call a tool. If not, it produces an answer directly and the loop ends.
3. If a tool is called, the Strand framework feeds the tool's output back into context, then lets the LLM look again and decide the next step.
4. The loop runs at most max_iterations times (default 10). If that ceiling is hit, the last LLM output is force-returned.

Once you understand this loop, writing an agent boils down to two things. Define your tools, and write a system prompt good enough for the LLM to know when to use which tool.

## 4. Define Your First Tool

A tool is just a decorated Python function. Strand reads the function's docstring and type hints and auto-generates the schema the LLM sees.

```python
from strands import tool

@tool
def search_wikipedia(query: str, top_k: int = 3) -> list[dict]:
    """Search the local Wikipedia corpus for articles matching the query.
    
    Args:
        query: The user's search query in natural language.
        top_k: How many top matching articles to return (default 3).
    
    Returns:
        A list of dicts, each with keys 'title' and 'excerpt'.
    """
    # Your retrieval implementation. Start with BM25 here, POC-03 swaps in vector retrieval.
    ...
```

Register it on the agent with `agent = Agent(model=..., tools=[search_wikipedia])`. When the LLM needs to look something up, it generates a call to search_wikipedia on its own, and Strand executes the call and feeds the result back.

Three rules of thumb for tool design. (1) The more specific the tool, the better. Do not write god tools like `do_anything(query)`. (2) Returning structured data (a list of dicts) beats returning long strings, the LLM parses it more accurately. (3) Saying in the docstring when **not** to use the tool helps more than saying when to use it.

## 5. Add Memory

The simplest form of memory is conversation memory, letting the agent remember the last few turns of a chat. Strand ships with `ConversationManager`.

```python
from strands.memory import SlidingWindowConversationManager

agent = Agent(
    model=OpenAIModel("gpt-4o-mini"),
    tools=[search_wikipedia],
    conversation_manager=SlidingWindowConversationManager(window_size=10),
)
```

A window_size of 10 means the most recent 10 messages enter the LLM context and anything older gets dropped. This is a classic interview tradeoff question. Too large a window blows up context cost, too small a window loses context. The production answer is usually sliding window plus summarization, but at the POC stage a sliding window is enough.

## 6. Wrap It as a CLI

Write a `__main__.py`.

```python
import sys
from .agent import build_agent

def main():
    agent = build_agent()
    if len(sys.argv) > 1 and sys.argv[1] == "ask":
        print(agent(" ".join(sys.argv[2:])))
    else:
        # interactive mode
        while True:
            user = input("> ").strip()
            if user in {"exit", "quit"}: break
            print(agent(user))

if __name__ == "__main__":
    main()
```

Now `python -m poc01 ask "What is RAG?"` works. The interactive mode (without the ask argument) lets you carry on a multi-turn chat with the agent and verify the memory is working.

## 7. Three Points an Interviewer Will Dig Into

If the interviewer knows Strand, they will ask three categories of question.

1. "What did you set max_iterations to, and why?" The answer should turn on the tradeoff between a cost ceiling and user experience. Production usually runs 5 to 7, a POC can stay at the default 10.
2. "What happens when a tool throws?" Strand by default feeds the exception message back to the LLM and lets it handle the situation, but in production you should layer retries and a circuit breaker on top. That is the depth the interviewer is listening for.
3. "How do you know the agent is not hallucinating?" The answer is an evaluation harness (the content of POC-03) plus a citation requirement (enforced in the system prompt so that every answer must carry citations, refusing to output otherwise).

## 8. Further Reading

- Strand's official quickstart at https://github.com/strands-agents/sdk-python (note it is sdk-python, not sdk).
- Anthropic's Building Effective Agents guide, which draws a clean line between agent patterns and workflow patterns.
- After finishing POC-01, go back and read the main loop in `agent.py` in the Strand source (around 300 lines). Lining the conceptual model up with the code lets you talk source-level detail in interviews, which scores points.

End of tutorial. Go back to [POC-01](../pocs/poc-01-strand-agents/README.md) and start building.
