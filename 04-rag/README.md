# 04 — RAG (Retrieval-Augmented Generation)

> Ingest -> embed -> retrieve -> rerank -> generate -> eval

> 📌 **Seeded from AI_Engineer_Interview_Guide.pdf — Q04 (RAG).** See `interview-qa.md`.

**Tier:** 1 — GenAI Core  |  **Interview frequency:** Very High

## Why this matters for your interviews
The most common production LLM pattern. You must be able to design it end-to-end AND debug it. Re-ranking is the detail most candidates miss.

## Subtopics checklist
- [ ] Ingestion & chunking (200-500 tokens, overlap, structure-aware)
- [ ] Embedding + indexing with metadata
- [ ] Retrieval: top-k, hybrid (vector + BM25)
- [ ] Re-ranking with a cross-encoder (the differentiator)
- [ ] Generation: answer-only-from-context + citations
- [ ] Evaluation: recall, faithfulness, relevance (RAGAS)
- [ ] Debugging retrieval vs. generation separately

## Suggested learning flow
1. Read/skim a primary resource in `resources.md`.
2. Write the core ideas in your own words in `concepts.md`.
3. Distil the must-knows + one-liners into `cheatsheet.md`.
4. Practice out loud from `interview-qa.md` (answer, then check).
5. Build (or map an existing) project in `projects/` and attach a STAR story.
6. Update **Status** below and log a mock answer in `../00-START-HERE/MOCK-INTERVIEW-LOG.md`.

## Interview angles (how they'll probe you)
- Design a RAG pipeline end-to-end (guide Q04)
- Retrieval returns junk — how do you debug it
- When RAG is the wrong choice

## Files in this folder
- **README.md** — this file: what/why, subtopics, flow, interview angles, status.
- **concepts.md** — your study notes; explain each idea like you would to an interviewer.
- **cheatsheet.md** — must-know terms, power-move one-liners, numbers to quote.
- **interview-qa.md** — Q -> ideal answer -> power move -> follow-ups.
- **resources.md** — docs, papers, videos, blogs.
- **projects/** — project ideas ranked by resume impact (build 1-2 you can demo).

## Status
- [ ] Concepts drafted (`concepts.md`)
- [ ] Cheatsheet filled (`cheatsheet.md`)
- [ ] Q&A rehearsed out loud (`interview-qa.md`)
- [ ] Project built / mapped (`projects/`)
- [ ] Can teach this topic from memory

_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
