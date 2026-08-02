# A2A (Agent-to-Agent) — Interview Q&A

Copy-paste template for any A2A answer:

> **What A2A connects** (agent ↔ agent) → **contrast with MCP** (agent ↔ tools) → **the mechanism** (agent card → discovery → task lifecycle) → **when it's worth it** (cross-boundary) → a trade-off.

---

**Q. What is A2A and how is it different from MCP?**

A2A is an open protocol for agents to talk to other agents — discover a peer, hand it a task, and get the result — even across different teams and frameworks. MCP solves a different layer: it connects a single agent to its tools and data sources. So MCP is agent-to-tools, A2A is agent-to-agent, and they're complementary — an agent uses MCP to reach a database and A2A to delegate a sub-task to another agent. My one-liner: MCP is the drill in your hand, A2A is calling in the electrician.

---

**Q. Two agents built by different teams, on different frameworks, need to work together. How?**

That's exactly what A2A is for. Each agent publishes an agent card — a manifest at a known URL listing its skills, endpoint, and auth. My agent discovers the other's card, submits a task, and tracks it through its lifecycle: submitted → working → completed, with an input-required state if the remote agent needs more info. Because A2A is just a protocol over HTTP and JSON, it doesn't matter that one is LangGraph and the other is CrewAI — same reason Gmail and Outlook exchange email over SMTP. I wouldn't rewrite their agent into my framework; I'd speak the protocol.

---

**Q. What's in an agent card and why is it needed?**

An agent card is the agent's public profile — its name, the skills it offers, its endpoint URL, and its authentication scheme, usually as JSON at a well-known address. It's needed for discovery: before I delegate, my agent has to know what the other one can actually do and how to reach and authenticate to it. It's the business card an agent hands out so peers can decide whether it's the right one for the job.

---

**Q. When would you NOT use A2A?**

When all my agents live in one codebase and one framework. Then a multi-agent orchestrator is simpler — I'm just calling functions I already own, so a cross-org protocol is pure overhead. A2A earns its keep when agents are independently owned, deployed, or built on different stacks. The analogy: I don't send myself a contract and invoice to pass the salt — that paperwork is for working with an outside party. So: A2A across boundaries, in-framework sub-agents within one system.

---

**Q. Agents calling other agents sounds risky — how do you keep it safe?**

Two angles. Security: A2A carries authentication and authorization declared in the agent card, so a remote agent can verify who's calling and whether they're allowed to run a given skill — I don't let any caller trigger sensitive actions. Control: agent-to-agent delegation can loop, so I cap iterations, set a budget guard, and define clear termination conditions, same as any agent loop. And I treat a remote agent's output as untrusted input — validate it before acting, which ties into guardrails.
