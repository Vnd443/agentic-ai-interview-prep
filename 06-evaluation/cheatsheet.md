# Evaluation — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Mindset
- Can't measure it → can't improve/ship it —
- Demo vs product —

## The five layers
- Task metrics —
- LLM-as-judge —
- Human eval (gold standard) —
- Adversarial / red-team —
- Regression testing —

## Task metrics
- Classification: Accuracy / Precision / Recall / F1 —
- Summarization: ROUGE / BERTScore —
- Translation: BLEU / COMET —
- Retrieval: Precision@k / Recall@k / MRR / nDCG —
- Match metric to business cost —

## LLM-as-judge
- Structured rubric + few-shot anchors —
- Biases: position / verbosity / self-preference —
- Fixes: pairwise, position-swap, ensemble —
- Judge for iteration, human for sign-off —

## RAG eval — the RAG Triad
- Context relevance —
- Groundedness / faithfulness —
- Answer relevance —
- Tools: RAGAS / LangSmith —

## Golden dataset
- Versioned answer key —
- Real queries + edge cases —
- Grow by sampling production —

## Adversarial
- Jailbreaks / injection / OOD —
- Grade graceful failure, not just accuracy —

## Regression & CI
- Re-run golden set on every change —
- Quality gate blocks the deploy —
- Track scores over time —

## Offline vs online
- Offline: golden set, pre-deploy —
- Online: A/B, 👍/👎, task completion, sampled traces —

## Numbers / facts worth quoting
- 
