# 01 — Python (Core + Advanced)

> FastAPI, Boto3, Pandas, SQLAlchemy, async, testing

**Tier:** 3 — Backbone  |  **Interview frequency:** Medium-High

## Why this matters for your interviews
The language everything is built in. Async + FastAPI + testing LLM apps are the parts that come up for AI roles.

## Subtopics checklist
- [ ] async/await, event loop, asyncio.gather
- [ ] GIL: why threads don't help CPU work
- [ ] FastAPI: Pydantic, DI, streaming, background tasks
- [ ] Boto3 with IAM roles, retries, pagination
- [ ] Pandas / SQLAlchemy essentials
- [ ] Testing: mock the LLM boundary; separate eval suite
- [ ] OOP & dataclasses; type hints (typing, Pydantic models)
- [ ] HTTP & REST: calling APIs with `requests` / `httpx` (auth, JSON, status codes)
- [ ] Environments & packaging: venv, `pip`/`uv`, `.env` / env vars for secrets
- [ ] Database connectivity: SQLAlchemy / async DB drivers, connection pooling

## Suggested learning flow
1. Read/skim a primary resource in `resources.md`.
2. Write the core ideas in your own words in `concepts.md`.
3. Distil the must-knows + one-liners into `cheatsheet.md`.
4. Practice out loud from `interview-qa.md` (answer, then check).
5. Build (or map an existing) project in `projects/` and attach a STAR story.
6. Update **Status** below and log a mock answer in `../00-START-HERE/MOCK-INTERVIEW-LOG.md`.

## Interview angles (how they'll probe you)
- How async works & when it helps an AI service
- Why FastAPI + how you structure a backend
- How you test an LLM-dependent app

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
