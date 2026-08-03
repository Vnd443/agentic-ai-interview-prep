# Fine-Tuning — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> Tests pragmatic trade-off judgment — the most valued applied-AI skill. The right answer is almost always "prompt first, fine-tune only when…". Contrasts with [[03-prompt-engineering]] and [[05-rag]].

---

## 1. Prompt-engineering-first (always start here) ⭐

**Definition:** Before fine-tuning, exhaust **prompt engineering**: few-shot examples, chain-of-thought, structured output, and system prompts. It's faster, cheaper, and reversible — enough for ~80% of tasks.

**Example:** **giving clear instructions to a smart new hire** before deciding to send them on a two-week training course. Most of the time, better instructions fix the problem.

**Why it matters:** *The* power move. *"I'd spend one day on prompt engineering before committing two weeks to a fine-tuning pipeline — iteration speed and cost usually win."* (→ [[03-prompt-engineering]])

---

## 2. What fine-tuning is (retraining on your examples)

**Definition:** **Fine-tuning** continues training a base model on your **input→output pairs** so the behaviour is baked into the weights — no longer dependent on a long prompt.

**Example:** the new hire goes on the **specialist course** and internalises the house style, so you no longer hand them a detailed brief every time — they just know it.

**Why it matters:** It changes *behaviour/style*, not *knowledge freshness*. *"Fine-tuning bakes behaviour into the weights; for fresh facts I still reach for RAG, not another training run."*

---

## 3. When to fine-tune (the checklist)

**Definition:** Fine-tune when: (1) you need a **consistent format/style** prompting can't reliably enforce; (2) it's a **narrow, well-defined task** with abundant labelled data; (3) you want to **cut latency/cost** by removing long system prompts; (4) you want to **distil** a big model's behaviour into a small cheap one.

**Example:** training a course only makes sense when the role is **stable, well-defined, and you'll hire many people for it** — not for a one-off task that changes weekly.

**Why it matters:** Shows judgment, not reflex. *"I fine-tune for consistent style, a narrow high-volume task, latency wins, or distillation — otherwise prompting wins on speed and cost."*

---

## 4. When NOT to fine-tune

**Definition:** Avoid it when: (1) you have **fewer than a few hundred** high-quality examples; (2) the task **changes often** (retraining is expensive); (3) you need **broad general knowledge** (risk of catastrophic forgetting); (4) the base model already does well **with the right prompt**.

**Example:** you don't build a bespoke training course for a role that's redefined every month — the course is obsolete before it's done.

**Why it matters:** Knowing when *not* to is the senior signal. *"If the task shifts often or I don't have clean data, fine-tuning is a liability — I stay with prompting or RAG."*

---

## 5. LoRA / PEFT / QLoRA (train a small adapter, not the whole model)

**Definition:** **LoRA** (Low-Rank Adaptation) freezes the base weights and trains small **low-rank adapter matrices** injected into attention layers — updating <1% of parameters. **PEFT** is the umbrella (parameter-efficient fine-tuning). **QLoRA** adds quantization so training fits on a single GPU. One base model can host **many swappable adapters**.

**Example:** instead of **rebuilding the whole car** to change its handling, you **swap in a small tuning chip** — cheap, fast, and you can pop in a different chip for a different need.

**Why it matters:** Makes fine-tuning practical. *"LoRA trains tiny adapters instead of full weights — far less compute, and I can serve one base model with many swappable adapters."*

---

## 6. Catastrophic forgetting (and how to mitigate it)

**Definition:** **Catastrophic forgetting** = fine-tuning on a narrow task makes the model **lose general capability** it had before. Mitigate by **mixing in general data**, keeping the **learning rate low**, and using **adapters** (LoRA) instead of full fine-tuning.

**Example:** a doctor who over-specialises in one procedure and gets rusty at general medicine — you keep them doing broad rounds to stay sharp.

**Why it matters:** A guaranteed follow-up. *"Narrow fine-tuning risks catastrophic forgetting — I mix in general data, keep the LR low, and prefer adapters so the base capability survives."*

---

## 7. Distillation (big model teaches a small one)

**Definition:** **Distillation** trains a **small, cheap model** to imitate a large model's outputs, capturing much of its behaviour at a fraction of the serving cost/latency.

**Example:** an **expert writing a concise playbook** a junior can follow — most of the expertise, a fraction of the salary.

**Why it matters:** A concrete "fine-tune to save money" story. *"I distil a frontier model's behaviour into a small one when I need its quality at production cost and latency."* (→ [[15-cost-optimization]])

---

## 8. Building a clean dataset

**Definition:** Curate high-quality input→output pairs that **match the production distribution**; **dedupe**; hold out a **clean eval set** (watch for leakage); balance classes/formats; iterate from small. **Hundreds–thousands of clean examples beat millions of noisy ones.**

**Example:** a **great textbook beats a pile of random handouts** — curation matters more than raw volume.

**Why it matters:** Data quality is the whole game. *"I start small, measure against the prompt-engineering baseline, and only scale data if it actually beats it — clean beats big."*

---

## 9. Hybrid — fine-tune + RAG

**Definition:** Combine them: **fine-tune** for domain **language/style/format**, then layer **RAG** on top for **factual grounding**. Domain-adapted *and* current.

**Example:** the specialist who both **speaks the house style** (fine-tune) and **checks the latest manual before answering** (RAG).

**Why it matters:** Shows you see them as complementary. *"Fine-tune for how it talks, RAG for what it knows — the hybrid is domain-adapted and still current."* (→ [[05-rag]])

---

## Quick misconceptions to avoid
- ❌ "Fine-tune to add new facts." → No — that's **RAG's** job; fine-tuning changes **behaviour/style**.
- ❌ "Fine-tune first for better quality." → **Prompt-engineer first** — cheaper, faster, reversible; enough ~80% of the time.
- ❌ "More data is always better." → **Clean beats big** — hundreds of curated examples beat millions of noisy ones.
- ❌ "Fine-tuning means retraining all the weights." → Use **LoRA/PEFT** — tiny adapters, <1% of params, swappable.
- ❌ "Fine-tune once and you're done." → Narrow tuning risks **catastrophic forgetting**; mix general data, low LR, adapters.
- ❌ "It's fine-tuning *or* RAG." → **Hybrid** — fine-tune for style, RAG for fresh facts.

---
_Related: [[03-prompt-engineering]] · [[05-rag]] · [[15-cost-optimization]] · [[18-aws-bedrock-sagemaker]] · [[02-llm-foundations]] · [[06-evaluation]]_
