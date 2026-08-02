# Multi-Agent Orchestration — Interview Q&A

Format: **Q → ideal answer → power move → likely follow-ups.** Rehearse out loud.

---

### Q1. When would you use multiple agents instead of one — and when would you NOT?
**Ideal answer:** I default to one agent with a good tool set, because it's cheaper, lower-latency, and far easier to debug. I go multi-agent only when the task has genuinely separable sub-problems that benefit from parallelism, or when sub-tasks need *specialized context* — different tools or knowledge per role — or when one agent's context would overflow or get polluted doing everything itself. Deep research, large multi-file code changes, and multi-domain support are good fits. I would NOT use multiple agents for a linear, latency-sensitive, or cost-sensitive task — the extra tokens and coordination overhead aren't justified.

**Power move:** *"The trap is over-engineering. I justify every added agent against a single-agent baseline — more agents means more tokens, more latency, and more places to fail."*

**Follow-ups:** What's the token cost multiplier? (~10–15× for orchestrator+workers) · How do you know a single agent is failing? (context overload / serial latency)

---

### Q2. What's the difference between a workflow and an agent?
**Ideal answer:** In a workflow, the LLM steps run through predefined code paths that I control — deterministic, predictable, easy to debug. In an agent, the LLM dynamically decides its own steps and which tools to call at runtime — flexible but less predictable. Most real production systems are actually workflows with a few agentic steps, not fully autonomous agents, because you want control where the task is known and autonomy only where it genuinely helps.

**Power move:** *"Autonomy is a cost, not a goal — I use deterministic workflow paths wherever the steps are known and reserve agentic freedom for the parts that actually need it."*

**Follow-ups:** Name the workflow patterns · Where do you draw the autonomy line?

---

### Q3. Walk me through designing an orchestrator-worker system for research.
**Ideal answer:** A lead orchestrator agent takes the research question and decomposes it into sub-questions. It spawns worker agents — each with its **own context window** — to investigate one sub-question with search/retrieval tools, running in parallel. Each worker returns a structured finding. The orchestrator then synthesizes the findings into a final answer, resolving conflicts and citing sources. The key design choices: workers are isolated so deep exploration of one thread doesn't pollute the others; results come back structured so synthesis is clean; and I cap workers and iterations so cost stays bounded. I'd add an evaluator pass at the end to check the synthesis is grounded.

**Power move:** *"The real reason for sub-agents here is context isolation — each worker explores deeply in its own window, and only a compressed result crosses back to the lead."*

**Follow-ups:** How many workers? (bounded, task-dependent) · How do you stop cost blowup? (caps + budget) · How do workers share state?

---

### Q4. How do you stop an agent loop from running forever or exploding in cost?
**Ideal answer:** Several guards, layered. A hard **max-iteration cap** on the loop — usually the single most important one. A **token/cost budget** that stops or degrades gracefully when exceeded. **Wall-clock timeouts** per step and per task. An explicit **termination condition** — the agent signals done via a finish tool or a goal check passes. **Stall detection** — if the agent repeats the same action or makes no progress, break out. And retries with backoff on tool errors so one flaky tool doesn't kill the run. For high-risk actions I add a human-in-the-loop approval gate.

**Power move:** *"Every autonomous loop needs three things minimum: a max-iteration cap, a budget guard, and a clear termination condition — without them it's a runaway."*

**Follow-ups:** What iteration cap do you pick? (10–25, task-dependent) · How do you detect a stall? · What do you do when budget is hit? (degrade / return partial)

---

### Q5. What is reflection / evaluator-optimizer, and when is it worth the extra calls?
**Ideal answer:** It's a generate-then-critique loop: one agent produces output, a second agent (or the same one, for reflection) evaluates it against criteria and gives feedback, and the generator revises — repeating until the evaluator passes or a cap is hit. It's worth the extra calls when there are **clear evaluation criteria** and iteration measurably improves quality — code that must pass tests, translations, structured extraction. It's *not* worth it when you can't articulate what "better" means, because then the critique just adds cost and latency without reliable gains.

**Power move:** *"Reflection only pays off when the eval signal is real — if I can't write down the pass criteria, adding an evaluator just burns tokens."*

**Follow-ups:** Reflection vs evaluator-optimizer? (same agent vs separate) · How many iterations? · Ties to eval → [[06-evaluation]]

---

### Q6. What's the difference between MCP and A2A?
**Ideal answer:** They solve different connection problems. **MCP** — Model Context Protocol — is a standard for connecting an agent to *tools, data, and resources*: it's the agent-to-capability layer. **A2A** — Agent-to-Agent — is an emerging standard for agents built on *different frameworks or from different vendors* to discover and communicate with each other as peers. So MCP is agent↔tools, A2A is agent↔agent; they're complementary, and a serious multi-agent system might use both.

**Power move:** *"MCP connects an agent to its tools; A2A connects agents to each other — different layers of the same stack."*

**Follow-ups:** Why standardize agent communication at all? (interop across vendors) · How does context transfer in a handoff? → [[08-mcp-and-custom-skills]]

---

### Q7. What are the failure modes of multi-agent systems and how do you handle them?
**Ideal answer:** Main ones: **cost/latency blowup** from too many agents and turns — handled with caps and budgets; **error propagation**, where one worker's bad output corrupts the synthesis — handled by validating worker results and having the orchestrator sanity-check before synthesizing; **context poisoning**, a hallucination getting passed between agents as fact — handled by keeping outputs structured and verified; and **coordination bugs / deadlocks** in handoff systems — handled by clear termination and a central synthesizer where possible. I also add observability — tracing each agent's calls — so I can localize which agent failed.

**Power move:** *"With multiple agents, a wrong answer can look confident because it survived several hops — so I validate at the boundaries, not just at the end."*

**Follow-ups:** How do you debug which agent failed? (tracing → [[16-monitoring-and-debugging]]) · How is this related to context poisoning? → [[09-context-engineering]]

---

_Related: [[07-agents-tool-use]] · [[09-context-engineering]] · [[11-langchain-langgraph]] · [[06-evaluation]] · [[22-llm-system-design]]_
