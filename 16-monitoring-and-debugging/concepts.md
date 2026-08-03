# Monitoring & Production Debugging — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> Proves you've **shipped**, not just prototyped. Eval overlaps with [[06-evaluation]]; cost dashboards with [[15-cost-optimization]]; guardrail logging with [[14-safety-guardrails]].

---

## 1. Why "works in test, fails in prod" is the whole topic

**Definition:** An LLM feature can pass every offline test and still fail live, because production inputs, load, and conversations don't look like your test set. Monitoring + debugging is how you catch and diagnose that gap.

**Example — a car that passes the garage inspection but stalls on the highway:** the garage is calm and controlled; the highway has rain, potholes, and other drivers. You don't trust the inspection alone — you watch the dashboard while driving.

**Why it matters:** This is the question that separates shippers from notebook-runners. *"Offline green doesn't mean production-safe — real traffic is messier, so I instrument and monitor rather than trust the test set."*

---

## 2. Data drift (the world moved, your model didn't)

**Definition:** **Data drift** = production inputs diverge from what you built/tested on — new slang, typos, topics, formats. The model looks fine but quality quietly erodes.

**Example:** a **dress code written in 2010** — still "enforced," but everyone now wears things it never anticipated, so it stops matching reality. The rule didn't change; the world did.

**Why it matters:** It's the top silent failure. Detect it: track **input distribution** (embedding-space shift, topic/keyword shift, out-of-vocabulary rate) and **alert on drift**. *"I monitor input distributions and alert on drift — most 'the model got worse' reports are the inputs changing, not the model."*

---

## 3. Train–test leakage (scores too good to be true)

**Definition:** **Leakage** = information from the test set (or the future) sneaks into training/eval, so offline scores are inflated and don't hold up live. Common forms: overlap between train and eval, or **temporal leakage** (evaluating on data from before the training cutoff).

**Example:** a student who **saw the exam answers beforehand** — aces the test, flunks the real world. The grade was never measuring real ability.

**Why it matters:** *"If offline scores look suspiciously high, I re-audit the eval set for overlap and temporal leakage before I trust it."*

---

## 4. Evaluation mismatch (measuring the wrong thing)

**Definition:** Offline metrics (F1, BLEU, ROUGE, perplexity) may not track what **users** actually value. A high score that doesn't move user satisfaction is a mismatch.

**Example:** **teaching to the test** — a school with rising test scores but graduates who can't do the job. The number went up; the thing you cared about didn't.

**Why it matters:** *"I check that my offline metric correlates with real user value — if BLEU is up but thumbs-down is up too, the metric is lying."* Fix by building evals that reflect real satisfaction (→ [[06-evaluation]]).

---

## 5. Prompt sensitivity (users don't phrase like your tests)

**Definition:** Small wording changes swing outputs, and **real user phrasing varies far more** than your handful of clean test prompts.

**Example:** a **voice assistant that only understands the demo phrasing** — "play jazz" works, but "put on something chill" flops. Real users say the second kind.

**Why it matters:** *"My test prompts are too clean — I sample real phrasings into my eval set so prompt sensitivity shows up before users find it."*

---

## 6. Latency, timeouts & concurrency

**Definition:** Under real load you hit **rate limits, timeouts, and degradation from concurrency** that never appear in single-user testing. Track **p50/p95/p99 latency**, error/timeout rate, and profile under realistic traffic.

**Example:** a **restaurant kitchen that's fine cooking one order** but collapses when 50 tickets land at once. You have to stress-test at dinner-rush volume, not by cooking one plate.

**Why it matters:** *"I profile under realistic concurrency and watch tail latency — p99, not the average — because that's what users actually feel and what trips timeouts."*

---

## 7. Context-window overflow (the silent truncation)

**Definition:** Production conversations grow; when they exceed the context window, information is **silently dropped**, causing hard-to-diagnose degradation.

**Example:** a **short-order cook who can only remember the last 5 items** — order 6 and the first quietly falls off the pad. Nothing errors; the dish is just wrong.

**Why it matters:** *"I set token-count alarms and cap conversation length with summarization, because overflow degrades quality without throwing an error."* (→ [[09-context-engineering]])

---

## 8. Three layers of monitoring (the framework)

