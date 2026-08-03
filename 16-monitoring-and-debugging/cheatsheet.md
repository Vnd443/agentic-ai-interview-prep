# Monitoring & Production Debugging — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Mindset
- Works in test, fails in prod —
- Offline green ≠ production-safe —

## Failure modes (the checklist)
- Data drift (inputs changed) —
- Train–test leakage (scores too good) —
- Eval mismatch (metric ≠ user value) —
- Prompt sensitivity (real phrasing varies) —
- Latency / timeouts / concurrency —
- Context-window overflow (silent) —

## Detecting drift
- Embedding-space distribution shift —
- Topic / keyword shift —
- Out-of-vocab rate —

## 3 layers of monitoring (framework)
- Input-distribution tracking —
- Output-quality scoring (sampled LLM-judge) —
- User-feedback loop (👍/👎, reports) —

## What to log / alert
- Inputs/outputs (PII-redacted) —
- Latency p50/p95/p99 —
- Error/timeout rate —
- Token usage + cost —
- Refusal / flag rate —
- Alert on deltas from baseline (not absolute) —
- Version-stamp every request —

## Reproducing an incident
- Pull exact logged request + context + params + version —
- Replay at temperature 0 —
- Suspects: non-determinism / changed version / stale retrieval —

## LLMOps (vs MLOps)
- Prompt versioning —
- Non-determinism —
- Eval-in-production —
- Tracing spans (LangSmith / Langfuse) —

## Safe rollout
- Canary (small % first) —
- Instant rollback —
- Prompt/model version tracking —
- Eval-in-CI before deploy —

## Numbers / facts worth quoting
- Watch p95/p99, not average latency —
