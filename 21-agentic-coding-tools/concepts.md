# Agentic Coding Tools — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> Proof you use agents daily, not just talk about them. Frame every point as first-hand insight into agent design. Ties directly to [[07-agents-tool-use]] and [[08-mcp-and-custom-skills]].

---

## 1. What they are (an agent specialised for code)

**Definition:** An **agentic coding tool** (Claude Code, Cline, Roo Code, OpenCode, IBM BOB) is an **LLM in a loop** with tools for reading/searching/editing files, running the shell, and running tests — pointed at your repo.

**Example:** a **fast junior engineer at your desk** — you describe the task, they read the code, make changes, run the tests, and show you the diff. You steer and review; they do the typing.

**Why it matters:** Reframes the tool from "autocomplete" to "agent." *"These are agents specialised for software tasks — an LLM in a loop with file, shell, and test tools — which is exactly the agent pattern I'd build for any domain."* (→ [[07-agents-tool-use]])

---

## 2. The loop — observe → reason → act → verify ⭐

**Definition:** Each turn: **observe** (repo state, tool output) → **reason** (plan the next step) → **act** (edit a file, run a command) → **verify** (run tests / read output), repeating until the task is done or it's stuck.

**Example:** a **mechanic diagnosing a car** — look, hypothesise, adjust one thing, test-drive, repeat. They don't rebuild the engine blind; each change is checked.

**Why it matters:** The **verify** step is what separates a reliable tool from a plausible-sounding one. *"The verification loop — run the tests after each change — is the difference between an agent that fixes the bug and one that confidently breaks something else."*

---

## 3. Context management on a large repo

**Definition:** The repo is far bigger than the context window, so the tool **retrieves selectively** — searches, opens only relevant files, summarises, and uses memory/instruction files (e.g. a `CLAUDE.md`) for durable conventions.

**Example:** a **detective working a case** — you don't memorise the whole city, you pull the specific files relevant to the lead you're chasing.

**Why it matters:** Context is the scarce resource. *"On a big repo the tool can't hold everything, so it retrieves the relevant slice and leans on an instructions file for conventions — context engineering, applied."* (→ [[09-context-engineering]])

---

## 4. Permissions & guardrails on actions

**Definition:** Actions that touch the world (writing files, running shell, pushing) are **permission-gated** — the tool asks or runs under a chosen mode, and destructive actions need confirmation.

**Example:** a **new hire who needs sign-off** before deleting a production table — trusted for edits, gated on the irreversible stuff.

**Why it matters:** Autonomy without guardrails is dangerous. *"Permission gating on shell and writes is the safety layer — I let it edit freely but keep a human gate on destructive or outward-facing actions."* (→ [[14-safety-guardrails]])

---

## 5. Getting reliable results (spec → plan → verify → review)

**Definition:** The workflow that works: give a **tight spec**, let it **plan first**, keep changes **scoped**, make it **run tests** to verify, and **review the diff**. Encode repo conventions in skills/custom instructions.

**Example:** delegating to that junior engineer — a clear ticket, a plan you approve, small PRs, and you read the diff before merge. Vague ask in, vague result out.

**Why it matters:** Steering quality determines output quality. *"I treat it like a fast junior — tight spec, plan first, scoped changes, tests to verify, human review on risky diffs."*

---

## 6. Sub-agents & skills (structured workflows)

**Definition:** Beyond the single loop, these tools add **sub-agents** (spawn a focused agent for a subtask) and **skills** (packaged, reusable instruction sets for a recurring workflow).

**Example:** a **lead delegating to specialists** — one person owns the migration, another the review — instead of doing everything in one head.

**Why it matters:** Shows you understand orchestration, not just single-agent use. *"Sub-agents and skills let me decompose a big task and encode repeatable workflows — the same multi-agent patterns I'd design deliberately."* (→ [[08-mcp-and-custom-skills]], [[12-multi-agent-orchestration]])

---

## 7. Where they fail (and when not to trust them)

**Definition:** Failure modes: large ambiguous tasks with no tests to verify against, unfamiliar/underdocumented code, silent over-broad changes, and confident wrong reasoning. Don't trust them on **irreversible, security-sensitive, or unverifiable** changes without review.

**Example:** the junior who confidently ships a plausible fix that passes no test you actually asked for — impressive-looking, occasionally wrong.

**Why it matters:** Knowing the limits is the senior signal. *"I don't trust it on irreversible or unverifiable changes — no tests to check against means I review by hand and scope tightly."*

---

## 8. What using them taught me about building agents ⭐

**Definition:** Daily use teaches agent design from the inside: **tool schema design**, **permissioning**, **verification loops**, and **context management** — the exact levers you pull when *building* an agent.

**Example:** learning to cook by watching a great chef every night — you absorb the technique, not just the meal.

**Why it matters:** *The* power move for this topic. *"Using these daily taught me agent design from the inside — tool schemas, permissioning, verification loops, context management — which is exactly what I apply when building agents."*

---

## Quick misconceptions to avoid
- ❌ "It's just fancy autocomplete." → It's an **agent** — a loop with tools, planning, and verification.
- ❌ "Give it the task and walk away." → **Spec, plan, verify, review** — steering quality drives output quality.
- ❌ "It reads the whole repo." → It **retrieves selectively**; context is the scarce resource.
- ❌ "Let it run anything." → **Permission-gate** shell and writes; confirm destructive actions.
- ❌ "If it compiles, it's correct." → No — **tests verify**; without them, review by hand.
- ❌ "Using a tool isn't real agent experience." → It's **first-hand insight** into tool design, permissioning, and verification loops.

---
_Related: [[07-agents-tool-use]] · [[08-mcp-and-custom-skills]] · [[12-multi-agent-orchestration]] · [[09-context-engineering]] · [[14-safety-guardrails]]_
