# A2A (Agent-to-Agent) — Project Ideas

> Build one small thing you can demo. Even a minimal A2A handshake makes a strong talking point.

## Idea 1 — Expose one agent, call it from another (recommended)
Wrap a small specialist agent (e.g. a "currency converter" or "summarizer" agent) with an agent card + A2A endpoint. From a second client agent, discover it and delegate a task end-to-end.
- **Demo:** show the agent card JSON, then the task going submitted → working → completed.
- **Talking point:** discovery via the card, task lifecycle, and that the two could be on different frameworks.

## Idea 2 — A2A + MCP in one flow
Give the remote agent an MCP tool (e.g. a database lookup). The client delegates via A2A; the remote agent fulfills it using MCP.
- **Talking point:** the clearest possible demonstration of "A2A is agent↔agent, MCP is agent↔tools" — you built both layers.

## Idea 3 — Cross-framework handshake
Client on LangGraph, remote on a different framework (CrewAI / ADK), talking over A2A.
- **Talking point:** framework-agnostic interop — the payoff of a shared protocol.

## What to capture for the interview
- The agent card you wrote (skills, endpoint, auth).
- The task lifecycle states you observed.
- One line on when A2A was worth it vs just using an orchestrator.

_Related: [[12-agent-orchestration]] · [[08-mcp-and-custom-skills]] · [[11-langchain-langgraph]]_
