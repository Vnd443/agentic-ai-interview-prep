# 09 — Context Engineering

> The discipline of deciding **what goes into the model's context window, when, and in what form** — so the model has exactly the right information to act, and nothing that distracts it.

**Tier:** 1 — GenAI Core (DIFFERENTIATOR)  |  **Interview frequency:** Rising fast (2025–2026)

## Why this matters for your interviews
"Prompt engineering" is wording a single request; **context engineering** is managing the *entire* context an agent sees across a long-running task — system prompt, tools, memory, retrieved docs, prior turns, tool outputs. For agentic systems this is *the* skill: models degrade when context is too long, contradictory, or noisy. Being able to talk about **context assembly, compaction, memory, and failure modes** signals senior, production-grade thinking.

## Subtopics checklist
- [ ] Context window as a **budget** (every token competes; more ≠ better)
- [ ] The 4 levers: **Write, Select, Compress, Isolate** context
- [ ] **Write** — scratchpads, external memory, state outside the window
- [ ] **Select** — RAG/retrieval, tool-result filtering, choosing what to pull in
- [ ] **Compress** — summarization, compaction, pruning old turns
- [ ] **Isolate** — sub-agents / separate contexts to avoid cross-contamination
- [ ] **Memory:** short-term (in-window) vs long-term (store); episodic vs semantic
- [ ] Failure modes: **context rot, lost-in-the-middle, context poisoning, distraction, clash**
- [ ] Prompt caching & structuring context so the cacheable prefix is stable
- [ ] Context for **multi-turn agents** (conversation grows → summarize/trim)
- [ ] Context structure: **SYSTEM / CONTEXT / USER** separation (what goes in each block)

## Suggested learning flow
1. Read a primary resource in `resources.md` (Anthropic + LangChain writeups).
2. Write the 4 levers + failure modes in your own words in `concepts.md`.
3. Distil `cheatsheet.md` (terms + one-liners + the failure-mode list).
4. Rehearse `interview-qa.md` out loud.
5. Build a project in `projects/` (a compaction/memory strategy you can demo).

## Interview angles (how they'll probe you)
- "What's the difference between prompt engineering and context engineering?"
- "An agent's answers get worse as the conversation gets longer — why, and what do you do?"
- "How do you decide what to keep in context vs. offload to memory/retrieval?"
- "What is context rot / lost-in-the-middle and how do you mitigate it?"

## Files in this folder
- **README.md** — this hub.
- **concepts.md** — the 4 levers, memory, failure modes, explained.
- **cheatsheet.md** — terms, one-liners, the failure-mode list.
- **interview-qa.md** — Q → ideal answer → power move → follow-ups.
- **resources.md** — primary sources.
- **projects/** — build ideas.

## Status
- [ ] Concepts read & internalized
- [ ] Cheatsheet reviewed
- [ ] Q&A rehearsed out loud
- [ ] Project built / mapped
- [ ] Can teach this topic from memory

_Related: [[07-agents-tool-use]] · [[12-agent-orchestration]] · [[05-rag]] · [[02-llm-foundations]]_
_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
