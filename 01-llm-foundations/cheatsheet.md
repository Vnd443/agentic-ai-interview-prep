# LLM Foundations — Cheatsheet

> Night-before rapid revision. Keep it to one page.

## Must-know terms
- **Token** — sub-word unit the model actually processes. ~4 chars ≈ 0.75 words.
- **Autoregressive generation** — predict next-token distribution → sample → append → repeat.
- **Context window** — max tokens attended to at once = **prompt + output**. Overflow silently drops info.
- **Lost in the middle** — mid-context facts recalled worst; put critical info at top & bottom.
- **Temperature** — randomness knob. ~0 = factual/deterministic; high = creative.
- **top_p (nucleus)** — sample from smallest set with cumulative prob ≥ p. **top_k** — top k tokens only.
- **Greedy decoding** — always take the top token; deterministic, can repeat.
- **Perplexity** — model "surprise" at text; lower = more predictable (fluency/confidence proxy).
- **Log-probs** — per-token confidence; threshold to abstain.
- **Self-consistency** — sample N, take majority vote; N× cost, better on reasoning.
- **Constrained decoding** — enforce structure (function calling / JSON schema) at sampling layer.

## Transformer & training (must-know)
- **Self-attention** — each token → **Q/K/V**; match Query·Key → softmax weights → blend of Values. A soft learned lookup.
- **Multi-head** — several attention patterns in parallel, then combined.
- **Positional encoding** — injects order (no recurrence); modern = RoPE.
- **Parallel, not sequential** — transformers process all tokens at once; decoders just **mask** the future.
- **Decoder-only** (GPT/Claude/Llama) = causal, generation. **Encoder-only** (BERT) = bidirectional, embeddings/classification. **Encoder-decoder** (T5) = translation/summarization.
- **Lifecycle:** Pretraining (next-token) → SFT (instruction pairs) → RLHF/DPO (alignment).
  - *"Pretraining = knowledge, SFT = obedience, RLHF = manners."*
- **vs traditional NLP:** one model, zero-shot — not one trained model per task.

## Power-move one-liners
- "It predicts the most *likely* next token, not the *true* one — that's why it's fluent AND why it hallucinates."
- Attention → "It's a soft, learned lookup — every token pulls in a weighted blend of the tokens most relevant to it."
- Transformers → "The win over RNNs is parallelism plus long-range attention — that's what let them scale."
- Architecture → "Chat models are decoder-only; embedding models are encoder-based — same building block, different attention."
- Hallucinations → **"RAG + output validation, defence in depth. No single trick."**
- Decoding → "Temperature ~0 for extraction/classification, raised only for ideation — cheapest reliability lever."
- Context → "Bigger window isn't free quality; attention degrades, so I keep context lean and put key facts at the edges."
- Latency → "Output tokens dominate latency — they're generated one at a time — so trim what the model *writes*."
- Abstention → "A calibrated 'I don't know' beats a confident wrong answer in production."

## Numbers / facts worth quoting
- **1 token ≈ 4 characters ≈ 0.75 words** (~750 words ≈ 1,000 tokens).
- **Output tokens ≈ 4–5× the price of input tokens** (and the latency bottleneck).
- Context windows today: commonly **~128K–200K**, some **1M+** — but usable ≠ reliable across the whole span.
- Self-consistency = **N× cost** for a majority vote.
- Temperature range: **0 → deterministic**, **~0.7–1.0 → balanced/creative**, **>1.2 → often incoherent**.

## The 5-layer hallucination defence (recite in order)
1. RAG / grounding → 2. Chain-of-thought → 3. Output validation (verifier / NLI) → 4. Constrained decoding → 5. Confidence + abstention.
Measure it: **RAGAS faithfulness / NLI entailment / human spot-check.**
