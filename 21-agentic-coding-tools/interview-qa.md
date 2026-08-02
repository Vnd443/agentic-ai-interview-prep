# Agentic Coding Tools — Interview Q&A

> Starter set (not from the guide PDF). Tools on your resume: **Claude Code, OpenCode, IBM BOB, Cline, Roo Code.** This shows you *use* agents daily, not just talk about them.

---

## Q1. How do agentic coding tools work under the hood?

**Ideal answer:** They're **agents specialised for software tasks**: an LLM in a loop with tools for reading/searching/editing files, running the shell, and running tests. The loop is observe (repo state, tool output) → reason → act (edit/run) → verify (tests), iterating until the task is done. They add repo context (files, git), permission gating on actions, and often sub-agents/skills for structured workflows.

**🔑 Power move:** "Using these daily taught me agent design from the inside — tool schemas, permissioning, verification loops, context management — which is exactly what I apply when *building* agents."

**Follow-ups:** How do they manage context on a large repo? How do permissions/guardrails work? Where do they fail?

---

## Q2. How do you get reliable results from an agentic coding tool?

**Ideal answer:** Give it a tight spec and let it plan first; keep changes scoped; make it run tests to verify; review diffs; and use skills/custom instructions to encode repo conventions. Treat it like a fast junior engineer — clear task, verification loop, human review on risky changes.

**Follow-ups:** When do you *not* trust it? How do you prevent runaway/destructive actions?

---

## Q3. Compare the tools you've used and why you'd pick one.

**Ideal answer (structure):** Contrast on autonomy level, IDE vs. CLI, model flexibility, extensibility (MCP/skills), and guardrails. Tie to a concrete task where one fit better. *(Fill from your hands-on experience with Claude Code / Cline / Roo Code / OpenCode / IBM BOB.)*

---

## Your notes / STAR angle
- _TODO: a task an agentic tool accelerated, and how you steered/verified it._
