# Multi-Agent Orchestration — Resources

> Verify links live. Read 2–3 primary sources, then write `concepts.md` in your own words.

## Primary (read these first)
- **Anthropic — "Building Effective Agents"** (Dec 2024). *The* reference for workflow vs agent and the building-block patterns (chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer). https://www.anthropic.com/engineering/building-effective-agents
- **Anthropic — "How we built our multi-agent research system"** (engineering blog). Orchestrator-worker in production + context isolation + cost trade-offs. https://www.anthropic.com/engineering
- **LangGraph — multi-agent docs** (supervisor, network/swarm, hierarchical). State-graph orchestration. https://langchain-ai.github.io/langgraph/

## Frameworks to know by name
- **LangGraph** — graph/state-based orchestration (supervisor, hierarchical teams).
- **OpenAI Agents SDK / Swarm** — handoffs, lightweight multi-agent.
- **CrewAI** — role-based crews of agents.
- **AutoGen (Microsoft)** — conversational multi-agent.

## Protocols
- **MCP (Model Context Protocol)** — agent ↔ tools/data → [[06-mcp-and-custom-skills]].
- **A2A (Agent-to-Agent)** — agent ↔ agent interop (Google-originated open spec). Search "A2A protocol spec".

## Tie-ins from this repo
- Single-agent tool use & ReAct → [[05-agents-tool-use]]
- Context isolation / sub-agent windows → [[21-context-engineering]]
- Evaluator loops → [[08-evaluation]]
- Cost of extra agents → [[09-cost-optimization]]
- Tracing which agent failed → [[19-production-debugging]]

## Watch/skim
- Talks on production multi-agent systems (2025–2026) — search recent conference talks.

_TODO: after reading, paste your 3 best links + one takeaway each here._
