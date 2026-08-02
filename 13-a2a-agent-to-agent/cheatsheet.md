# A2A (Agent-to-Agent) — Cheatsheet

> Fill each `—` with your own one-line explanation from memory. Test recall.

## Big picture
- A2A in one line —
- A2A vs MCP (the key one) —
- Agents as microservices —

## Core pieces
- Agent Card —
- Discovery —
- Task lifecycle (submitted → working → input-required → completed/failed) —
- Message —
- Part —
- Artifact —

## Interop & security
- Framework-agnostic (LangGraph ↔ CrewAI ↔ ADK) —
- Transport (HTTP + JSON) —
- Auth / authorization between agents —

## Judgment
- When A2A earns its keep (cross-org / cross-framework) —
- When it's overkill (one framework → use orchestrator) —

## Failure modes / risks
- Unauthorized delegation —
- A remote agent going down mid-task —
- Runaway agent-to-agent loops (cap + budget) —
