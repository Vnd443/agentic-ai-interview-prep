# 02 — LLM Foundations (Mental Model of an LLM)

> How LLMs actually work: tokens, context window, decoding, hallucinations

> 📌 **Seeded from AI_Engineer_Interview_Guide.pdf — Q01 (Hallucinations).** See `interview-qa.md`.

**Tier:** 1 — GenAI Core  |  **Interview frequency:** Very High

## Why this matters for your interviews
Every GenAI interview starts here. If you can't explain tokens, context windows, and why models hallucinate, nothing else lands. It's also the base you build RAG, agents, and cost answers on.

## Subtopics checklist
_Big picture_
- [ ] AI → ML → GenAI → LLM (the family tree)
- [ ] What an LLM / foundation model is
- [ ] Where NLP fits
- [ ] Generative AI vs Traditional ML

_Basics_
- [ ] Tokens
- [ ] Embeddings (meaning → numbers)

_How it works inside_
- [ ] Autoregressive generation
- [ ] Parameters & scale (7B/70B)
- [ ] Transformer & self-attention (Q/K/V, multi-head, positional encoding)
- [ ] Architecture flavors: decoder-only vs encoder-only vs encoder-decoder (and why)
- [ ] Training lifecycle: pretraining → SFT → RLHF/DPO
- [ ] GPT = Generative Pre-trained Transformer + model families (GPT/Claude/Llama/Gemini, open vs closed)
- [ ] Types by size/specialty: LLM / SLM / MoE / reasoning / multimodal
- [ ] In-context learning (zero / one / few-shot)
- [ ] Base vs instruction-tuned vs reasoning models (when to reach for each)
- [ ] Benchmarks & leaderboards (MMLU, GPQA, etc.) — what they do/don't tell you

_Using it_
- [ ] Context window, overflow, 'lost in the middle'
- [ ] Decoding params: temperature, top_p, top_k, greedy vs. sampling
- [ ] Why hallucinations happen + how to reduce them (layered defence)
- [ ] Perplexity, log-probs, self-consistency

_Engineering disciplines_
- [ ] Prompt vs Context vs Loop vs Orchestration engineering
- [ ] The agent loop (looping)
- [ ] Memory — types (episodic/semantic/procedural), storage techniques, challenges
- [ ] Generative AI vs AI Agents vs Agentic AI (the three terms, clearly distinguished)

## Suggested learning flow
1. Read `concepts.md` top-to-bottom (definition → example → why it matters).
2. In `cheatsheet.md`, write your own one-line explanation next to each topic — that's your recall test.
3. Practice out loud from `interview-qa.md` (answer, then check); add any interesting question using the template.
4. Skim a primary resource in `resources.md` to go deeper where needed.
5. Build (or map an existing) project in `projects/` and attach a STAR story.
6. Update **Status** below and log a mock answer in `../00-START-HERE/MOCK-INTERVIEW-LOG.md`.

## Interview angles (how they'll probe you)
- Explain hallucinations and your layered defence (guide Q01)
- Walk through what happens when you send a prompt
- Which decoding settings for factual vs. creative tasks and why

## Files in this folder
- **README.md** — this file: what/why, subtopics, flow, interview angles, status.
- **concepts.md** — learn-first notes. Each concept = plain **definition → example (or diagram/code) → why it matters**. Read top-to-bottom the first time.
- **cheatsheet.md** — just the topic list; write your own one-line explanation next to each.
- **interview-qa.md** — simple **Q → answer** format; copy the template to add your own.
- **resources.md** — docs, papers, videos, blogs.
- **projects/** — project ideas ranked by resume impact (build 1-2 you can demo).

## Status
- [x] Concepts drafted (`concepts.md`)
- [x] Cheatsheet filled (`cheatsheet.md`)
- [ ] Q&A rehearsed out loud (`interview-qa.md`)
- [ ] Project built / mapped (`projects/`)
- [ ] Can teach this topic from memory

_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
