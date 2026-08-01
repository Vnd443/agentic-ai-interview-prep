# 01 — LLM Foundations

> How LLMs actually work: tokens, context window, decoding, hallucinations

> 📌 **Seeded from AI_Engineer_Interview_Guide.pdf — Q01 (Hallucinations).** See `interview-qa.md`.

**Tier:** 1 — GenAI Core  |  **Interview frequency:** Very High

## Why this matters for your interviews
Every GenAI interview starts here. If you can't explain tokens, context windows, and why models hallucinate, nothing else lands. It's also the base you build RAG, agents, and cost answers on.

## Subtopics checklist
- [ ] Tokenization & how autoregressive generation works
- [ ] **Transformer architecture & self-attention (Q/K/V), multi-head, positional encoding**
- [ ] **Architecture flavors: decoder-only vs encoder-only vs encoder-decoder (and why)**
- [ ] **Training lifecycle: pretraining → SFT → RLHF/DPO**
- [ ] **How this differs from traditional NLP (one model, zero-shot vs one model per task)**
- [ ] Context window, what overflow does, 'lost in the middle'
- [ ] Decoding params: temperature, top_p, top_k, greedy vs. sampling
- [ ] Why hallucinations happen + how to reduce them (layered defence)
- [ ] Perplexity, log-probs, self-consistency
- [ ] Prompt vs. completion tokens → cost/latency

## Suggested learning flow
1. Read/skim a primary resource in `resources.md`.
2. Write the core ideas in your own words in `concepts.md`.
3. Distil the must-knows + one-liners into `cheatsheet.md`.
4. Practice out loud from `interview-qa.md` (answer, then check).
5. Build (or map an existing) project in `projects/` and attach a STAR story.
6. Update **Status** below and log a mock answer in `../00-START-HERE/MOCK-INTERVIEW-LOG.md`.

## Interview angles (how they'll probe you)
- Explain hallucinations and your layered defence (guide Q01)
- Walk through what happens when you send a prompt
- Which decoding settings for factual vs. creative tasks and why

## Files in this folder
- **README.md** — this file: what/why, subtopics, flow, interview angles, status.
- **concepts.md** — your study notes; explain each idea like you would to an interviewer.
- **cheatsheet.md** — must-know terms, power-move one-liners, numbers to quote.
- **interview-qa.md** — Q -> ideal answer -> power move -> follow-ups.
- **resources.md** — docs, papers, videos, blogs.
- **projects/** — project ideas ranked by resume impact (build 1-2 you can demo).

## Status
- [x] Concepts drafted (`concepts.md`)
- [x] Cheatsheet filled (`cheatsheet.md`)
- [ ] Q&A rehearsed out loud (`interview-qa.md`)
- [ ] Project built / mapped (`projects/`)
- [ ] Can teach this topic from memory

_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
