# Context Engineering — Interview Q&A

Format: **Q → ideal answer → power move → likely follow-ups.** Rehearse out loud.

---

### Q1. What's the difference between prompt engineering and context engineering?
**Ideal answer:** Prompt engineering is crafting the wording of a single instruction to get a good response. Context engineering is the broader discipline of managing *everything* the model sees at inference time across a whole task — the system prompt, tool definitions, retrieved knowledge, memory, conversation history, and tool outputs — deciding what to include, in what order, and in what compressed form. For a single Q&A, prompt engineering is enough; for a long-running agent that takes many steps, context engineering is what determines success, because the window fills up and quality degrades if it's poorly managed.

**Power move:** *"Prompt engineering is what you say; context engineering is everything the model can see when it decides what to do. RAG and prompt engineering are both subsets of it."*

**Follow-ups:** Where does RAG fit? (the *Select* lever) · Why does this matter more for agents than chatbots? (multi-step → context accumulates)

---

### Q2. An agent's answers get worse the longer the conversation runs. Why, and what do you do?
**Ideal answer:** Two things. First, **context rot** — as the window fills, the model attends less reliably to any given token, so accuracy drops even before hitting the max length. Second, the history accumulates noise — old tool dumps, resolved sub-tasks, superseded plans — which distracts the model. The fix is compaction: at around 70–80% of the window I summarize old context into a structured form (decisions, established facts, open todos), keep the system prompt and recent turns verbatim, and drop the raw noise. I also offload durable facts to a memory store so they're recalled on demand rather than held in-window the whole time.

**Power move:** *"I summarize to a structured schema — decisions/facts/todos/open-questions — not free prose, so compaction never silently drops a constraint the agent needs later."*

**Follow-ups:** What's the risk of summarizing? (lossy) · How do you pick the trigger? (token threshold) · What do you keep verbatim?

---

### Q3. How do you decide what goes in the context window vs. what to leave out?
**Ideal answer:** I use the four levers — Write, Select, Compress, Isolate. **Write**: anything I don't need *right now* goes to external state (files, memory store), not the window. **Select**: I retrieve only the info relevant to the current step — RAG for knowledge, memory recall for facts, and I expose only the tools the agent needs, since tool schemas cost tokens too. **Compress**: whatever I do keep, I trim — especially tool outputs, which are the biggest silent hog. **Isolate**: if a sub-task has its own heavy context, I hand it to a sub-agent with its own window so it doesn't pollute the main thread. The goal is the smallest high-signal set at each step.

**Power move:** *"Every token competes — I treat the window like RAM, not a filing cabinet. More context usually hurts."*

**Follow-ups:** What's usually the biggest token consumer? (raw tool results) · How do sub-agents help? (context isolation → [[12-agent-orchestration]])

---

### Q4. What is "lost in the middle"? How do you mitigate it?
**Ideal answer:** It's the empirical finding that LLMs recall information at the *start* and *end* of a long context much more reliably than information buried in the *middle*. So if you stuff 50 retrieved chunks in and the key one is at position 25, the model may effectively ignore it. Mitigations: keep critical instructions and facts at the **top and bottom** of the prompt; retrieve fewer, higher-quality chunks (this is where re-ranking earns its place); and keep the overall context short so there's less "middle" to get lost in.

**Power move:** *"This is exactly why re-ranking matters — it's not just relevance, it's putting the best chunk where the model will actually read it."*

**Follow-ups:** How does this relate to context rot? (both are long-context degradation) · How many chunks do you retrieve? (few + rerank, not many)

---

### Q5. What is context poisoning and how do you prevent it?
**Ideal answer:** Context poisoning is when a wrong value — a model hallucination or a bad tool result — gets written into the context or long-term memory and is then treated as ground truth on later steps, so the error compounds. It's especially dangerous with memory, because the agent will confidently build on a fabricated "fact." Prevention: validate tool outputs before acting on them, don't blindly persist model-generated claims into long-term memory, and keep a clear boundary between *retrieved/verified* facts and *model-asserted* ones.

**Power move:** *"Memory is a double-edged sword — anything you persist, you're trusting forever, so I gate what gets written, not just what gets read."*

**Follow-ups:** How is this different from a normal hallucination? (it *persists* and compounds) · Ties to guardrails → [[14-safety-guardrails]].

---

### Q6. What's the connection between context engineering and cost?
**Ideal answer:** Direct. Tokens are the cost unit, so trimming context cuts cost and latency at the same time. Two big levers: compaction reduces the tokens per step on long tasks, and **prompt caching** gives ~90% cheaper reads on a stable prefix. To benefit from caching I order context so the static parts — system prompt, tool defs, static docs — come first and don't change, with dynamic conversation after a cache breakpoint. A common mistake that silently kills caching is putting a timestamp or per-request ID at the very top.

**Power move:** *"Good context engineering is also cost engineering — a lean, cache-friendly context is cheaper, faster, and more accurate all at once."*

**Follow-ups:** What breaks the cache? (changing the prefix) · Quantify caching savings? (~90% on cached reads) → [[15-cost-optimization]]

---

### Q7. How do you handle memory in a multi-session agent?
**Ideal answer:** I separate short-term from long-term. Short-term is the in-window conversation, bounded and compacted as it grows. Long-term lives in an external store — a vector DB for semantic recall, key-value for structured facts. At the end of a session I summarize what happened into durable memory (episodic events + semantic facts), and at the start of the next session I retrieve only the memories relevant to the new request rather than replaying everything. That keeps continuity without blowing the budget, and it's the Write + Select levers working together.

**Power move:** *"I recall selectively, not exhaustively — the point of memory is to *avoid* holding everything in context, so retrieving too much defeats it."*

**Follow-ups:** Episodic vs semantic vs procedural? · How do you decide what's worth persisting? · How do you avoid stale memory clashing with new info? (clash → prefer freshest)

---

_Related: [[05-rag]] · [[07-agents-tool-use]] · [[12-agent-orchestration]] · [[15-cost-optimization]]_
