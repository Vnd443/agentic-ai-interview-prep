# 10 — Memory Systems

> How an agent **remembers** across turns and across sessions — what to store, where to store it, and how to pull the right memory back at the right moment.

**Tier:** 1 — GenAI Core  |  **Interview frequency:** Rising (agentic roles)

## Why this matters for your interviews
The context window is short-term memory — it disappears when the conversation ends or gets trimmed. **Memory systems** are how an agent keeps useful information *beyond* the window: your name, past decisions, a running project state. Interviewers for agentic roles want to hear that you can separate **what belongs in the prompt right now** from **what belongs in a store you retrieve later** — and that you know memory is a retrieval problem, not just a storage one.

## Subtopics checklist
- [ ] Short-term (in-window) vs **long-term** (external store) memory
- [ ] Memory types: **episodic** (what happened), **semantic** (facts), **procedural** (how-to / skills)
- [ ] Session memory vs **cross-session / user memory** (persists across conversations)
- [ ] Conversation memory patterns: **buffer, summary, buffer-window, summary-buffer**
- [ ] Where memory lives: vector DB, key-value store, **DynamoDB / RDS**, a document
- [ ] **Writing** memory — what's worth saving, and when (every turn? on a signal?)
- [ ] **Retrieving** memory — semantic search vs recency vs explicit key lookup
- [ ] **Forgetting / decay** — pruning, TTL, relevance scoring (memory isn't infinite)
- [ ] Memory in frameworks: **LangGraph checkpointers / store**, LangChain memory
- [ ] Failure modes: stale memory, contradictory memories, memory poisoning
- [ ] Semantic caching — reuse answers for semantically similar queries (cut cost/latency)

## Suggested learning flow
1. Read a primary resource in `resources.md` (LangGraph memory + a memory-store writeup).
2. Write the memory types + read/write/forget loop in your own words in `concepts.md`.
3. Distil `cheatsheet.md` (memory types + patterns + one-liners).
4. Rehearse `interview-qa.md` out loud.
5. Build a project in `projects/` (an agent that remembers you across sessions).

## Interview angles (how they'll probe you)
- "What's the difference between memory and just a longer context window?"
- "Your chatbot forgets the user between sessions — how do you fix it?"
- "How do you decide what to write to long-term memory vs let it go?"
- "How would you retrieve the *right* memory when the store has thousands of entries?"

## Files in this folder
- **README.md** — this hub.
- **concepts.md** — memory types, the read/write/forget loop, storage choices.
- **cheatsheet.md** — terms, patterns, one-liners.
- **interview-qa.md** — Q → answer → copy-paste template.
- **resources.md** — primary sources.
- **projects/** — build ideas.

## Status
- [ ] Concepts read & internalized
- [ ] Cheatsheet reviewed
- [ ] Q&A rehearsed out loud
- [ ] Project built / mapped
- [ ] Can teach this topic from memory

_Related: [[09-context-engineering]] · [[05-rag]] · [[07-agents-tool-use]] · [[12-agent-orchestration]]_
_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
