# Cost Optimization — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q02 (Cost Optimisation)**.

---

## Q1. How can you reduce the cost of running LLMs? ⭐ (guide Q02)

**Why they ask:** Companies want engineers who build *without burning the budget*.

**Ideal answer — the levers, roughly in impact order:**
1. **Model selection & routing** — use the smallest model that clears the bar; route simple queries to a cheap/fast model (e.g. Haiku or a fine-tuned small model), escalate only hard ones. *Alone this can cut 60–80%.*
2. **Prompt optimisation** — shorter prompts = fewer tokens. Compress system prompts, drop redundant few-shot examples.
3. **Caching** — **semantic cache** keyed by embedding similarity serves near-identical questions from cache; also use provider **prompt caching**.
4. **Batching & async** — batch non-urgent jobs; many APIs give ~50% off batch endpoints. Run summarisation/classification overnight, not real-time.
5. **Fine-tuning for efficiency** — a small specialised fine-tune can beat a large general model at a fraction of inference cost; upfront training pays back fast.
6. **Output length control** — set `max_tokens`; don't let it write 2000 tokens when 200 will do. Structured output = exactly what you need.

**🔑 Power move:** Quantify. *"Model routing cut our API costs 70% while holding 95% of quality."* Numbers win.

**Follow-ups:**
- How do you decide the routing threshold? → cheap classifier or heuristic on query complexity; measure escalation rate.
- Risks of semantic caching? → stale answers, false cache hits; tune similarity threshold + TTL.
- Batch vs. real-time trade-off (latency SLA)?

---

## Q2. How do you estimate the monthly cost of an LLM feature before building it?

**Ideal answer:** Back-of-envelope: `requests/day × (avg input tokens + avg output tokens) × price/token`. Split input vs. output pricing (output is usually 3–5× input). Add a buffer for retries and system-prompt overhead. Then identify the biggest term and attack it (usually output tokens or model tier).

**🔑 Power move:** Do the math out loud — it signals production thinking.

**Follow-ups:** How does prompt caching change the math? Where does a reranker/embedding cost fit?

---

## Q3. Cost vs. quality — how do you avoid over-optimizing?

**Ideal answer:** Set a quality floor (an eval suite) first, then cut cost only while staying above it. Track cost-per-successful-task, not cost-per-call — a cheap model that fails and triggers a retry+escalation is not cheap.

---

## Your notes / STAR angle
- _TODO: a cost-reduction story with before/after numbers._
