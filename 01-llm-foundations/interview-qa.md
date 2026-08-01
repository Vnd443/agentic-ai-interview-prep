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

## Q4. Explain the transformer / self-attention like I'm not an ML expert.

**Ideal answer:** A transformer reads all the tokens at once and uses **self-attention** to let each token look at every other token and decide which ones matter for what comes next. Mechanically, each token produces three vectors — a **Query** (what it's looking for), a **Key** (what it offers), and a **Value** (what it contributes). You match each Query against all Keys to get relevance weights, then blend the Values by those weights — a soft, learned lookup over the sequence. **Multi-head** attention runs several of these in parallel so different heads capture different relationships, and **positional encoding** adds word order since there's no left-to-right recurrence. The big win over older RNNs is that it's **parallel** and captures **long-range dependencies**, which is what let it scale into today's LLMs.

**🔑 Power move:** "The key insight is parallelism plus attention — RNNs processed one token at a time and lost long-range context; transformers look at everything at once."

**Follow-ups:** Decoder-only vs encoder-only vs encoder-decoder (and which for chat/embeddings)? · What does 'masked' attention mean in a decoder? · Why positional encoding?

---

## Q5. Walk me through how an LLM is trained (pretraining → SFT → RLHF).

**Ideal answer:** Three stages. **Pretraining** — next-token prediction over massive unlabeled text; this is where the model gets its knowledge and raw capability, and it's the expensive part. **SFT (supervised fine-tuning)** — train on curated instruction→response pairs so it actually follows instructions instead of just continuing text. **RLHF or DPO** — align it to human preferences so it's helpful, harmless, and honest, using a reward model + RL or Direct Preference Optimization. Short version: pretraining gives it knowledge, SFT gives it obedience, RLHF gives it manners.

**🔑 Power move:** "Language and knowledge come from pretraining — fine-tuning only shapes behavior. That's why RAG, not fine-tuning, is usually the right fix for *knowledge* gaps." (bridges to [[07-fine-tuning]] and [[04-rag]])

**Follow-ups:** Where does fine-tuning fit vs RAG? · What's DPO vs RLHF? · Why can't you just pretrain and ship?

---

## Your notes / STAR angle
- Where in your IBM work did you reduce hallucinations? What did the rate drop from → to?
- _TODO: capture one concrete story._
