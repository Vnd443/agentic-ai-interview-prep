# MCP & Custom Agent Skills — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Starter set (not from the guide PDF). **This is your differentiator** — be able to go deep and tell a build story.

---

**Q1. What is MCP (Model Context Protocol) and why does it matter?**

MCP is an open protocol that standardizes how AI applications connect to external tools, data, and context — a universal adapter between the model and the outside world. An MCP server exposes capabilities over a standard interface, and any MCP-compatible client — an agent, an IDE, a chat app — can use them with no custom glue. The reason it matters: before MCP, every agent-to-tool integration was bespoke, so N apps times M tools meant N×M one-off integrations to build and maintain. MCP is USB-C for LLM tools — I build a server once and reuse it across every compatible client, because it decouples the tool implementation from the agent.

---

**Q2. What does an MCP server expose, and how does a client talk to it?**

A server exposes three primitives: tools, which are actions the model can invoke; resources, which are data or context the model can read; and prompts, which are reusable templates. It's a client–server model — the host app runs an MCP client per server, and the server holds the actual tool and data logic. They communicate over a transport: stdio, which is a local subprocess that's fast and keeps data on-machine, or HTTP for remote and shared servers. The protocol is identical either way; only the distance changes.

---

**Q3. What are custom agent skills, and how do they differ from tools?**

A skill is a packaged, reusable set of instructions — typically a SKILL.md with a name, a description, and a body — that the agent loads on demand when a task matches, so it extends behavior without retraining or bloating the base prompt. A tool is a single callable function; a skill is procedural know-how, "how to do X here," that may orchestrate several tools and carry instructions on using them well. The analogy I use is a drill versus the recipe for mounting a shelf level — the drill is the tool, the recipe is the skill that choreographs the drill, the level, and the stud-finder. Skills keep context lean because they load only when relevant, and the description is what triggers them, so it has to be crisp.

---

**Q4. When would you use a skill versus a fine-tune versus a longer system prompt?**

They trade off permanence against cost. A long system prompt is always loaded, so it burns context on every call even when irrelevant. A fine-tune bakes behavior into the weights — powerful, but expensive, slow to change, and it needs training data. A skill loads only when relevant, so I get the capability without the permanent context cost, and I can edit it in minutes. My order is prompt first, then a skill for any reusable procedure, and fine-tune only when I genuinely need the behavior baked into the model. For a workflow that changes monthly, the skill wins easily — it's a binder I open for the task, not a multi-week training course.

---

**Q5. What are the security implications of giving an agent MCP tools?**

An MCP tool is real access, so the risks are the same as any tool access, plus a supply-chain angle: a third-party server I didn't write runs code and sees my data. So I grant least privilege, sandbox execution, validate inputs and outputs, and gate high-stakes actions behind human approval. I also vet the servers I connect, because a standard connector doesn't make a tool trustworthy — it's like plugging in a USB stick from a stranger; the port being standard doesn't make the device safe. And I treat the content a server returns in resources as untrusted input, because it can carry prompt injection.

---

**Q6. Walk me through an MCP server or custom skill you've built.**

_(Structure to fill with your real work.)_ I start with the problem — the repetitive integration or workflow that was costing time. Then why MCP or a skill over a hardcoded integration — portability and reuse across clients. Then the interface I exposed: which tools, which resources, which prompts, and how I handled auth and safety. Then how a client consumes it and what got reused. I close with the outcome — time saved, number of clients that picked it up, or reliability gained. This is the headline of my profile, so I make the build story concrete and specific.

---

## Your notes / STAR angle
- _TODO: your strongest MCP-server or custom-skill story — problem, interface exposed, auth/safety, reuse, outcome. This is the headline of your profile._
