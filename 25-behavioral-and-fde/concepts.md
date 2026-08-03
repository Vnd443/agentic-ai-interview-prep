# Behavioral & FDE — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> Half of every loop. The `interview-qa.md` here is a STAR workbook — draft your stories there; this file is the *method*.

---

## 1. STAR structure (a story with a spine) ⭐

**Definition:** Answer behavioral questions in **STAR**: **S**ituation (context), **T**ask (your responsibility), **A**ction (what *you* did), **R**esult (the outcome, **quantified**). Spend most of the time on Action and Result.

**Example:** a **news report** — headline the situation in a sentence, then focus on what happened and the outcome. A rambling answer with no result is a story with no ending.

**Why it matters:** Structure is what makes an answer land. *"I answer in STAR and always close on a number — a story without a measured result doesn't stick."*

---

## 2. Quantify the result (end on a number)

**Definition:** Every story ends with a **metric**: percentage, time saved, cost cut, users served, latency reduced, error rate dropped. Even a rough, honest number beats "it went well."

**Example:** "we made it faster" vs. **"we cut p95 latency from 4s to 900ms and support tickets dropped 30%"** — one is a claim, the other is evidence.

**Why it matters:** Numbers separate an engineer who *owned* impact from one who was *nearby* it. *"I close every story with a number — it's the difference between 'I helped' and 'I moved this metric.'"*

---

## 3. Pick 3–5 flagship stories that flex

**Definition:** Prepare **3–5 strong, real stories** (from IBM / GenAI work) and map each to **several** prompts — ambiguity, ownership, conflict, failure, impact. One story often answers many questions from a different angle.

**Example:** a **capsule wardrobe** — a few versatile pieces you re-combine for any occasion, instead of a new outfit for every event.

**Why it matters:** You can't prep 30 stories; you can master 5. *"I keep about five flagship stories and flex the angle — the same project can show ownership, learning fast, or influence depending on the question."*

---

## 4. The FDE mindset (ambiguity → ship → measure) ⭐

**Definition:** A **Forward Deployed Engineer** is customer-facing, ships fast, and **owns ambiguity end-to-end** — discovery → build → deploy → support. The signature move: dropped into a vague customer problem, scope it, ship something working fast, quantify the impact.

**Example:** a **paramedic, not a hospital specialist** — you arrive on-site with incomplete information, stabilise the situation fast, and adapt as facts change.

**Why it matters:** *The* power move for an FDE loop. *"For an FDE role I lead with a story where I was dropped into ambiguity at a customer, scoped it, shipped fast, and quantified the impact — that's the whole job in one story."*

---

## 5. Customer & stakeholder translation

**Definition:** Turn a **vague business ask into a working AI solution** — and explain technical trade-offs (cost, latency, accuracy) to a **non-technical** stakeholder in their terms.

**Example:** a **translator in a negotiation** — both sides think they're talking past each other until someone renders each side's need in the other's language.

**Why it matters:** The core FDE signal. *"I translate a fuzzy business ask into a scoped solution and explain the cost/latency/accuracy trade-off in the stakeholder's language, not mine."*

---

## 6. Speed vs. correctness judgment

**Definition:** Know when to ship a **"good enough" solution fast and iterate**, and when **not** to cut the corner (security, data integrity, irreversible actions). Being able to name *both* sides shows maturity.

**Example:** a **pit stop** — fast is the whole point, but you never skip torquing the wheel. Speed everywhere *except* the thing that must not fail.

**Why it matters:** *"I ship fast and iterate by default, but I name the corners I won't cut — anything irreversible or safety-critical gets the extra care."*

---

## 7. End-to-end ownership

**Definition:** A narrative where you owned **discovery → build → deploy → support** — not just your slice. Shows you carry a problem to a measured outcome, not to a hand-off.

**Example:** a **general contractor** who's accountable from the first sketch to the keys in hand — not a subcontractor who does one wall and leaves.

**Why it matters:** *"I like owning the whole arc — discovery to support — because that's where I can actually guarantee the outcome instead of hoping the hand-off holds."*

---

## 8. Questions to ask them (prepare 3–5)

**Definition:** Interviews are two-way. Prepare **3–5 sharp questions** — e.g. "What does success look like in the first 90 days?", "How do you balance customer velocity vs. platform quality?" — plus role/company-specific ones.

**Example:** **test-driving the car you might buy** — you're evaluating them as much as they're evaluating you, and it shows engagement.

**Why it matters:** Good questions signal seniority and genuine interest. *"I always come with a few pointed questions — the first-90-days one and the velocity-vs-quality one tell me a lot about how the team actually operates."*

---

## Quick misconceptions to avoid
- ❌ "Behavioral rounds are the easy filler." → They're **half the loop** and often the decider.
- ❌ "Tell the story chronologically with lots of context." → Use **STAR**; keep Situation short, spend time on Action + Result.
- ❌ "Saying it went well is enough." → **Quantify** — end on a number every time.
- ❌ "Prep a unique story per question." → Master **3–5 flexible** stories mapped to many prompts.
- ❌ "FDE = just coding at a customer site." → It's **owning ambiguity end-to-end** and translating business ↔ technical.
- ❌ "Faster is always better." → Name the **corners you won't cut** — that's the mature answer.

---
_Related: [[22-llm-system-design]] · [[15-cost-optimization]] · [[07-agents-tool-use]] · [[21-agentic-coding-tools]]_
