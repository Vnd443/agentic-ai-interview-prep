# Cost Optimization — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q02 (Cost Optimisation)**.

---

**Q1. How can you reduce the cost of running LLMs? ⭐ (guide Q02)**

I go after the levers roughly in impact order. The biggest is model selection and routing: use the smallest model that clears the quality bar, and route by difficulty — simple queries to a cheap fast model like Haiku or a fine-tuned small one, escalating only hard queries to a frontier model. That alone can cut 60 to 80 percent. Next, prompt optimization — shorter prompts mean fewer input tokens on every call, so I compress the system prompt and drop redundant few-shot examples. Then caching: a semantic cache keyed by embedding similarity serves near-identical questions at almost zero cost, and provider prompt caching cuts about 90 percent off a large static prefix. Then batching — non-urgent jobs go through the batch API for around 50 percent off. Fine-tuning a small specialized model can beat a large general one at a fraction of inference cost for high-volume narrow tasks. And output length control — capping max_tokens and requesting structured output, since output tokens cost three to five times input. The power move is to quantify: "model routing cut our API cost 70 percent while holding 95 percent of quality."

---

**Q2. How do you estimate the monthly cost of an LLM feature before building it?**

Back-of-envelope, it's requests per day times average input plus output tokens times price per token — and I split input and output pricing because output usually runs three to five times input. I add a buffer for retries and system-prompt overhead, which people forget is paid on every single call. Then I find the dominant term, which is almost always output tokens or the model tier, and I design against that before writing code. Doing the math out loud is the point — it signals I think about production economics, not just whether the feature works.

---

**Q3. How do you decide the routing threshold between a cheap and an expensive model?**

I put a lightweight classifier or heuristic in front that scores query complexity, and route below the threshold to the cheap model and above it to the frontier one. The key is I don't guess the threshold — I measure. I track the escalation rate and, more importantly, the quality on each tier against an eval suite, then tune the threshold to the point where I'm capturing most of the savings without dropping below my quality floor. If the cheap model starts failing and triggering escalations, that's a signal the threshold is too aggressive.

---

**Q4. What are the risks of semantic caching?**

The two big ones are stale answers and false cache hits. A semantic cache serves a stored answer when a new question is similar enough, so if the threshold is too loose it'll return a confidently wrong answer to a subtly different question, and if the underlying data changed the cached answer is stale. I manage both by tuning the similarity threshold carefully and setting a TTL so entries expire, and I don't cache anything time-sensitive or user-specific. It's a great lever for repeat FAQ-style traffic, but I treat the threshold and TTL as things to measure, not set-and-forget.

---

**Q5. How do you avoid over-optimizing cost at the expense of quality?**

I set a quality floor first — an eval suite that defines "good enough" — and only cut cost while staying above it. The metric I actually optimize is cost per successful task, not cost per call, because a cheap model that fails, forces a retry, and then escalates to the expensive model is more expensive than just using the right model once. So the cheapest contractor who does the job twice isn't cheap. That framing keeps me from chasing a low per-call number that quietly raises total cost and tanks the user experience.

---

## Your notes / STAR angle
- _TODO: a cost-reduction story with before/after numbers (e.g. routing cut cost X% at Y% quality; monthly spend $A → $B)._
