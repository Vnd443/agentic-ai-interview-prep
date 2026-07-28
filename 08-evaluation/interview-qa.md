# Evaluation — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q06 (Evaluation)**.

---

## Q1. How do you evaluate an LLM's output quality? ⭐ (guide Q06)

**Why they ask:** If you can't measure quality, you can't improve it. Rigorous eval before shipping is table stakes.

**Ideal answer — the toolkit:**
1. **Task-specific metrics** — classification: accuracy/F1/precision-recall; summarisation: ROUGE, BERTScore; translation: BLEU, COMET; generation: perplexity/coherence. Pick metrics that align with the *business goal*.
2. **LLM-as-judge** — a stronger model scores a weaker model's outputs on helpfulness/accuracy/safety. Scales for fast iteration. Use a **structured rubric** to reduce judge variance.
3. **Human evaluation** — gold standard for subjective quality. A/B tests, 👍/👎, time-on-task, task-completion rate; annotation pipelines for systematic review.
4. **Adversarial testing** — red-team edge cases, jailbreaks, out-of-domain inputs; evaluate how *gracefully* it fails.
5. **Regression testing** — keep a suite of known-good input/output pairs; re-run on every prompt/model/pipeline change to catch regressions before prod.

**🔑 Power move:** *"LLM-as-judge for fast iteration loops, human eval for final sign-off"* — shows you get the speed-vs-quality trade-off.

**Follow-ups:**
- How do you reduce LLM-judge bias/variance? → rubric, few-shot anchors, pairwise comparison, position-swap, ensemble judges.
- What are RAGAS metrics? → faithfulness, answer relevance, context precision/recall.
- How do you build a "golden" eval set?

---

## Q2. What is RAGAS / how do you evaluate a RAG system specifically?

**Ideal answer:** Evaluate retrieval and generation separately. **Retrieval:** context precision & recall (did we fetch the right chunks?). **Generation:** **faithfulness** (is the answer grounded in retrieved context, i.e. no hallucination?) and **answer relevance** (does it address the question?). RAGAS uses an LLM to score these without full human labels, good for CI-style regression checks.

**Follow-ups:** Faithfulness vs. relevance — a system can be one but not the other. How?

---

## Q3. How do you set up evaluation in CI so quality doesn't regress?

**Ideal answer:** Maintain a versioned golden set. On every change, run task metrics + LLM-judge + regression suite; gate the deploy on a quality threshold. Track scores over time; alert on drops. Sample live traffic into the golden set to keep it representative.

---

## Your notes / STAR angle
- _TODO: an eval harness you built and what it caught._
