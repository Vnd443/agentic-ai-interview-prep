# 03 — Prompt Engineering & API Access

> Few-shot, chain-of-thought, structured output, system prompts

**Tier:** 1 — GenAI Core  |  **Interview frequency:** High

## Why this matters for your interviews
The cheapest, fastest lever for quality. Interviewers want to see you exhaust (and measure) prompting before reaching for fine-tuning.

## Subtopics checklist
- [ ] System prompts & role framing
- [ ] Zero-shot vs. few-shot; picking good examples
- [ ] Chain-of-thought (and when to skip it)
- [ ] Structured output / JSON mode / function calling
- [ ] Delimiters & separating instructions from untrusted data
- [ ] Prompt versioning + evaluating prompt changes
- [ ] API access: SDK / Messages API, auth, params (temperature, max_tokens, stop)
- [ ] Prompt caching — keep the cacheable prefix stable to cut cost/latency
- [ ] Token & cost basics per call (see [[15-cost-optimization]])
- [ ] Advanced reasoning techniques: self-consistency, decomposition, tree-of-thought

## Suggested learning flow
1. **Read `concepts.md` top-to-bottom** — learn-first: each idea is definition → real-world example → why it matters.
2. **Fill `cheatsheet.md` in your own words** — it's a topic skeleton; writing the one-liners yourself tests recall.
3. **Practice out loud from `interview-qa.md`** — answer first, then check.
4. Build (or map an existing) project in `projects/` and attach a STAR story.
5. Update **Status** below and log a mock answer in `../00-START-HERE/MOCK-INTERVIEW-LOG.md`.

## Interview angles (how they'll probe you)
- Techniques to make a prompt reliable
- How to force reliable JSON output
- When chain-of-thought hurts

## Files in this folder
- **README.md** — this file: what/why, subtopics, flow, interview angles, status.
- **concepts.md** — learn-first notes: each concept as definition → real-world example → why it matters.
- **cheatsheet.md** — topic skeleton you fill in your own words (recall practice).
- **interview-qa.md** — simple Q on one line, answer on the next; copy-paste template to add your own.
- **resources.md** — docs, papers, videos, blogs.
- **projects/** — project ideas ranked by resume impact (build 1-2 you can demo).

## Status
- [ ] Concepts drafted (`concepts.md`)
- [ ] Cheatsheet filled (`cheatsheet.md`)
- [ ] Q&A rehearsed out loud (`interview-qa.md`)
- [ ] Project built / mapped (`projects/`)
- [ ] Can teach this topic from memory

_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
