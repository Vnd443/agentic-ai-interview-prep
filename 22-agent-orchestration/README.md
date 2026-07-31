# 22 — Multi-Agent Orchestration

> How to coordinate **multiple LLM agents (and loops)** to solve tasks one agent can't — via decomposition, specialization, and control — while keeping cost and reliability in check.

**Tier:** 1 — GenAI Core (DIFFERENTIATOR)  |  **Interview frequency:** High for Agentic AI / FDE roles

## Why this matters for your interviews
Single-agent tool use is table stakes. The senior signal is knowing **when to add more agents, which topology to use, and how to control the loop** so it doesn't run forever or blow the budget. This is your resume's home turf (Agentic AI Engineer) — you should own the orchestration patterns *and* their trade-offs, including "when a single agent is better."

## Subtopics checklist
- [ ] The agent loop recap: **observe → think → act → repeat** (control it)
- [ ] **Loop control:** max-iteration caps, budget/token guards, termination conditions, stall detection
- [ ] **Orchestrator–Worker** (a lead agent spawns/coordinates sub-agents)
- [ ] **Sub-agents** and **context isolation** (each gets its own window)
- [ ] **Routing** (a classifier/router sends the task to the right agent/model)
- [ ] **Handoffs** (agent → agent transfer of control; swarm style)
- [ ] **Reflection / self-critique** and **Evaluator–Optimizer** (generate → critique → refine)
- [ ] **Prompt chaining** vs **parallelization** vs **dynamic** orchestration
- [ ] Single-agent vs multi-agent trade-offs (cost, latency, failure surface)
- [ ] Shared state / memory across agents; communication protocols (A2A)
- [ ] Reliability: retries, timeouts, error propagation, human-in-the-loop gates

## Suggested learning flow
1. Read Anthropic's "Building effective agents" + a LangGraph multi-agent guide (`resources.md`).
2. Write each topology + when to use it in `concepts.md`.
3. Distil `cheatsheet.md` (patterns table + loop-control list).
4. Rehearse `interview-qa.md` out loud.
5. Build one multi-agent demo in `projects/`.

## Interview angles (how they'll probe you)
- "When would you use multiple agents instead of one? When would you NOT?"
- "Design an orchestrator-worker system for [research / coding / support]."
- "How do you stop an agent loop from running forever or exploding in cost?"
- "What's reflection / evaluator-optimizer and when is it worth the extra calls?"

## Files in this folder
- **README.md** — this hub.
- **concepts.md** — topologies, loop control, trade-offs.
- **cheatsheet.md** — pattern table + control guards + one-liners.
- **interview-qa.md** — Q → ideal answer → power move → follow-ups.
- **resources.md** — primary sources.
- **projects/** — build ideas.

## Status
- [ ] Concepts read & internalized
- [ ] Cheatsheet reviewed
- [ ] Q&A rehearsed out loud
- [ ] Project built / mapped
- [ ] Can teach this topic from memory

_Related: [[05-agents-tool-use]] · [[21-context-engineering]] · [[11-langchain-and-langgraph]] · [[18-llm-system-design]] · [[06-mcp-and-custom-skills]]_
_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
