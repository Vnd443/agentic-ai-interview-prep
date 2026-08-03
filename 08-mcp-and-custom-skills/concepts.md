# MCP & Custom Agent Skills — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> **This is your differentiator** — few candidates can go deep here. Builds on agents ([[07-agents-tool-use]]); contrast with agent-to-agent ([[13-a2a-agent-to-agent]]).

---

## 1. The problem MCP solves (N×M bespoke wiring)

**Definition:** Before **MCP**, every agent-to-tool connection was custom code: each app wired each tool its own way. With *N* apps and *M* tools that's *N×M* one-off integrations to build and maintain.

**Example — chargers before USB-C:** every phone had its own plug, so every household had a drawer of incompatible cables. USB-C made one standard connector work everywhere. MCP does that for LLM tools.

**Why it matters:** The framing that lands the whole topic. *"Before MCP, every agent-to-tool integration was bespoke — MCP makes tools portable. I build a server once and reuse it across any compatible client."*

---

## 2. What MCP is (a standard protocol)

**Definition:** **MCP (Model Context Protocol)** is an open protocol that standardizes how AI apps connect to external tools, data, and context. An **MCP server** exposes capabilities over a standard interface; any MCP-compatible **client** (agent, IDE, chat app) can use them with no custom glue.

```mermaid
flowchart LR
    subgraph Host [Host app · Claude / IDE]
        LLM[LLM] --- C1[MCP client]
        LLM --- C2[MCP client]
    end
    C1 -->|stdio / HTTP| S1[MCP server A]
    C2 -->|stdio / HTTP| S2[MCP server B]
    S1 --> R1[Tools · Resources · Prompts]
    S2 --> R2[DB / API / files]
```

**Example:** a **universal power adapter** for travel — the wall socket (client) and your device (tool) don't need to know each other's shape; the adapter (protocol) makes any-to-any work.

**Why it matters:** It **decouples tool implementations from the agent**, so capabilities are reusable across apps and you don't rebuild integrations per framework. *"MCP is USB-C for LLM tools."*

---

## 3. The three server primitives (tools, resources, prompts)

**Definition:** An MCP server exposes three kinds of things:
- **Tools** — *actions* the model can invoke (send email, run a query).
- **Resources** — *data/context* the model can read (files, DB rows, docs).
- **Prompts** — *reusable templates* a user or client can trigger.

**Example — a workshop:** **tools** are the power tools you operate, **resources** are the reference manuals on the shelf, **prompts** are the pre-written job cards ("how to service a bike"). Same workshop, three kinds of help.

**Why it matters:** Knowing the exact surface signals hands-on depth. *"A server exposes tools for actions, resources for data, and prompts for reusable templates — that's the whole contract."*

---

## 4. Client–server model & transports (stdio / HTTP)

**Definition:** MCP is **client–server**. The **host app** runs an MCP **client** per server; the **server** runs the actual tool/data logic. They talk over a **transport**: **stdio** (local subprocess — fast, private) or **HTTP** (remote/networked).

**Example:** stdio = talking to a colleague **in the same room** (a local process on your machine). HTTP = **phoning** a colleague in another building (a remote server). Same conversation, different distance.

**Why it matters:** *"stdio for local servers I run alongside the app, HTTP for remote/shared ones — the protocol is the same, only the transport changes."* Local stdio keeps sensitive data on-machine.

---

## 5. Using existing servers vs. building your own

**Definition:** You can **consume** ready-made servers (filesystem, GitHub, Slack, Postgres, etc.) or **build** one to expose *your* systems — define the tools/resources/prompts, handle auth, and any client can use it.

**Example:** you can buy standard USB-C peripherals off the shelf (existing servers), or manufacture your own device with a USB-C port so it works with everyone's laptops (your server).

**Why it matters:** The build story is the memorable part. *"I reuse community servers for common systems and build my own to expose our internal APIs once, so every client gets them for free."*

---

## 6. What a custom skill is (loadable know-how)

**Definition:** A **skill** is a packaged, reusable set of *instructions/capabilities* — typically a `SKILL.md` with a name, a description, and a body — that an agent **loads on demand** when a task matches. It extends behavior without retraining or bloating the base prompt.

