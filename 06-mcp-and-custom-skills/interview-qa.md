# MCP & Custom Agent Skills — Interview Q&A

> Starter set (not from the guide PDF). **This is your differentiator** — be able to go deep and tell a build story.

---

## Q1. What is MCP (Model Context Protocol) and why does it matter?

**Ideal answer:** MCP is an open protocol that standardizes how AI applications connect to external tools, data sources, and context — think "a universal adapter between the model and the outside world." An **MCP server** exposes tools/resources/prompts over a standard interface; any MCP-compatible **client** (agent/IDE) can use them without custom glue. It decouples tool implementations from the agent, so capabilities are reusable across apps and you don't rebuild integrations per framework.

**🔑 Power move:** "Before MCP, every agent-to-tool integration was bespoke. MCP makes tools portable — I built a server once and reused it across clients."

**Follow-ups:**
- What does an MCP server expose? → **tools** (actions), **resources** (data/context), **prompts** (reusable templates).
- Transport? → stdio / HTTP. Client ↔ server messaging.
- Security implications of giving an agent MCP tools? → same as any tool access: permissions, sandboxing, validation.

---

## Q2. What are custom agent skills and how do they differ from tools?

**Ideal answer:** A **skill** is a packaged, reusable set of *instructions/capabilities* (often a `SKILL.md` with a name + description + body) that an agent loads **on demand** when a task matches — extending behaviour without retraining or bloating the base prompt. A **tool** is a callable function/API; a **skill** is more like procedural know-how ("how to do X here") that may orchestrate several tools. Skills keep the context lean by loading only when relevant.

**🔑 Power move:** "Skills let me encode a repeatable workflow once and have the agent invoke it just-in-time — capability without context bloat."

**Follow-ups:** When a skill vs. a fine-tune vs. a longer system prompt? How is a skill discovered/triggered?

---

## Q3. Walk me through building an MCP server / a custom skill you've made.

**Ideal answer (structure):** Problem → why MCP/skill over a hardcoded integration → the interface you exposed (tools/resources) → auth & safety → how a client consumes it → outcome/reuse. *(Fill with your real IBM/agentic-coding work.)*

---

## Your notes / STAR angle
- _TODO: your strongest MCP-server or custom-skill story — this is the headline of your profile._
