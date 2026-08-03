# Agents & Tool Use — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Core
- Agent = LLM in a loop with tools + goal —
- Observe → think → act → repeat —
- Chat (talk) vs agent (do) —

## Tools
- Tool = function + name + description + JSON schema —
- Description IS the interface (API docs) —
- Right granularity (not god-tool, not 50 micro-tools) —
- Helpful, actionable error messages —
- Idempotent where possible —

## Orchestration patterns
- ReAct (reason + act + observe) —
- Plan-then-Execute —
- Map-Reduce (parallel subtasks) —
- Multi-agent + orchestrator —
- Match pattern to task complexity —

## Chain vs agent vs workflow
- Workflow = you decide the path (deterministic) —
- Agent = LLM decides the path (runtime) —
- Least-agentic thing that works —

## Guardrails
- Least privilege / permissions —
- Human-in-the-loop for high-stakes —
- Sandboxed execution —
- Input + output validation before call —

## Error handling & stopping
- Retries + backoff —
- Fallbacks / graceful degradation —
- Max-iteration cap + budget —
- Termination condition —

## Reach & extensibility
- Computer use (eyes + mouse, no API) —
- MCP (standard tool connection) —
- Custom skills (loadable playbooks) —

## Eval & tracing
- Trace: thought → tool → args → result —
- Metrics: success rate / tool accuracy / steps / cost —
- Replay failing traces; per-tool unit evals —

## Numbers / facts worth quoting
- 