**Definition:** A clean structure for the "how do you monitor?" question:
1. **Input-distribution tracking** — detect drift and out-of-scope inputs.
2. **Output-quality scoring** — a rolling **LLM-as-judge** score on a sample, plus schema/guardrail pass rates.
3. **User-feedback loops** — explicit 👍/👎, thumbs, reports, task-completion.

**Example:** a **hospital monitoring a patient** — vitals coming in (inputs), lab results on samples (output quality), and the patient telling you how they feel (feedback). You need all three to know their real state.

**Why it matters:** *The* power-move framework. *"I set up three layers — input distribution, output quality, and user feedback — then narrow the failure mode systematically instead of guessing."*

---

## 9. What to actually log & alert on

**Definition:** Log inputs/outputs (**PII-redacted**), latency (p50/p95/p99), error/timeout rate, token usage & cost, refusal/flag rate, and a sampled quality score. **Alert on deltas from baseline**, not absolute thresholds. Stamp every request with **prompt & model version**.

**Example:** a **home smoke alarm vs. a thermometer** — you don't want a constant temperature readout; you want an alert when it *changes* dangerously from normal. Baselines beat fixed thresholds.

**Why it matters:** *"I alert on deltas from a rolling baseline and version-stamp every request — so when quality shifts I can see exactly what changed and when."* (→ [[15-cost-optimization]])

---

## 10. Reproducing an incident (the un-reproducible bad answer)

**Definition:** When a user reports a bad answer you can't reproduce: **pull the exact logged request** — prompt + retrieved context + params + **model/prompt version** — and replay it at **temperature 0**. Usual suspects: non-determinism (temperature), a since-changed prompt/model version, or stale retrieval. Diff against the current pipeline.

**Example:** an **airplane black box** — you don't re-fly the crash from memory; you replay the exact recorded inputs to see what happened. Request-level logging is your black box.

**Why it matters:** *"I can only debug what I logged — request-level logging with version stamps lets me replay the exact failure at temperature 0 instead of guessing."*

---

## 11. LLMOps vs. MLOps (what's new)

**Definition:** **LLMOps** = MLOps plus the LLM-specific concerns: **prompt versioning**, **non-determinism**, **eval-in-production** (quality isn't a single accuracy number), and **observability/tracing** across multi-step agent runs (spans per step, e.g. **LangSmith / Langfuse**).

**Example:** MLOps is **managing a factory line** (versioned models, pipelines); LLMOps adds **managing a writer's room** — the "code" includes prompts that change, outputs vary run to run, and quality is judged, not just measured.

**Why it matters:** *"LLMOps adds prompt version control, handling non-determinism, and eval-in-prod to classic MLOps — plus tracing spans over an agent's whole decision path."* (→ [[06-evaluation]], [[07-agents-tool-use]])

---

## 12. Safe rollout — canary, rollback, version tracking

**Definition:** Ship changes behind a **canary** (small % of traffic first), keep the ability to **roll back instantly**, and track **which prompt/model version** was live when quality shifted.

**Example:** a **food taster** — a new recipe goes to a few plates before the whole banquet, and you can pull it immediately if anyone reacts.

**Why it matters:** *"I canary prompt/model changes on a slice of traffic with instant rollback and version tracking — a prompt tweak is a deploy and deserves the same safety."* (→ [[06-evaluation]] eval-in-CI, [[19-iac-and-devops]])

---

## Quick misconceptions to avoid
- ❌ "Passing offline tests means it's production-ready." → Real traffic drifts, varies, and scales differently — **monitor live**.
- ❌ "The model got worse." → Usually **data drift** (inputs changed), not the model.
- ❌ "High offline metric = happy users." → Watch for **eval mismatch**; correlate the metric with real satisfaction.
- ❌ "Average latency is fine." → Users feel **p95/p99** tail latency and timeouts.
- ❌ "Overflow throws an error." → Context overflow **silently drops** info — set token alarms.
- ❌ "Alert on fixed thresholds." → Alert on **deltas from baseline**; absolute thresholds miss gradual drift.
- ❌ "A prompt change isn't a deploy." → It is — **version, canary, and be able to roll back**.

---
_Related: [[06-evaluation]] · [[15-cost-optimization]] · [[14-safety-guardrails]] · [[07-agents-tool-use]] · [[09-context-engineering]] · [[19-iac-and-devops]] · [[11-langchain-langgraph]]_
