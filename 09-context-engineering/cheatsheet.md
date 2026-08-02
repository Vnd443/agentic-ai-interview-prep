# Context Engineering — Cheatsheet

## One-liner
> "Prompt engineering is what you *say*; context engineering is everything the model can *see* when it acts — assembled, ordered, compressed, and isolated across a whole task."

## The 4 levers (memorize)
- **Write** — state *outside* the window (scratchpad, files, long-term memory).
- **Select** — pull the *right* info *in* (RAG, memory recall, expose only needed tools).
- **Compress** — shrink what you keep (summarize, compact, prune, trim tool output).
- **Isolate** — separate contexts (sub-agents, sandboxes) so they don't contaminate.

## Context is a budget
Every token competes. More ≠ better. Window ≈ RAM: page in high-signal, evict noise.
Biggest silent hog: **raw tool-call outputs** — trim them.

## Memory
- Lifetime: **short-term** (in-window) vs **long-term** (external store).
- Type: **episodic** (events) · **semantic** (facts) · **procedural** (how-to / instructions).
- Pattern: end of session → summarize to store; next session → retrieve only relevant.

## Compaction
Replace old context with a **structured** summary (decisions / facts / todos / open Qs) at ~70–80% window fill. Keep system prompt + recent turns verbatim. Risk: lossy → use structure, not prose.

## Failure modes (say these)
- **Context rot** — quality drops as window fills (even below max). → keep it lean.
- **Lost in the middle** — middle content recalled worst. → critical info at **top & bottom**.
- **Context poisoning** — a hallucination gets memorized as fact. → validate before persisting.
- **Distraction** — too much history drowns the task. → restate the goal near the end.
- **Clash** — contradictory sources. → de-dupe, prefer freshest, isolate.

## Prompt caching angle
Keep the **prefix stable** (system → tools → static docs → breakpoint → dynamic). No timestamps/nonces up top. ~90% cheaper cached reads. → ties to [[15-cost-optimization]].

## Power moves
- Name the **4 levers** unprompted when asked about long agent runs.
- Drop **"lost in the middle"** + **"context rot"** by name.
- "I summarize to a *structured* schema, not free text, so compaction doesn't silently drop a constraint."
- "Most context bugs are noise, not missing info — I trim tool outputs first."

## Don't confuse
- RAG ⊂ Select lever. Prompt engineering ⊂ context engineering. Context engineering = the umbrella.
