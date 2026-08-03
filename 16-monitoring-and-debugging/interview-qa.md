# Monitoring & Production Debugging — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q03 (Production Debugging)**.

---

**Q1. Your model works in testing but fails in production. What's going wrong? ⭐ (guide Q03)**

I walk a systematic checklist of failure modes rather than guessing. Data drift — production inputs are messier than my test set, with slang, typos, and new topics — so I monitor input distributions and alert on shift. Train-test leakage — if offline scores were suspiciously high, I re-audit the eval set for overlap or temporal leakage. Evaluation mismatch — offline metrics like F1 or BLEU may not track what users actually value, so I check the metric correlates with real satisfaction. Prompt sensitivity — real users phrase things far more variably than my clean test prompts, and small wording changes swing outputs. Latency and concurrency — rate limits, timeouts, and degradation under real load that never show up single-user. And context-window overflow — production conversations grow and silently drop information. The power move is the framework: I set up three layers of monitoring — input-distribution tracking, output-quality scoring, and user-feedback loops — then narrow the failure mode systematically.

---

**Q2. How do you monitor an LLM system in production?**

I log inputs and outputs with PII redacted, and track latency at p50, p95, and p99, error and timeout rate, token usage and cost, and refusal or flag rate. For quality I run a rolling LLM-as-judge score on a sample of traffic plus explicit user feedback like thumbs up and down. The key discipline is alerting on deltas from a rolling baseline rather than absolute thresholds, because gradual drift never trips a fixed line. And I version-stamp every request with the prompt and model version, so when quality shifts I can see exactly what changed and when. The mental model is three layers — input distribution, output quality, user feedback — so I'm watching the vitals, the lab samples, and what the user actually reports.

---

**Q3. A user reports a bad answer you can't reproduce. What do you do?**

I pull the exact logged request — the prompt, the retrieved context, the parameters, and the model and prompt version that were live at that moment — and replay it at temperature 0. The usual suspects are non-determinism from a non-zero temperature, a prompt or model version that has since changed, or a stale retrieval that returned different chunks than it does now. Once I reproduce it with the logged inputs, I diff against the current pipeline to find what moved. This is exactly why request-level logging with version stamping matters — I can only debug what I logged, and it's my black box for replaying the incident instead of guessing.

---

**Q4. How do you detect data drift concretely?**

I track the distribution of inputs, not just individual requests. Concretely that's watching for shift in embedding space — are incoming queries landing in different regions than my baseline — plus topic and keyword shift, and the out-of-vocabulary or new-entity rate. When those move beyond a threshold from baseline, I alert. The reason I lead with drift is that most "the model got worse" reports are actually the inputs changing while the model stayed the same, so measuring the inputs tells me whether to look at the data or the model.

---

**Q5. What does LLMOps add over classic MLOps?**

MLOps already covers versioned models, pipelines, and deployment. LLMOps adds the LLM-specific concerns on top. Prompt versioning, because the prompt is part of the "code" and changes constantly. Handling non-determinism, since the same input can give different outputs run to run. Eval-in-production, because quality isn't a single accuracy number — I score a sample of live outputs continuously. And observability with tracing spans across a multi-step agent run, using something like LangSmith or Langfuse, so I can see the whole decision path. My rule that ties it together: a prompt change is a deploy, so it gets version tracking, a canary on a slice of traffic, and instant rollback, just like a code change.

---

## Your notes / STAR angle
- _TODO: a "worked in dev, broke in prod" war story — the failure mode, how you diagnosed it, and what monitoring you added so it wouldn't recur._
