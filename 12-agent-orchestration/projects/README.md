# Multi-Agent Orchestration — Project Ideas

Ranked by interview impact. Each should give you a **number** and a **story**. This is your differentiator turf — build at least one flagship.

---

### ⭐ 1. Orchestrator–worker research assistant
**Build:** A lead agent decomposes a question into sub-questions, spawns N worker agents (each own context + search tool) in parallel, then synthesizes with citations. Add worker caps + a budget guard.
**Number:** "Parallel workers cut wall-clock ~3× vs sequential; capped at 5 workers / $X budget per run."
**Interview use:** the flagship pattern. Emphasize **context isolation** as the real reason.

---

### ⭐ 2. Loop-control harness (the reliability story)
**Build:** Take any agent loop and add the full guard suite: max-iteration cap, token budget, per-step timeout, explicit `finish` tool, stall detection (repeated-action break), retry-with-backoff. Log why each run terminated.
**Number:** "Runaway loops dropped to 0; median run finished in 6 iterations, hard cap at 20."
**Interview use:** loop control is a guaranteed follow-up — have a real story.

---

### 3. Evaluator–optimizer (self-improving output)
**Build:** Generator produces output (e.g. code or a translation); evaluator agent scores it against explicit criteria; loop until pass or cap. Compare quality with vs without the loop.
**Number:** "Eval-optimizer raised pass@1 from X% to Y% at 2 extra calls per task."
**Interview use:** shows you know *when* iteration is worth the cost. Ties to [[06-evaluation]].

---

### 4. Routing layer (cheap-vs-capable model router)
**Build:** A classifier routes each request to a small/cheap model or a frontier model based on difficulty. Measure cost + quality.
**Number:** "Routing sent ~70% of traffic to the small model → ~60% cost cut at ~97% of quality."
**Interview use:** doubles as a [[15-cost-optimization]] story.

---

### 5. Handoff / swarm support bot
**Build:** A triage agent hands off to specialized agents (billing, tech, account) with context transfer. Show control moving between peers with no central synthesizer.
**Number:** "Handoff routing resolved X% end-to-end without escalation."
**Interview use:** contrasts decentralized (swarm) vs centralized (orchestrator).

---

## How to talk about these
Open with **why one agent wasn't enough** (parallelism / specialized context / overflow), name the **topology**, then the **control guards** and the **measured result**. Always end with a number, and always mention the single-agent baseline you compared against.

_TODO: pick one (ideally #1 or #2), build it, record the real numbers here._
