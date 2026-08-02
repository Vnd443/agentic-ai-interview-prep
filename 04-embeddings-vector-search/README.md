# 04 — Embeddings & Vector Search

> Dense vectors, similarity, ANN/HNSW, vector databases

> 📌 **Seeded from AI_Engineer_Interview_Guide.pdf — Q07 (Embeddings).** See `interview-qa.md`.

**Tier:** 1 — GenAI Core  |  **Interview frequency:** High

## Why this matters for your interviews
The foundation under search, recommendations, and RAG. Deep understanding here signals strong fundamentals.

## Subtopics checklist
- [ ] What embeddings are & what space they live in
- [ ] Cosine vs. dot product vs. Euclidean
- [ ] Choosing an embedding model (benchmark on YOUR data)
- [ ] ANN indexes: HNSW vs. IVF, recall/latency trade-offs
- [ ] Vector DBs: Pinecone, Weaviate, pgvector, Qdrant
- [ ] Re-embedding on model version change; index freshness

## Suggested learning flow
1. Read/skim a primary resource in `resources.md`.
2. Write the core ideas in your own words in `concepts.md`.
3. Distil the must-knows + one-liners into `cheatsheet.md`.
4. Practice out loud from `interview-qa.md` (answer, then check).
5. Build (or map an existing) project in `projects/` and attach a STAR story.
6. Update **Status** below and log a mock answer in `../00-START-HERE/MOCK-INTERVIEW-LOG.md`.

## Interview angles (how they'll probe you)
- Explain embeddings + production use (guide Q07)
- How ANN search works and what you trade off
- Keeping an index fresh & consistent

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
