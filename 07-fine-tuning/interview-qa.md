# Fine-Tuning — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q05 (Fine-Tuning vs. Prompting)**.

---

## Q1. When would you fine-tune vs. use prompt engineering? ⭐ (guide Q05)

**Why they ask:** It tests pragmatic trade-off judgment — arguably the most important applied-AI skill.

**Ideal answer:**
- **Start with prompt engineering — always.** Faster, cheaper, reversible: few-shot, chain-of-thought, structured output, system prompts. For ~80% of tasks it's enough.
- **Fine-tune when:** (1) you need consistent format/style prompting can't reliably enforce; (2) narrow, well-defined task with abundant labelled data; (3) you want to cut latency by removing long system prompts; (4) you want to **distil** a large model's behaviour into a small cheap one.
- **Don't fine-tune when:** (1) fewer than a few hundred high-quality examples; (2) the task changes often (retraining is expensive); (3) you need broad general knowledge (risk of **catastrophic forgetting**); (4) the base model already does well with the right prompt.
- **Hybrid:** fine-tune for domain language/style, then layer **RAG** on top for factual grounding → domain-adapted *and* current.

**🔑 Power move:** Frame it as iteration speed & cost: *"I'd spend 1 day on prompt engineering before committing 2 weeks to a fine-tuning pipeline."*

**Follow-ups:**
- What is LoRA / PEFT and why use it? → train small adapter matrices, not full weights — cheap, fast, swappable.
- What is catastrophic forgetting and how do you mitigate it? → mix in general data, keep learning rate low, use adapters.
- How much data do you actually need? → task-dependent, but hundreds–thousands of *clean* examples beats millions of noisy ones.

---

## Q2. What is LoRA and why is it popular?

**Ideal answer:** **LoRA** (Low-Rank Adaptation) freezes the base weights and trains small low-rank adapter matrices injected into attention layers. You update <1% of parameters → far less memory/compute, fast training, and you can host one base model with many swappable adapters. QLoRA adds quantization to fit training on a single GPU.

**Follow-ups:** Full fine-tune vs. LoRA trade-offs? Serving multiple adapters?

---

## Q3. How do you build a fine-tuning dataset?

**Ideal answer:** Curate high-quality input→output pairs that match production distribution; dedupe; hold out a clean eval set (watch for leakage); balance classes/formats; and iterate — start small, measure against the prompt-engineering baseline, and only scale data if it beats it.

---

## Your notes / STAR angle
- _TODO: any fine-tuning / distillation you did, and why over prompting._
