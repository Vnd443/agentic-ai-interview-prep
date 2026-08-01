# LLM Foundations — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

---

**Q1. How do you handle hallucinations in LLMs?**

Use layers, not one trick. (1) RAG — give the model real documents and tell it to answer only from them. (2) Chain-of-thought — make it reason step by step. (3) Output check — a second pass or validator flags unsupported claims. (4) Let it say "I don't know" instead of guessing. Key line: "No single fix — RAG plus output validation, defence in depth."

---

**Q2. How does an LLM generate text?**

It breaks text into tokens, then predicts the next token over and over (autoregressive), feeding each new token back in until it hits a stop point. The context window is the max tokens (prompt + answer) it can see at once. Temperature controls randomness — near 0 for factual tasks, higher for creative ones. Cost and speed depend on total tokens, so shorter answers are cheaper and faster.

---

**Q3. What causes hallucinations in the first place?**

The model is trained to produce fluent, likely text — not true text. It has no built-in sense of truth or "I don't know." So gaps in training data, outdated knowledge, or vague prompts push it to fill in a confident-sounding but wrong answer. That's why grounding it with RAG and letting it abstain both help.

---

**Q4. Explain the transformer / self-attention simply.**

A transformer reads all the tokens at once and uses self-attention so each token can look at every other token and decide which ones matter. Each token makes a Query (what it wants), a Key (what it is), and a Value (what it offers); match Queries to Keys, then blend the Values — a smart lookup. The big win over older RNNs is that it works in parallel and captures long-range links, which is what let it scale.

---

**Q5. Walk me through how an LLM is trained.**

Three stages. Pretraining — predict the next word over huge text; this gives it knowledge (the expensive part). SFT — train on good question→answer pairs so it follows instructions. RLHF/DPO — align it to human preferences so it's helpful and safe. Short version: pretraining = knowledge, SFT = obedience, RLHF = manners. Knowledge comes from pretraining, so use RAG (not fine-tuning) to fix knowledge gaps.

---

**Q6. What is the difference between Traditional ML and Generative AI?**

Traditional ML models classify or predict based on existing data (e.g. spam / not spam). Generative AI goes further and creates entirely new content — text, images, or audio — that resembles the data it was trained on. Also, traditional ML usually needs one model per task, while one GenAI foundation model handles many tasks.

---

## Your notes / STAR angle
- Where in your IBM work did you reduce hallucinations? What did the rate drop from → to?
- _TODO: capture one concrete story._
