# Evaluation — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> RAG-specific eval overlaps with [[05-rag]]; production eval with [[16-monitoring-and-debugging]].

---

## 1. Why eval is the whole game (a demo vs a product)

**Definition:** **Evaluation** is measuring output quality systematically so you can improve it and ship safely. Without it you're guessing.

**Example — cooking for guests:** a demo is tasting one dish once and saying "yum." A restaurant tastes **every** dish against a recipe standard, every service. If you can't measure quality, you can't tell whether tonight's change made the food better or worse.

**Why it matters:** *"If you can't measure it, you can't improve it — and you definitely can't ship it responsibly. Eval is what turns a demo into a product."* This mindset is table stakes for any applied AI role.

---

## 2. The eval toolkit (five layers)

**Definition:** No single metric — you layer them:
1. **Task metrics** — hard numbers for the task type.
2. **LLM-as-judge** — a model scores outputs against a rubric.
3. **Human eval** — the gold standard for subjective quality.
4. **Adversarial / red-team** — how gracefully it fails on edge cases.
5. **Regression testing** — a golden set re-run on every change.

**Why it matters:** *"I evaluate in layers — automatic metrics for speed, LLM-judge for scale, human eval for sign-off, plus adversarial and regression suites."* Naming the layers signals you've actually shipped.

---

## 3. Task metrics (match the metric to the job)

**Definition:** Pick metrics that fit the task and the business goal:

| Task | Metrics |
|---|---|
| Classification | Accuracy, **Precision / Recall / F1** |
| Summarization | ROUGE, **BERTScore** |
| Translation | BLEU, COMET |
| Retrieval | Precision@k, Recall@k, MRR, nDCG |

**Example — a smoke detector:** **precision** = "when it alarms, is it really a fire?" **recall** = "of all real fires, how many did it catch?" A detector that never alarms has perfect precision but catches nothing; one always alarming catches everything but is useless. **F1** balances the two.

**Why it matters:** *"I choose metrics that align with the business cost — high recall when missing a case is expensive (fraud, safety), high precision when false alarms are costly."*

---

## 4. LLM-as-judge (a model grades the model)

**Definition:** Use a strong LLM to score another model's outputs (helpfulness, accuracy, safety) against a **structured rubric**. Fast and scalable for iteration.

**Example — an exam grader with a rubric:** a human grader with a clear rubric ("2 pts for citing the source, 2 for correctness…") is consistent; without a rubric, grades wander. Give the LLM-judge the same explicit rubric and its scores stabilize.

**Why it matters:** It's how you iterate quickly without waiting on humans — but it has biases (see next). Power move: *"LLM-as-judge for fast iteration loops, human eval for final sign-off."*

---

## 5. LLM-judge biases (and how to tame them)

**Definition:** Judges have known biases: **position bias** (favoring the first/last option), **verbosity bias** (longer = "better"), **self-preference** (favoring their own style). Tame with:
- **Rubric + few-shot anchors** — define what each score means.
- **Pairwise comparison** (A vs B) instead of absolute scores.
- **Position swap** — run both orders, average.
- **Ensemble / stronger judge** — multiple judges vote.

**Example:** a judge who always picks whoever spoke last isn't judging content. Swapping the order and averaging cancels that out — like alternating who presents first in a debate.

**Why it matters:** Shows depth. *"I don't trust a raw judge score — I use a rubric, pairwise comparisons, and position-swapping to control bias."*

---

## 6. RAG evaluation — the RAG Triad

**Definition:** Evaluate retrieval and generation **separately**:
- **Context relevance** — did we fetch the right chunks?
- **Groundedness / faithfulness** — is the answer supported by the context (no hallucination)?
- **Answer relevance** — does it address the question?

**Example:** faithful-but-irrelevant = a perfectly accurate answer to a question nobody asked. Relevant-but-ungrounded = on-topic but made-up. You need both, so you score both legs.

**Why it matters:** *"A RAG system can pass one leg and fail another, so a single score hides the failure — I score the whole triad."* Tools: **RAGAS**, LangSmith. (→ [[05-rag]])

---

## 7. The golden dataset (your answer key)

**Definition:** A curated, **versioned** set of input → expected-output (or scoring criteria) pairs that represents real usage. It's the yardstick every change is measured against.

**Example:** a teacher's **answer key**. Without it, every grader marks differently and you can't compare this week's model to last week's. You keep the key current by adding new tricky cases sampled from real traffic.

**Why it matters:** *"I build a golden set from real queries plus known edge cases, version it, and grow it by sampling production — it's the backbone of regression testing."*

---

## 8. Adversarial & red-team testing

**Definition:** Deliberately attack the system — jailbreaks, prompt injection, out-of-domain inputs, ambiguous or offensive prompts — and check it **fails gracefully** (refuses, abstains, stays safe).

**Example:** crash-testing a car. You don't ship because it drives fine on a sunny road; you slam it into walls first. Same here — you probe the nasty inputs before users find them.

**Why it matters:** Ties eval to safety. *"I red-team for jailbreaks and injection and grade how gracefully it fails, not just accuracy on happy-path inputs."* (→ [[14-safety-guardrails]])

---

## 9. Regression testing & eval in CI

**Definition:** On **every** prompt/model/pipeline change, re-run the golden set + task metrics + judge, and **gate the deploy** on a quality threshold. Track scores over time; alert on drops.

**Example:** unit tests for AI. Just as code CI blocks a merge that breaks a test, eval CI blocks a prompt tweak that quietly drops faithfulness from 0.9 to 0.7.

**Why it matters:** Prompt changes silently regress — a fix for one case breaks three others. *"I put eval in CI with a quality gate so no change ships that drops the score below threshold."* Bridges to [[16-monitoring-and-debugging]] (online eval).

---

## 10. Offline vs online eval

**Definition:** **Offline** = before deploy, on your golden set. **Online** = in production, on live traffic (A/B tests, thumbs up/down, task-completion rate, sampling live outputs back into the golden set).

**Example:** offline = a rehearsal with a script; online = opening night with a real audience reacting. You need both — rehearsal catches obvious breaks, the live audience reveals what you couldn't script.

**Why it matters:** *"Offline eval gates the release; online eval (A/B + feedback + sampled traces) tells me if it actually works for users, and feeds new cases back into the golden set."*

---

## Quick misconceptions to avoid
- ❌ "One overall score is enough." → Score the **components** (esp. the RAG triad) or you hide failures.
- ❌ "LLM-judge scores are objective." → They have **position/verbosity/self** biases — control them.
- ❌ "High accuracy = good model." → Depends on precision vs recall and the **business cost** of each error.
- ❌ "Eval is a one-time step before launch." → It's **continuous** — regression in CI + online monitoring.
- ❌ "Human eval is too slow to bother." → Use it for **final sign-off**; judge/metrics for the fast loop.

---
_Related: [[05-rag]] · [[16-monitoring-and-debugging]] · [[14-safety-guardrails]] · [[02-llm-foundations]] · [[24-fine-tuning]]_
