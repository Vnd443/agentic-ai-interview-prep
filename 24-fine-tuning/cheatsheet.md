# Fine-Tuning — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## The mindset ⭐
- Prompt-engineer first — cheaper, faster, reversible, enough ~80% of tasks —
- "1 day on prompts before 2 weeks on a fine-tune pipeline" —

## What it is
- Continue training on your input→output pairs; behaviour baked into weights —
- Changes style/behaviour, NOT knowledge freshness (that's RAG) —

## Fine-tune WHEN
- Consistent format/style prompting can't enforce —
- Narrow, well-defined task + abundant labelled data —
- Cut latency/cost by dropping long system prompts —
- Distil a big model into a small cheap one —

## Do NOT fine-tune WHEN
- < a few hundred clean examples —
- Task changes often (retraining expensive) —
- Need broad general knowledge (forgetting risk) —
- Base model already does well with the right prompt —

## LoRA / PEFT / QLoRA
- LoRA = train small low-rank adapters, freeze base, <1% of params —
- PEFT = umbrella term; QLoRA = + quantization, fits 1 GPU —
- One base model, many swappable adapters —

## Catastrophic forgetting
- Narrow tuning loses general capability —
- Mitigate: mix general data, low LR, use adapters —

## Distillation
- Small model imitates big model — quality at low cost/latency —

## Dataset
- Clean pairs matching prod distribution; dedupe; held-out eval (no leakage) —
- Clean beats big; measure vs. prompt baseline before scaling —

## Hybrid
- Fine-tune for style + RAG for fresh facts —

## Numbers / facts worth quoting
- Prompt engineering handles ~80% of tasks —
- LoRA updates <1% of parameters —
- Hundreds–thousands of clean examples > millions of noisy ones —
