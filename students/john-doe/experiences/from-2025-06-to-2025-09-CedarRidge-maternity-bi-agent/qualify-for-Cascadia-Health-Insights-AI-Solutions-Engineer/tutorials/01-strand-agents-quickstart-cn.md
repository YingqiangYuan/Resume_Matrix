# Tutorial 01: Strand Agents Quickstart

> 这是 POC-01 的配套教程。先读这份再去做 POC，避免上来就被 framework 概念劝退。

## 1. Strand Agents 是什么，不是什么

Strand Agents 是 AWS 在 2024 年开源的轻量级 agent framework。它的核心立场是：一个 agent 就是"LLM + tools + memory + loop"，没有更多花活。这个立场和 LangChain（更全栈、更重）以及 LlamaIndex（更偏检索）形成对比。Cascadia 选 Strand 是因为它的代码量小、对 AWS Bedrock 原生支持、生产环境的 observability 钩子设计得比 LangChain 干净。

Strand **不是**一个 RAG 框架（虽然它能用来搭 RAG）。它**不是**一个 LLM provider 抽象层（虽然它支持 OpenAI / Anthropic / Bedrock 三家）。它**不是**一个 workflow 编排器（虽然 agent loop 看起来像编排）。把这三层混淆是新手最常犯的错。

## 2. 安装与第一个 Hello Agent

用 `uv` 装：`uv pip install strands-agents`。Python 要 3.10 以上。然后写最小 hello：

```python
from strands import Agent
from strands.models import OpenAIModel

agent = Agent(
    model=OpenAIModel("gpt-4o-mini"),
    system_prompt="You are a helpful research assistant.",
)
print(agent("What is RAG in one sentence?"))
```

这段代码跑通的标志是 stdout 里有一句话回答。如果报 `OPENAI_API_KEY` 缺失，去 platform.openai.com 创建一个 key，存到 `.env` 文件里再用 `python-dotenv` 加载。

## 3. 概念模型：Agent Loop

Strand 的 agent loop 是这样的：

1. 用户输入一段文本。
2. LLM 决定要不要调 tool。如果不调，直接出答案，结束。
3. 如果调 tool，Strand 框架负责把 tool 的输出回填到上下文，再让 LLM 看一眼，决定下一步。
4. 这个循环最多跑 max_iterations 次（默认 10），超了强制返回最后一次 LLM 的输出。

理解这个 loop 之后，写 agent 的核心动作就两件事：定义 tools、写好 system prompt 让 LLM 知道什么时候用什么 tool。

## 4. 定义你的第一个 Tool

Tool 是一个被装饰过的 Python 函数。Strand 用函数的 docstring 和 type hint 自动生成 LLM 看到的 schema：

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
    # 你的 retrieval 实现，这里用 BM25 起步，POC-03 会换成向量检索
    ...
```

把它注册到 agent：`agent = Agent(model=..., tools=[search_wikipedia])`。LLM 在需要查资料时会自己生成对 search_wikipedia 的调用，Strand 负责执行后回填。

设计 tool 的 3 条经验法则：(1) tool 越具体越好，不要写 `do_anything(query)` 这种 god tool；(2) 返回结构化数据（list of dict）比返回长字符串好，LLM 解析准确率更高；(3) docstring 里写清楚什么时候**不**该用这个 tool，比 写"什么时候用"更管用。

## 5. 加上 Memory

最简单的 memory 是 conversation memory：让 agent 记住前几轮的对话。Strand 内置 `ConversationManager`：

```python
from strands.memory import SlidingWindowConversationManager

agent = Agent(
    model=OpenAIModel("gpt-4o-mini"),
    tools=[search_wikipedia],
    conversation_manager=SlidingWindowConversationManager(window_size=10),
)
```

window_size = 10 意味着最近 10 条消息会进入 LLM 上下文，更早的被丢弃。这是面试常考的取舍题：window 太大上下文成本爆炸，太小就丢失上下文。生产环境的解法通常是 sliding window + summarization，但 POC 阶段 sliding window 就够了。

## 6. 包装成 CLI

写一个 `__main__.py`：

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

然后 `python -m poc01 ask "What is RAG?"` 就能跑。Interactive 模式（不带 ask 参数）让你跟 agent 多轮对话，验证 memory 工作正常。

## 7. 面试会被深挖的 3 个点

面试官如果懂 Strand，会问这三类问题：

1. "你的 max_iterations 设的多少？为什么？"。答案应该围绕 cost ceiling + 用户体验之间的取舍。生产环境通常 5-7，POC 可以默认 10。
2. "如果 tool 报错怎么办？"。Strand 默认会把 exception message 回填给 LLM 让它自己处理，但生产环境你应该有重试 + circuit breaker。这是面试官想听的层次。
3. "你怎么知道 agent 不是在幻觉？"。答案是 evaluation harness（POC-03 的内容）+ citation 强制要求（在 system prompt 里要求每个答案带 citation, 不带的拒绝输出）。

## 8. 进一步阅读

- Strand 官方 quickstart：https://github.com/strands-agents/sdk-python（注意是 sdk-python 不是 sdk）
- Anthropic 关于 Building Effective Agents 的指南，把 agent 模式和 workflow 模式区分得很清楚
- 完成 POC-01 之后回头读 Strand 源码里 `agent.py` 的 main loop（约 300 行），把概念模型和代码对应上，面试时讲源码细节是加分项

教程到此结束。回到 [POC-01](../pocs/poc-01-strand-agents/README-cn.md) 开始动手。
