# MCP & Custom Agent Skills — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## MCP core
- Problem: N×M bespoke tool integrations —
- MCP = open protocol (USB-C for LLM tools) —
- Decouples tool impl from the agent —
- Client–server model (host runs a client per server) —

## Architecture (3 roles)
- Host = UI / front door (ChatGPT, Claude Desktop) —
- Client = per-server translator (host ↔ server) —
- Server = exposes tools/resources of an app (Gmail, DB) —
- Model discovers capabilities at run-time (no hardcoding) —

## Request lifecycle (5 steps) ⭐
- Plan (needs external action) —
- Discover (which servers + what they do) —
- Schema (read tool contract at run-time) —
- Auth (verify identity/permissions — OAuth) —
- Execute + reason (call tool → data → answer) —

## Server surface (3 primitives)
- Tools = actions —
- Resources = data / context —
- Prompts = reusable templates —

## Transports
- stdio (local subprocess, private) —
- HTTP (remote / shared) —

## Build vs reuse
- Existing servers (filesystem, GitHub, Slack, Postgres) —
- Build your own to expose internal systems —

## Skills
- SKILL.md = name + description + body —
- Loaded on demand (description is the trigger) —
- Capability without context bloat —

## Skill vs tool vs fine-tune vs prompt
- Tool = one callable action —
- Skill = procedural know-how, may orchestrate tools —
- Long prompt = always loaded (bloat) —
- Fine-tune = baked into weights (costly, slow) —
- Order: prompt → skill → fine-tune —

## Security
- Least privilege / sandbox / validation —
- HITL for high-stakes —
- Vet third-party servers (supply chain) —
- Resource content = untrusted (injection) —

## Enterprise value
- Scalability (change server, not the agent) —
- Security: RBAC (authorized data only) —
- Traceability: audit logs (what + why) —
- Graceful failure (report, don't crash) —

## Three pillars
- LLM = reasoning —
- RAG = knowledge —
- MCP = action / system access —

## MCP vs A2A
- MCP = agent ↔ tools (vertical) —
- A2A = agent ↔ agent (horizontal) —

## Numbers / facts worth quoting
- 
