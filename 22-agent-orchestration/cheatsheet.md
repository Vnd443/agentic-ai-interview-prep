# Multi-Agent Orchestration — Cheatsheet

## One-liner
> "Start with one agent; add orchestration only when the task needs decomposition, parallelism, or context isolation — because every extra agent is more tokens, more latency, more failure surface."

## Workflow vs Agent
- **Workflow** = LLM steps on *predefined code paths* (deterministic). Cheaper, debuggable.
- **Agent** = LLM *decides its own steps* at runtime. Flexible, less predictable.
- Most prod systems = workflows with agentic steps. Say this.

## The patterns (Anthropic taxonomy)
| Pattern | Use when |
|---------|----------|
| **Prompt chaining** | Fixed sequential subtasks (outline→draft→polish) |
| **Routing** | Distinct input types → specialized handler / model tier |
| **Parallelization** | Independent subtasks (*sectioning*) or multiple attempts (*voting*) |
| **Orchestrator–Workers** | Subtasks unknown upfront; lead decomposes & delegates |
| **Evaluator–Optimizer** | Clear eval criteria; generate→critique→refine loop |
| **Reflection** | Same agent critiques its own output (light evaluator) |

## Orchestrator–Worker
Lead decomposes → spawns workers (each **own context window**) → parallel → synthesize.
Win = **context isolation**. Cost = ~10–15× tokens of one chat. Earn it.

## Handoffs / Swarm
Peer agents transfer control (A→Billing agent). No central synthesizer. Support-style routing.

## Loop control (SAY THESE)
- **Max iterations** cap (10–25) — #1 guard
- **Token/cost budget** guard
- **Wall-clock timeout** (per step + per task)
- **Termination condition** (explicit `finish` / goal check)
- **Stall/loop detection** (repeated action → break)
- **Retries w/ backoff** on tool errors
- **Human-in-the-loop** gate before risky actions

## Protocols
- **MCP** = agent ↔ tools/data.  **A2A** = agent ↔ agent (cross-vendor).

## Single vs multi (trade-off)
- **Single**: linear, latency/cost-sensitive, tools fit one context. Debuggable.
- **Multi**: separable subtasks, specialized context per role, context would overflow.

## Power moves
- "The trap is over-engineering — I justify every added agent against a single-agent baseline."
- "Sub-agents are really a *context-isolation* tool, not just a parallelism tool." → [[21-context-engineering]]
- "Every autonomous loop needs a max-iteration cap + budget guard + termination condition."
- "MCP connects an agent to tools; A2A connects agents to each other."
