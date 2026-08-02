# 13 — A2A (Agent-to-Agent)

> An open protocol that lets **independent agents — built by different teams, on different frameworks — discover each other and delegate tasks**, without sharing internals.

**Tier:** 2 — Emerging (differentiator)  |  **Interview frequency:** New but rising (2025–2026)

## Why this matters for your interviews
Everyone can wire up sub-agents *inside one framework*. A2A is about the next step: your LangGraph agent handing a task to someone else's CrewAI agent it has never met, over a standard protocol. Knowing A2A — and crucially **how it differs from MCP** — signals you're tracking where agent systems are heading: an interoperable ecosystem, not one monolith. It's the "agents as microservices" story.

## Subtopics checklist
- [ ] What A2A is: a protocol for **agent ↔ agent** communication (Google-led, 2025)
- [ ] **A2A vs MCP** — MCP connects an agent to **tools/data**; A2A connects an agent to **other agents**
- [ ] **Agent Card** — the public manifest advertising an agent's skills/endpoint
- [ ] **Discovery** — how one agent finds another and reads its capabilities
- [ ] **Task lifecycle** — submit → working → (input-required) → completed/failed
- [ ] **Messages, parts, artifacts** — the units exchanged; streaming updates
- [ ] **Framework-agnostic interop** — LangGraph ↔ CrewAI ↔ ADK, etc.
- [ ] **Auth / security** between agents (who's allowed to delegate what)
- [ ] When A2A is overkill vs a single multi-agent framework ([[12-agent-orchestration]])

## Suggested learning flow
1. Read a primary resource in `resources.md` (the A2A spec/announcement).
2. Write "A2A vs MCP" + the agent-card/task-lifecycle in your own words in `concepts.md`.
3. Distil `cheatsheet.md`.
4. Rehearse `interview-qa.md` out loud — especially the A2A-vs-MCP one-liner.
5. (Optional) Build a project in `projects/` — expose one agent via an agent card, call it from another.

## Interview angles (how they'll probe you)
- "What's A2A and how is it different from MCP?"
- "Two agents built by different teams need to work together — how?"
- "What's in an agent card and why does discovery need one?"
- "When would you *not* use A2A and just use one orchestration framework?"

## Files in this folder
- **README.md** — this hub.
- **concepts.md** — A2A vs MCP, agent cards, discovery, task lifecycle.
- **cheatsheet.md** — terms + one-liners.
- **interview-qa.md** — Q → answer → copy-paste template.
- **resources.md** — primary sources.
- **projects/** — build ideas.

## Status
- [ ] Concepts read & internalized
- [ ] Cheatsheet reviewed
- [ ] Q&A rehearsed out loud
- [ ] Project built / mapped
- [ ] Can teach this topic from memory

_Related: [[12-agent-orchestration]] · [[08-mcp-and-custom-skills]] · [[11-langchain-langgraph]] · [[07-agents-tool-use]]_
_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
