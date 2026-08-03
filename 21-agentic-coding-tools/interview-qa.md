# Agentic Coding Tools — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Starter set (not from the guide PDF). Tools on your resume: **Claude Code, OpenCode, IBM BOB, Cline, Roo Code.** This shows you *use* agents daily, not just talk about them.

---

**Q1. How do agentic coding tools work under the hood?**

They're agents specialised for software tasks: an LLM in a loop with tools for reading and searching files, editing them, running the shell, and running tests. The loop is observe the repo state and tool output, reason about the next step, act by editing or running something, then verify with the tests — iterating until the task is done. On top of that they add repo context (relevant files, git state), permission gating on actions that touch the world, and often sub-agents or skills for structured, repeatable workflows. The thing I'd emphasise is that using these daily taught me agent design from the inside — tool schemas, permissioning, verification loops, context management — which is exactly what I apply when I'm the one *building* an agent.

---

**Q2. How do you get reliable results from an agentic coding tool?**

I treat it like a fast junior engineer. I give it a tight spec and let it plan first, keep the changes scoped rather than sprawling, make it run the tests to verify its own work, and review the diff before anything merges. For repo conventions I lean on skills or a custom-instructions file so it doesn't relearn them every session. The verification loop is the key — running tests after each change is what separates a tool that fixes the bug from one that confidently breaks something else. And I don't trust it blindly on irreversible, security-sensitive, or unverifiable changes; those I scope tightly and review by hand, because a large ambiguous task with no tests to check against is exactly where these tools fail.

---

**Q3. Compare the tools you've used and why you'd pick one.**

I contrast them on a few axes: autonomy level, whether they're IDE-integrated or CLI, how flexible they are on model choice, how extensible they are through MCP and skills, and how their guardrails and permissioning work. Then I tie it to a concrete task where one fit better than another — for example reaching for a CLI agent when I want it driving the shell and tests end-to-end, versus an IDE-embedded one when I'm doing tight, interactive edits with the code in front of me. *(Fill from your hands-on experience across Claude Code, Cline, Roo Code, OpenCode, and IBM BOB — a specific task and why one won makes this answer land.)*

---

## Your notes / STAR angle
- _TODO: a task an agentic tool accelerated, and how you steered/verified it._
