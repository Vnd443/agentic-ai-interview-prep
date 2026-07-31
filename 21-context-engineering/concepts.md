# Context Engineering — Concepts

> Prompt engineering = wording one request. **Context engineering = curating the whole set of tokens the model sees at inference time**, across a long-running agentic task. It's the superset skill for agents.

---

## 1. The core mental model: context is a budget

The context window is finite (e.g. 200K tokens) and **every token competes**. More context is *not* better — irrelevant, stale, or contradictory tokens actively hurt accuracy. Treat the window like RAM: you're deciding what to page in, what to keep resident, and what to evict.

What lives in the window at any step of an agent:
- **System prompt / instructions** (role, rules, output format)
- **Tool definitions** (schemas — these cost tokens too)
- **Retrieved knowledge** (RAG chunks, docs)
- **Memory** (facts recalled from a store)
- **Conversation / message history** (prior turns)
- **Tool call results** (often the biggest, noisiest consumer)
- **The current user request**

Context engineering = getting the **smallest high-signal set** of the above into the window at each step.

---

## 2. The 4 levers (Write · Select · Compress · Isolate)

A clean framework (LangChain's, widely used) for *how* you manage context:

| Lever | What it means | Techniques |
|-------|---------------|------------|
| **Write** | Put info *outside* the window so you don't have to hold it in-context | Scratchpads, external state/files, long-term memory stores |
| **Select** | Pull the *right* info *into* the window when needed | RAG retrieval, memory recall, choosing which tools/files to expose |
| **Compress** | Reduce token footprint of what you keep | Summarization, **compaction**, pruning old turns, trimming tool output |
| **Isolate** | Split context across boundaries so pieces don't contaminate each other | **Sub-agents** with their own windows, sandboxed tool state, per-task context |

If asked "how do you manage context in a long agent run?" → **name these 4 levers**, then give a concrete technique under each.

---

## 3. Memory

Two axes interviewers expect you to separate:

**By lifetime:**
- **Short-term / working memory** — lives *in* the context window (current conversation). Bounded by the window; must be trimmed/summarized as it grows.
- **Long-term memory** — persisted *outside* the window (vector DB, key-value store, files). Recalled selectively via the *Select* lever.

**By type (borrowed from cognitive science):**
- **Episodic** — past events/interactions ("last time the user asked X, we did Y").
- **Semantic** — facts/knowledge ("the user's project uses Postgres").
- **Procedural** — how to do things (system prompt, learned skills/instructions).

Production pattern: at end of a session (or on a token threshold), **summarize the conversation into durable memory**, store it, and on the next session **retrieve only relevant memories** back into context.

---

## 4. Compaction / summarization (the workhorse)

As an agent runs, the message history + tool outputs grow past the window. **Compaction** = replace a chunk of old context with a compressed summary that preserves the decisions, facts, and open threads, then continue.

- Trigger: token-count threshold (e.g. at ~70–80% of the window) or turn count.
- Keep verbatim: the system prompt, recent turns, and anything still actively referenced.
- Summarize/drop: old tool dumps, resolved sub-tasks, superseded plans.
- Risk: **lossy** — a bad summary silently drops a constraint the agent later needs. Mitigate by summarizing to a *structured* format (decisions / facts / todos / open questions) rather than free prose.

This is exactly what long-running coding agents (Claude Code, etc.) do when a session gets long.

---

## 5. Failure modes (name these — huge senior signal)

| Failure | What happens | Fix |
|---------|--------------|-----|
| **Context rot** | Quality degrades as the window fills — the model attends less reliably over very long contexts, even below the max | Keep context lean; compact aggressively; don't pad "just in case" |
| **Lost in the middle** | Info in the *middle* of a long context is recalled worse than info at the start/end | Put the most important facts/instructions at the **top and bottom**; keep critical context short |
| **Context poisoning** | A hallucination or wrong tool result gets written into context/memory and is then treated as fact, compounding | Validate tool outputs before persisting; don't blindly memorize model claims |
| **Context distraction** | Too much (even relevant) context makes the model over-focus on history and ignore the actual task | Trim; re-state the current goal near the end of the prompt |
| **Context clash** | Contradictory info in the window (stale doc vs new doc) confuses the model | De-dupe; prefer freshest source; isolate conflicting sources |

"Lost in the middle" and "context rot" are the two most cited — memorize both.

---

## 6. Structuring context for prompt caching

Prompt caching gives ~90% cheaper reads on the cached prefix (and lower latency). To benefit, **keep the front of the context stable**:
- Order: system prompt → tool defs → static docs → (cache breakpoint) → dynamic conversation.
- Don't inject a timestamp or per-request nonce at the top — it busts the cache.
- Context engineering and cost optimization overlap here (see [[09-cost-optimization]]).

---

## 7. How this differs from RAG and prompt engineering

- **Prompt engineering** — crafting the instruction/wording. A subset.
- **RAG** — one *technique* for the *Select* lever (retrieving external knowledge). A subset.
- **Context engineering** — the whole discipline: assembly, ordering, memory, compaction, isolation, and failure-mode management across a multi-step agent. It's the umbrella.

Interview one-liner: *"Prompt engineering is what you say; context engineering is everything the model can see when it decides what to do."*

---

_Related: [[04-rag]] · [[05-agents-tool-use]] · [[22-agent-orchestration]] · [[09-cost-optimization]] · [[18-llm-system-design]]_
