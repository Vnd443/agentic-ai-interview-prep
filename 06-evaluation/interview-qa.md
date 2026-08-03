# Evaluation — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q06 (Evaluation)**.

---

**Q1. How do you evaluate an LLM's output quality? ⭐ (guide Q06)**

I evaluate in layers because no single metric is enough. First, task-specific metrics: accuracy/precision/recall/F1 for classification, ROUGE or BERTScore for summarization, BLEU or COMET for translation — and I pick the metric that matches the business cost of an error. Second, LLM-as-judge: a strong model scores outputs against a structured rubric, which scales for fast iteration. Third, human evaluation as the gold standard for subjective quality and final sign-off — A/B tests, thumbs up/down, task-completion rate. Fourth, adversarial and red-team testing to see how gracefully it fails on jailbreaks and out-of-domain inputs. Fifth, regression testing: a versioned golden set re-run on every prompt/model/pipeline change so quality doesn't silently drift. The power move is the trade-off: LLM-as-judge for the fast iteration loop, human eval for the final sign-off.

---

**Q2. LLM-as-judge sounds circular — how do you trust the judge?**

I don't trust a raw judge score, because judges have known biases: position bias (favoring the first or last option), verbosity bias (longer looks better), and self-preference (favoring their own style). I control those with a structured rubric plus few-shot anchors that define what each score means, pairwise A-vs-B comparisons instead of absolute scores, position-swapping (run both orders and average), and sometimes an ensemble of judges that vote. So it's judge-with-guardrails for the fast loop, and I still keep humans for final sign-off on anything high-stakes.

---

**Q3. How do you evaluate a RAG system specifically?**

I use the RAG Triad and score the retrieval and generation legs separately, because a system can be faithful but irrelevant or relevant but ungrounded, and one overall number hides that. Context relevance asks whether retrieval fetched the right chunks; groundedness/faithfulness asks whether the answer is actually supported by those chunks with no hallucination; answer relevance asks whether it addresses the question. For retrieval I also track context precision and recall. RAGAS and LangSmith let me score these with an LLM instead of full human labels, which is what makes it cheap enough to run as a CI regression check.

---

**Q4. How do you set up evaluation in CI so quality doesn't regress?**

I maintain a versioned golden set — real queries plus known edge cases — and treat it like an answer key. On every prompt, model, or pipeline change, CI re-runs task metrics plus the LLM-judge on that set and gates the deploy on a quality threshold, exactly like unit tests block a bad merge. I track scores over time and alert on drops, so a prompt tweak that quietly pushes faithfulness from 0.9 to 0.7 gets caught before users do. To keep the set representative I sample live traffic back into it, which also feeds my online eval loop.

---

**Q5. What's the difference between offline and online eval, and why do you need both?**

Offline eval runs on my golden set before deploy — it's the rehearsal that gates the release. Online eval runs in production on live traffic: A/B tests, thumbs up/down, task-completion rate, and sampling real traces to score. Offline catches obvious regressions cheaply, but only live users reveal the failures I couldn't script, and those sampled traces become new golden-set cases. So offline gates the release and online tells me whether it actually works for users — the two feed each other.

---

## Your notes / STAR angle
- _TODO: an eval harness you built, what it caught, and the before → after numbers (e.g. faithfulness 0.7 → 0.9)._