**Example:** a chef's **recipe cards** in a box. The chef doesn't memorize 500 recipes (bloated prompt); they grab the *one* card when the order calls for it. The card's title (description) is how they know which to pull.

**Why it matters:** *"Skills let me encode a repeatable workflow once and have the agent invoke it just-in-time — capability without context bloat."* The **description is the trigger**, so it must be crisp (→ [[09-context-engineering]]).

---

## 7. Skill vs. tool (know-how vs. a callable)

**Definition:** A **tool** is a single callable function/API (one action). A **skill** is *procedural know-how* — "how to do X here" — that may **orchestrate several tools** and inject instructions.

**Example:** a **drill** is a tool (one action). *"How to mount a shelf level"* is a skill — it tells you to use the drill, the level, and the stud-finder in the right order. The skill choreographs the tools.

**Why it matters:** A crisp distinction interviewers love. *"A tool is a verb the model can call; a skill is a playbook that may call several tools and carry instructions on how to use them."*

---

## 8. Skill vs. fine-tune vs. long system prompt (the trade-off)

**Definition:** Four ways to give an agent a capability:
- **Long system prompt** — always loaded → burns context every call, even when irrelevant.
- **Fine-tune** — bakes behavior into weights → powerful but costly, slow to change, needs data.
- **Tool** — an action, but not the *procedure* for using it well.
- **Skill** — loaded **only when relevant** → capability without permanent context cost, editable in minutes.

**Example:** teaching an employee. **Fine-tune** = send them to a multi-week course (permanent, expensive). **Long prompt** = tape a huge instruction sheet to their monitor forever (always in the way). **Skill** = a binder they open only for the task at hand. For a workflow that changes monthly, the binder wins.

**Why it matters:** *"I try prompt first, then a skill for reusable procedures, and fine-tune only when I need the behavior baked in — skills give me most of the benefit with none of the retraining cost."*

---

## 9. Security of granting agents MCP tools

**Definition:** An MCP tool is real access, so the risks are the same as any tool access — plus a supply-chain angle: a **third-party server** you didn't write runs code and sees your data. Controls: **least privilege**, **sandboxing**, **input/output validation**, **human-in-the-loop** for high-stakes actions, and **vetting/trusting** the servers you connect. Watch for **prompt injection** via resources a server returns.

**Example:** plugging a **USB stick from a stranger** into your work laptop. The port being standard (MCP) doesn't make the device safe — you still scan it, limit what it can touch, and don't run it as admin.

**Why it matters:** Shows maturity beyond the hype. *"A standard connector doesn't mean a trusted tool — I vet servers, grant least privilege, sandbox execution, and treat resource content as untrusted input that could carry injection."* (→ [[14-safety-guardrails]])

---

## 10. MCP vs. A2A (agent↔tools vs. agent↔agent)

**Definition:** **MCP** connects an agent to **tools/data** (vertical: model reaching down to capabilities). **A2A** (Agent-to-Agent) connects **agents to each other** (horizontal: peers delegating). Different layers, often used together.

**Example:** MCP is an employee using the **company tools** (email, database). A2A is that employee **delegating to a coworker**. One reaches for a tool; the other hands off to a peer.

**Why it matters:** A clean boundary that prevents a common mix-up. *"MCP is agent-to-tools; A2A is agent-to-agent — I use MCP to give one agent capabilities and A2A to let agents collaborate."* (→ [[13-a2a-agent-to-agent]])

---

## Quick misconceptions to avoid
- ❌ "MCP is a model or a framework." → It's an **open protocol** (a standard), not a model or library.
- ❌ "A skill is just a tool." → A tool is one callable; a **skill is procedural know-how** that may orchestrate tools.
- ❌ "Put every capability in the system prompt." → That bloats context; **skills load on demand**.
- ❌ "A standard connector means it's safe." → Vet servers, least-privilege, sandbox — resource content can carry **injection**.
- ❌ "MCP and A2A are the same." → MCP = agent↔**tools**; A2A = agent↔**agents**.
- ❌ "You must build a server from scratch." → Reuse community servers; build your own only to expose *your* systems.

---
_Related: [[07-agents-tool-use]] · [[13-a2a-agent-to-agent]] · [[09-context-engineering]] · [[14-safety-guardrails]] · [[11-langchain-langgraph]] · [[24-fine-tuning]]_
