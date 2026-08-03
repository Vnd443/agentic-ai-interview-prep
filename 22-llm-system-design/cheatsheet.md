# LLM System Design — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Open with this ⭐
- Back-of-envelope token math, out loud — volume × tokens × price = $/day —
- Clarify scale / latency SLA / quality bar / budget first —

## The framework (say in order)
- Clarify → token math → data flow → model strategy → reliability → eval → safety —

## Architecture
- Queue (SQS/Kafka) → worker pool → LLM → result store —
- Decouple ingestion from processing —
- Semantic cache → router → retrieval → LLM → guardrails → observe —

## Long docs
- Chunk + map-reduce (summarise chunks, then the summaries) —
- Hierarchical for very long inputs —

## Cost levers
- Batch APIs (~50% cheaper) —
- Model routing (small for easy, frontier for hard) —
- Caching (exact + semantic) for dupes —

## Reliability
- Retries w/ exponential backoff —
- Dead-letter queue for repeated failures —
- Idempotency (dedupe by content hash, upsert by id) —
- Autoscale workers on queue depth —

## Quality / drift
- Sample 1–2% outputs for review —
- Track length / factual consistency; alert on drift from baseline —

## Latency
- Real-time: fast model, streaming, tight max_tokens, timeout + fallback —
- Batch: throughput, batch APIs, large context, tolerate per-item latency —
- Pick tier + serving mode from the SLA —

## Safety
- Input/output guardrails, PII, HITL on high-stakes —

## Numbers / facts worth quoting
- 10K docs × 2K tokens = 20M tokens/day (the canonical opener) —
- Batch APIs ~50% cheaper —
- QA sampling 1–2% of daily outputs —
