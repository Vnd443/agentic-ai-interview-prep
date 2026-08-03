# Fine-Tuning — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q05 (Fine-Tuning vs. Prompting)**.

---

**Q1. When would you fine-tune vs. use prompt engineering? ⭐ (guide Q05)**

I start with prompt engineering, always — few-shot, chain-of-thought, structured output, system prompts. It's faster, cheaper, and reversible, and for around 80% of tasks it's enough. I move to fine-tuning only in specific cases: when I need a consistent format or style that prompting can't reliably enforce, when it's a narrow well-defined task with abundant labelled data, when I want to cut latency and cost by removing a long system prompt, or when I want to distil a large model's behaviour into a small cheap one. I avoid fine-tuning when I have fewer than a few hundred high-quality examples, when the task changes often so retraining is a treadmill, when I need broad general knowledge and risk catastrophic forgetting, or when the base model already does well with the right prompt. And these aren't mutually exclusive — a strong hybrid is to fine-tune for domain language and style, then layer RAG on top for factual grounding, so it's both domain-adapted and current. The way I frame the whole decision is iteration speed and cost: I'd spend one day on prompt engineering before committing two weeks to a fine-tuning pipeline.

---

**Q2. What is LoRA and why is it popular?**

LoRA — Low-Rank Adaptation — freezes the base model weights and trains small low-rank adapter matrices injected into the attention layers, so I'm updating less than 1% of the parameters. That means far less memory and compute, fast training, and I can host one base model with many swappable adapters for different tasks. QLoRA goes further by adding quantization so the training fits on a single GPU. Compared to a full fine-tune, it's dramatically cheaper and more flexible, and the small quality gap rarely matters for the narrow tasks fine-tuning is right for in the first place.

---

**Q3. How do you build a fine-tuning dataset?**

I curate high-quality input-to-output pairs that match the production distribution, dedupe them, and hold out a clean eval set while watching carefully for leakage between train and eval. I balance the classes and formats so the model doesn't skew, and I iterate — start small, measure against the prompt-engineering baseline, and only scale the data if it actually beats that baseline. The principle throughout is that clean beats big: a few hundred to a few thousand carefully curated examples will outperform millions of noisy ones, because the model learns exactly what the noise teaches it.

---

## Your notes / STAR angle
- _TODO: any fine-tuning / distillation you did, and why over prompting._
