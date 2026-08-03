# LLM System Design — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q09 (System Design)**.

---

**Q1. Design a system that summarises 10,000 documents daily. ⭐ (guide Q09)**

I'd start with the math, out loud: 10K docs/day at roughly 2K tokens each is about 20M tokens/day, and I'd state the rough dollars-per-day so it's clear I'm thinking about real constraints. Then the architecture: a message queue like SQS or Kafka feeds a worker pool that pulls documents, calls the LLM, and writes to a result store in S3 or a database — decoupling ingestion from processing so a spike fills the queue instead of dropping work. For documents over the context window I map-reduce: summarise each chunk, then summarise the summaries, going hierarchical when they're very long. On cost I lean on batch APIs, which are roughly half price for non-urgent work, model routing so a small model handles simple docs and a large one only the complex ones, and caching for duplicate or near-duplicate documents. For reliability I add a dead-letter queue for repeated failures, retries with exponential backoff, idempotent processing so re-runs don't duplicate — dedupe by content hash, upsert by doc id — and autoscale the workers on queue depth. And for quality I sample 1–2% of outputs daily for review, track things like summary length and factual consistency, and alert on drift from a baseline. The one-liner I'd open with is that 10K × 2K = 20M tokens/day ≈ $X — the back-of-envelope math signals production thinking before I've drawn a box.

---

**Q2. What's your general framework for any LLM system design question?**

I go in a fixed order so I never miss a layer. First clarify requirements — scale, latency SLA, quality bar, budget. Then back-of-envelope the tokens into cost and QPS. Then the data flow: ingest → process → store → serve, decoupled with queues. Then model strategy: routing, batching, caching. Then reliability: retries, dead-letter queues, idempotency, autoscaling. Then eval and monitoring: output sampling, quality scores, drift alerts. And finally safety: guardrails, PII handling, and human-in-the-loop where the stakes are high. The thing I do throughout is narrate the trade-off at each step — "I'd choose X over Y because…" — because system design is really testing judgment, not recall.

---

**Q3. How do you design for latency — a real-time chatbot vs. batch analytics?**

I design from the SLA rather than picking a model first. For a real-time chatbot I want a smaller, faster model, streaming responses so the user sees tokens immediately, prompt caching, a tight `max_tokens`, and aggressive timeouts with a fallback so a slow call doesn't hang the UX. For batch analytics I flip it: maximise throughput with batch APIs and large context windows, and happily tolerate higher per-item latency because nobody's waiting on a single result. So the latency budget decides the model tier and the serving mode — real-time optimises the next user's wait, batch optimises total items processed by the deadline.

---

## Your notes / STAR angle
- _TODO: a system you designed — scale numbers, queue/worker choices, cost._
