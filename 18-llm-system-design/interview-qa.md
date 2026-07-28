# LLM System Design — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q09 (System Design)**.

---

## Q1. Design a system that summarises 10,000 documents daily. ⭐ (guide Q09)

**Why they ask:** System design tests thinking *at scale* — not the model, but the infra, cost, and reliability around it.

**Ideal answer — start with the math, then the architecture:**

**Back-of-envelope first:** 10K docs/day × ~2K tokens ≈ **20M tokens/day**. State the rough $/day out loud — it signals production thinking.

1. **Architecture** — **message queue** (SQS/Kafka) → **worker pool** → **LLM API** → **result store** (S3/DB). Docs enter the queue, workers pull, call the LLM, store results. **Decouple ingestion from processing.**
2. **Chunking** — docs over the context window use **map-reduce**: summarise each chunk, then summarise the summaries. Very long docs → **hierarchical** summarisation.
3. **Cost engineering** — **batch APIs** (~50% cheaper), **model routing** (small model for simple docs, large for complex), and **caching** for duplicate/near-duplicate docs.
4. **Reliability** — **dead-letter queues** for failures, retries with exponential backoff, **idempotent** processing (re-runs don't duplicate), health checks, **auto-scale workers on queue depth**.
5. **Quality assurance** — sample 1–2% of outputs daily for human review; track summary length, extractive density, factual consistency; alert on drift from baseline.

**🔑 Power move:** Always open with the back-of-envelope math: *"10K × 2K = 20M tokens/day ≈ $X"* — shows you think about real constraints.

**Follow-ups:**
- How do you auto-scale workers? → queue depth / lag target.
- What makes processing idempotent? → dedupe key = content hash; upsert by doc id.
- Real-time vs. batch trade-off? Where does latency budget bite?

---

## Q2. General framework for any LLM system design question?

**Ideal answer:**
1. **Clarify** requirements, scale, latency SLA, quality bar, budget.
2. **Back-of-envelope** tokens → cost/QPS.
3. **Data flow**: ingest → process → store → serve; decouple with queues.
4. **Model strategy**: routing, batching, caching.
5. **Reliability**: retries, DLQ, idempotency, autoscaling.
6. **Eval & monitoring**: sampling, quality scores, drift alerts.
7. **Safety**: guardrails, PII, HITL where stakes are high.

**🔑 Power move:** Narrate the trade-off at each step ("I'd choose X over Y because…").

---

## Q3. How do you design for latency (a real-time chatbot vs. batch analytics)?

**Ideal answer:** Real-time: smaller/faster model, streaming responses, prompt caching, tight `max_tokens`, aggressive timeouts + fallback. Batch: maximise throughput with batch APIs and large context, tolerate higher per-item latency. Pick the model tier and serving mode from the SLA, not the other way around.

---

## Your notes / STAR angle
- _TODO: a system you designed — scale numbers, queue/worker choices, cost._
