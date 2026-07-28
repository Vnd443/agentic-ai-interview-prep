# Production Debugging — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q03 (Production Debugging)**.

---

## Q1. Your model works in testing but fails in production. What's going wrong? ⭐ (guide Q03)

**Why they ask:** It proves you've actually *shipped*, not just run notebooks.

**Ideal answer — systematic list of failure modes:**
1. **Data drift** — prod inputs are messier (slang, typos, edge cases, new topics). Monitor input distributions; alert on drift.
2. **Train–test leakage** — suspiciously good offline scores? Re-audit for overlap / temporal leakage in the eval set.
3. **Evaluation mismatch** — offline metrics (F1, BLEU, perplexity) may not track what users care about. Build evals that reflect real satisfaction.
4. **Prompt sensitivity** — real user phrasing varies far more than your test prompts; tiny wording changes swing outputs.
5. **Latency & timeouts** — rate limits, timeouts, degradation under concurrent load. Profile under realistic traffic.
6. **Context-window overflow** — prod conversations grow; overflow silently drops info → hard-to-diagnose degradation.

**🔑 Power move (framework):** *"I'd set up three layers of monitoring — input-distribution tracking, output-quality scoring, and user-feedback loops — then systematically narrow the failure mode."*

**Follow-ups:**
- How do you detect drift concretely? → track embedding-space distribution, keyword/topic shift, out-of-vocab rate.
- How do you catch context overflow before users do? → token-count alarms, conversation-length caps + summarisation.
- What's your rollback / canary strategy?

---

## Q2. How do you monitor an LLM system in production?

**Ideal answer:** Log inputs/outputs (PII-redacted). Track latency (p50/p95/p99), error/timeout rate, token usage & cost, refusal/flag rate, and a rolling **quality score** (LLM-as-judge on a sample) plus explicit user feedback (👍/👎). Alert on deltas from baseline, not absolute thresholds.

**Follow-ups:** What sampling rate for LLM-judge scoring? How do you close the feedback loop into evals?

---

## Q3. A user reports a bad answer you can't reproduce. What now?

**Ideal answer:** Pull the exact logged request (prompt + retrieved context + params + model version). Non-determinism (temperature), a since-changed prompt/model version, or a stale retrieval are usual suspects. Reproduce with the logged inputs at temperature 0; diff against current pipeline. This is why request-level logging + version stamping matters.

---

## Your notes / STAR angle
- _TODO: a "worked in dev, broke in prod" war story from IBM._
