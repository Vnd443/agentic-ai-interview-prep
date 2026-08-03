# LangChain & LangGraph — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Starter set (not from the guide PDF).

---

**Q1. When do you use LangChain vs. LangGraph vs. plain API calls?**

I reach for the least abstraction that solves the problem. For simple, single-shot LLM use I use plain SDK calls — least magic, easiest to debug. For composing chains, prompt templates, retrievers, and integrations quickly, LangChain saves me from rewriting the same plumbing every project. LangGraph is for when I need stateful, cyclic, multi-step control flow — an agent that loops, branches, retries, and carries state. The trade-off I always name is that frameworks speed you up but add magic and version churn, so in production I keep control flow explicit; LangGraph's graph model gives me that control without hiding the loop, and I'll drop down to the plain SDK when the extra abstraction isn't earning its keep.

---

**Q2. How does LangGraph model an agent, and why a graph?**

LangGraph represents the agent as a state graph: nodes are steps — an LLM call, a tool, a decision — edges define transitions, conditional edges route based on the current state, and a shared state object is threaded through every node. Cycles are what make it powerful: they express ReAct-style loops with explicit stopping conditions, which a linear chain can't do cleanly. The reason a graph beats a hand-rolled agent loop is inspectability — the control flow is drawn out rather than buried in an opaque while-loop, so I can test one node in isolation, resume a run, and see exactly why it branched the way it did.

---

**Q3. How does checkpointing help, and how do you do human-in-the-loop?**

LangGraph persists the shared state at each step, so a run can be paused, resumed, replayed, or rewound instead of only run once end-to-end — like a video-game save point. That buys me two things. First, durability and debugging: a long agent run can recover from a crash mid-task, and I can replay a failure exactly rather than guess. Second, human-in-the-loop becomes a first-class node — I pause the graph before an irreversible action, surface it to a person for approval or edits, and resume from that exact checkpoint. It's the clean way to gate high-stakes actions without losing the run's state.

---

**Q4. How do you handle memory in a LangChain/LangGraph app?**

I split it into short-term and long-term. Short-term is the conversation buffer within the context window, and I trim or summarize it as it grows so I don't overflow context or let cost creep. Long-term is an external store — a vector DB for semantic recall, or a database keyed by user and session for durable facts. The key discipline is being explicit about what's carried forward on each turn rather than dumping the whole history back in, because that's where both context overflow and runaway cost come from. It's the same short-term-in-your-head, long-term-in-a-notebook split I'd use for any stateful system.

---

**Q5. What are the downsides of leaning heavily on these frameworks?**

Two big ones. Abstraction opacity: when something breaks, the bug can be hidden inside the framework's magic rather than my code, so debugging is slower. And version churn: these libraries move fast, and upgrades break things. So my rule is the least abstraction that works — I use LangGraph when I genuinely need controllable, resumable stateful loops, LangChain when integrations and composition save real time, and the plain SDK when neither is pulling its weight. I also keep tracing, evals, guardrails, and stop conditions as my responsibility — the framework doesn't make an app production-ready on its own.

---

## Your notes / STAR angle
- _TODO: what you built with LangChain/LangGraph, why that framework over plain SDK, and what you'd do differently._
