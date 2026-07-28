# Agents & Tool Use — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q08 (Agents & Tool Use)**.
> This is a **headline topic** on your resume — expect deep probing here.

---

## Q1. How would you build an AI agent that can use tools? ⭐ (guide Q08)

**Why they ask:** Agentic AI is the biggest applied-AI trend — agents that take *actions*, not just generate text.

**Ideal answer:**
1. **Core architecture** — an agent is an **LLM in a loop**: observe (input + tool results) → think (reason) → act (call a tool or respond). The LLM picks the tool and parameters from the current state.
2. **Tool definition** — clear names, descriptions, and JSON schemas. Tool descriptions *are* the interface — treat them like API docs; better descriptions → more reliable tool use.
3. **Orchestration patterns** — **ReAct** (reason+act) for step-by-step; **Plan-then-Execute** for complex multi-step; **Map-Reduce** for parallel subtasks. Match the pattern to task complexity.
4. **Error handling** — tools fail and APIs time out. Add retries, fallbacks, graceful degradation, and a **max-iteration limit** to prevent infinite loops. Log every step.
5. **Safety & guardrails** — real tool access is dangerous: permission systems, **human-in-the-loop** approval for high-stakes actions, sandboxed execution, and output validation *before* a tool call executes.

**🔑 Power move:** Name a specific framework you've used (LangGraph, CrewAI, or custom) and explain **why** you chose it over alternatives — shows real-world decision-making.

**Follow-ups:**
- ReAct vs. Plan-Execute — when each? → Plan-Execute for known multi-step workflows; ReAct for open-ended, adaptive tasks.
- How do you stop infinite loops / runaway cost? → max iterations, budget caps, loop detection.
- How do you make tool calls reliable? → strict schemas, validation, retries, idempotent tools.

---

## Q2. What's the difference between a chain, an agent, and a workflow?

**Ideal answer:** A **chain/workflow** is a *predefined* sequence of steps (deterministic control flow) — you decide the path. An **agent** lets the *LLM* decide the path at runtime (which tool, when to stop). Agents are more flexible but less predictable and harder to debug — prefer a workflow when the steps are known, an agent when they're not.

**🔑 Power move:** "I reach for the least-agentic thing that works — determinism is a feature in production."

**Follow-ups:** When is full agency overkill? How do you constrain an agent's action space?

---

## Q3. How do you evaluate and debug an agent?

**Ideal answer:** Trace every step (thought → tool call → args → result). Evaluate **task success rate**, tool-selection accuracy, number of steps, and cost per task. Common failures: wrong tool, malformed args, looping, ignoring tool errors. Log full traces and replay them; add per-tool unit evals.

---

## Q4. (Your edge) How do MCP and custom skills relate to agents?

**Ideal answer:** **MCP (Model Context Protocol)** standardizes how an agent connects to external tools/data via servers — decoupling tool implementations from the agent. **Custom agent skills** package reusable instructions/capabilities the agent loads on demand. Together they make agents extensible without retraining. *(See folder 06 for depth — this is your differentiator.)*

---

## Your notes / STAR angle
- _TODO: an agent you built — framework, tools, orchestration pattern, guardrails, outcome._
