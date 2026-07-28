# 18 — LLM System Design

> Scaling LLM systems, back-of-envelope cost math

> 📌 **Seeded from AI_Engineer_Interview_Guide.pdf — Q09 (System Design).** See `interview-qa.md`.

**Tier:** 4 — Interview Craft  |  **Interview frequency:** High (senior/FDE)

## Why this matters for your interviews
Where all topics combine. Always open with back-of-envelope token math, then architecture, cost, reliability, eval, safety.

## Subtopics checklist
- [ ] Back-of-envelope token & cost math (do it out loud)
- [ ] Queue -> workers -> LLM -> store architecture
- [ ] Chunking / map-reduce / hierarchical summarisation
- [ ] Cost engineering: batch, routing, caching
- [ ] Reliability: DLQ, retries, idempotency, autoscale
- [ ] QA sampling + drift alerts
- [ ] Latency design: real-time vs. batch

## Suggested learning flow
1. Read/skim a primary resource in `resources.md`.
2. Write the core ideas in your own words in `concepts.md`.
3. Distil the must-knows + one-liners into `cheatsheet.md`.
4. Practice out loud from `interview-qa.md` (answer, then check).
5. Build (or map an existing) project in `projects/` and attach a STAR story.
6. Update **Status** below and log a mock answer in `../00-START-HERE/MOCK-INTERVIEW-LOG.md`.

## Interview angles (how they'll probe you)
- Design a system to summarise 10K docs/day (guide Q09)
- General framework for any LLM system design Q
- Designing for latency (chatbot vs. batch)

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
