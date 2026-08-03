# Agents & Tool Use — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q08 (Agents & Tool Use)**.
> This is a **headline topic** on your resume — expect deep probing here.

---

**Q1. How would you build an AI agent that can use tools? ⭐ (guide Q08)**

An agent is fundamentally an LLM in a loop: it observes the current state (input plus prior tool results), reasons about the next step, acts by calling a tool or answering, reads the result, and repeats until the goal is met. The pieces I focus on are, first, tool definition — clear names, descriptions, and JSON schemas, because the description is the interface the model reasons over, so I write them like API docs. Second, an orchestration pattern matched to the task: ReAct for open-ended work, plan-then-execute for known multi-step workflows, map-reduce for parallel subtasks. Third, error handling — retries with backoff, fallbacks, idempotent tools, and a hard max-iteration and budget cap so it can't loop forever. Fourth, guardrails, because real tool access is dangerous: least-privilege permissions, human-in-the-loop approval for irreversible actions, sandboxed execution, and validating arguments before the call runs. The power move is naming the framework I actually used — LangGraph, CrewAI, or custom — and why I chose it over the others.

---

**Q2. What's the difference between a chain, an agent, and a workflow?**

A chain or workflow is a predefined sequence of steps — I decide the path, so it's deterministic and easy to debug. An agent lets the LLM decide the path at runtime: which tool to call and when to stop. Agents are more flexible but less predictable and harder to trace, so my rule is to reach for the least-agentic thing that works — determinism is a feature in production. I give the LLM genuine agency only where the path can't be known ahead of time; if the steps are fixed, I hard-code them and just use the model for the parts that need judgment.

---

**Q3. How do you stop an agent from looping forever or burning runaway cost?**

Every loop gets a hard stop condition: a max-iteration cap, a token or dollar budget, and an explicit termination check for "goal met." On top of that I add loop detection — if the agent repeats the same tool call with the same args, I break out. Tools are idempotent where possible so retries are safe, and I trace steps so I can see when it's spinning. An agent without a stop condition is an outage waiting to happen, so I treat the cap as mandatory, not optional.

---

**Q4. Why do agents pick the wrong tool or pass bad arguments, and how do you fix it?**

Most of the time it's not the model being dumb — it's the tool definitions. If a description is vague or two tools overlap, the model guesses. So I write each tool like API documentation: an unambiguous name, when to use it, what each argument means, and a helpful, actionable error message when a call is malformed, so the model can self-correct instead of hitting a dead end. I also keep tools right-sized — not one god-tool with twenty arguments, not fifty micro-tools — and validate arguments against the schema before executing. Better tool ergonomics fix reliability far more than swapping the model.

---

**Q5. How do you evaluate and debug an agent?**

Because the path is dynamic, I trace every step — the thought, the tool called, the arguments, and the result — so I can replay any failure instead of guessing. I measure task success rate, tool-selection accuracy, number of steps, and cost per task. The common failure modes are choosing the wrong tool, passing malformed arguments, ignoring a tool error, and looping, so I add per-tool unit evals and watch those specifically. I can't debug an agent I can't see, so tracing is the foundation, and it doubles as my production monitoring signal.

---

**Q6. (Your edge) How do MCP and custom skills relate to agents?**

MCP, the Model Context Protocol, standardizes how an agent connects to external tools and data through servers, so tool implementations are decoupled from the agent — it's like a USB-C port for tools, any compliant tool plugs into any compliant agent without custom wiring. Custom skills package reusable instructions the agent loads on demand, like playbooks it pulls off the shelf when a task matches. Together they let me extend an agent's capabilities without retraining or rebuilding it, which is the practical way real systems grow. This is my differentiator, and folder 08 goes deep on it.

---

## Your notes / STAR angle
- _TODO: an agent you built — framework, the tools, orchestration pattern, guardrails, and the outcome (success rate, cost, time saved)._
