# Agentic Coding Tools — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## What they are
- LLM in a loop + tools (read/search/edit files, shell, tests), pointed at a repo —
- Claude Code, Cline, Roo Code, OpenCode, IBM BOB —
- An agent specialised for software tasks —

## The loop ⭐
- Observe (repo/output) → reason → act (edit/run) → verify (tests) —
- Verify step = reliable vs. plausible —

## Context on large repos
- Retrieve selectively, open only relevant files —
- Instructions/memory file for durable conventions —

## Permissions
- Gate shell + writes; confirm destructive actions —
- Autonomy without guardrails = dangerous —

## Reliable results
- Tight spec → plan first → scoped changes → tests → review diff —
- Treat like a fast junior engineer —

## Structured workflows
- Sub-agents (focused subtask) —
- Skills (packaged reusable workflows) —

## Failure modes
- Ambiguous tasks with no tests to verify —
- Unfamiliar code, silent over-broad changes, confident-wrong —
- Don't trust on irreversible / unverifiable changes —

## The power move ⭐
- Using them daily = agent design from the inside (schemas, permissioning, verify loops, context) —

## Numbers / facts worth quoting
- (mostly qualitative — lead with a concrete task one of these accelerated for you) —
