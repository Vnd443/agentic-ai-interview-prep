# MCP & Custom Agent Skills — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## MCP core
- Problem: N×M bespoke tool integrations —
- MCP = open protocol (USB-C for LLM tools) —
- Decouples tool impl from the agent —
- Client–server model (host runs a client per server) —

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

## MCP vs A2A
- MCP = agent ↔ tools (vertical) —
- A2A = agent ↔ agent (horizontal) —

## Numbers / facts worth quoting
- 
