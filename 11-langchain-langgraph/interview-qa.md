# LangChain & LangGraph — Interview Q&A

> Starter set (not from the guide PDF).

---

## Q1. When do you use LangChain vs. LangGraph vs. plain API calls?

**Ideal answer:** **Plain SDK calls** for simple, single-shot LLM use — least abstraction, easiest to debug. **LangChain** for composing chains, prompt templates, retrievers, and integrations quickly. **LangGraph** when you need **stateful, cyclic, multi-step** control flow — agents that loop, branch, retry, and maintain state as a graph. Reach for the least abstraction that solves the problem.

**🔑 Power move:** "Frameworks speed you up but add magic; in production I keep control flow explicit — LangGraph's graph model gives that without hiding the loop."

**Follow-ups:** Downsides of heavy framework use? (debugging opacity, version churn) When would you drop it?

---

## Q2. How does LangGraph model an agent, and why a graph?

**Ideal answer:** LangGraph represents the agent as a **state graph**: nodes are steps (LLM call, tool, decision), edges (including conditional edges) define transitions, and a shared **state** object is threaded through. Cycles enable loops (ReAct-style) with explicit stopping conditions. The graph makes control flow inspectable, resumable, and testable — vs. an opaque agent loop.

**Follow-ups:** How do you persist/checkpoint state? Human-in-the-loop nodes?

---

## Q3. How do you handle memory in a LangChain/LangGraph app?

**Ideal answer:** Short-term: conversation buffer within the context window (trim/summarise as it grows). Long-term: external store (vector DB for semantic recall, or a DB keyed by user/session). Be explicit about what's carried forward to avoid context overflow and cost creep.

---

## Your notes / STAR angle
- _TODO: what you built with LangChain/LangGraph and why that framework._
