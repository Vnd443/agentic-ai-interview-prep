# Context Engineering — Project Ideas

Ranked by interview impact. Each should give you a **number** and a **story**.

---

### ⭐ 1. Compaction engine for a long-running agent
**Build:** Wrap an agent loop so that when the message history crosses a token threshold (say 75% of the window), it summarizes older turns into a **structured** memento — `{decisions, facts, open_todos, discarded}` — and replaces the raw history with it, keeping the system prompt + last N turns verbatim.
**Demo/number:** "Ran a 40-step task; compaction cut per-step tokens ~60% and let it finish within the window instead of erroring out."
**Interview use:** the canonical context-engineering story. Ties to [[22-agent-orchestration]] loop control.

---

### ⭐ 2. Two-tier memory (short-term + long-term recall)
**Build:** Add a long-term store (vector DB). At session end, summarize into episodic + semantic memories and persist. At session start, retrieve only memories relevant to the new query. Show the same agent "remembering" a fact across sessions **without** replaying old transcripts.
**Number:** "Cross-session continuity with ~90% fewer tokens than replaying full history."
**Interview use:** Write + Select levers; memory types.

---

### 3. "Lost in the middle" harness
**Build:** Insert a known fact at position start / middle / end of a long context and measure recall accuracy at each. Reproduce the degradation curve.
**Number:** "Middle-position recall dropped to X% vs Y% at the edges — so I now put critical context top and bottom."
**Interview use:** proves you've *measured* a failure mode, not just read about it.

---

### 4. Cache-aware context assembler
**Build:** A prompt builder that orders context as system → tools → static docs → [cache breakpoint] → dynamic turns, and refuses to put volatile tokens (timestamps, request IDs) in the prefix. Measure cost with vs. without.
**Number:** "Restructuring for cache hits cut input cost ~85% on repeated calls."
**Interview use:** links context engineering to [[09-cost-optimization]].

---

### 5. Tool-output trimmer
**Build:** Middleware that summarizes/truncates large tool results (API JSON dumps, search results) before they enter context, keeping only fields the agent uses.
**Number:** "Trimming tool outputs removed ~50% of context tokens with no accuracy loss."
**Interview use:** shows you know the biggest silent token hog.

---

## How to talk about these
Lead with the **failure you were fixing** (window overflow / degraded answers / cost), the **lever you applied** (Write/Select/Compress/Isolate), and the **measured result**. Always end with a number.

_TODO: pick one, build it, record the real numbers here._
