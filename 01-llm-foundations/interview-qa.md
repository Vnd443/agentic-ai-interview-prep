# LLM Foundations — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q01 (Hallucinations & Trust)**.
> Format per question: **Q → Ideal answer → Power move → Follow-ups you should expect.**

---

## Q1. How do you handle hallucinations in LLMs? ⭐ (guide Q01)

**Why they ask:** Hallucinations are the #1 reason companies don't trust AI in production. Anything customer-facing needs an answer here.

**Ideal answer — layered defence, not one trick:**
1. **RAG / grounding** — stop relying on parametric memory. Retrieve real chunks from a vector store and force the model to answer *only* from that context.
2. **Chain-of-thought** — make the model show its working; step-by-step reasoning skips fewer facts than jumping to a plausible-sounding conclusion.
3. **Output validation & guardrails** — post-process: fact-check against sources, a second verifier model call, or an NLI classifier that flags unsupported claims.
4. **Constrained decoding** — if the answer must be one of N options, use function calling / structured JSON instead of free generation.
5. **Confidence calibration** — use log-probs or self-consistency (sample several outputs, take the majority); **abstain** when confidence is low rather than guessing.

**🔑 Power move:** "No single technique is enough — I combine **RAG + output validation as defence in depth**. Production needs layers."

**Follow-ups to expect:**
- How do you *measure* hallucination rate? → faithfulness eval (RAGAS), NLI entailment against sources, human spot-check.
- What's self-consistency and when is it worth the extra cost?
- How does constrained decoding differ from just prompting for JSON?

---

## Q2. Explain how an LLM generates text (tokens, context window, decoding).

**Ideal answer:**
- Text is **tokenized** into sub-word units; the model predicts a probability distribution over the next token, repeatedly, autoregressively.
- **Context window** = max tokens (prompt + output) the model can attend to at once. Overflow silently drops information.
- **Decoding controls the sampling:** greedy vs. `temperature` (randomness), `top_p` (nucleus), `top_k`. Low temperature → deterministic/factual; high → creative/varied.
- Cost and latency scale with **total tokens** (input + output), which is why prompt and `max_tokens` discipline matters.

**🔑 Power move:** Tie it to a real lever — "I keep temperature ~0 for extraction/classification and only raise it for ideation."

**Follow-ups:** What is perplexity? Why do long contexts degrade quality ("lost in the middle")? Greedy vs. sampling trade-offs?

---

## Q3. What causes hallucinations in the first place?

**Ideal answer:** LLMs are trained to produce *fluent, likely* text, not *true* text — they have no built-in notion of truth or "I don't know." Causes: gaps in training data, outdated parametric knowledge, ambiguous prompts, over-long context, and pressure to always answer. That's *why* grounding (RAG) and abstention work — they replace "most likely token" with "supported by evidence."

**Follow-ups:** Why can a bigger model still hallucinate confidently? How does temperature interact with hallucination?

---

## Your notes / STAR angle
- Where in your IBM work did you reduce hallucinations? What did the rate drop from → to?
- _TODO: capture one concrete story._
