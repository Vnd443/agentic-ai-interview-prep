# Prompt Engineering — Interview Q&A

> Starter set (not from the guide PDF). Add answers in your own words + a story where you can.

---

## Q1. What techniques do you use to improve prompt reliability?

**Ideal answer:** Clear role/system prompt; **few-shot** examples that match the target distribution; **chain-of-thought** for reasoning tasks; **structured output** (JSON schema / function calling) to constrain format; explicit constraints and negative instructions; and delimiters to separate instructions from user data. Iterate against an eval set — don't eyeball it.

**🔑 Power move:** "Prompting is the cheapest, fastest lever — I exhaust it (and measure it) before reaching for fine-tuning."

**Follow-ups:** Zero- vs. few-shot? When does CoT hurt (simple tasks, added cost/latency)?

---

## Q2. How do you get reliable structured/JSON output?

**Ideal answer:** Prefer native **function calling / structured output / JSON mode** over free-text-then-parse. Provide the schema, give an example, set temperature low, and **validate** the output (retry on schema failure). Constrained decoding beats hoping the model formats correctly.

**Follow-ups:** What do you do on a parse failure? (retry with the error, or a repair pass)

---

## Q3. What is chain-of-thought and when should you NOT use it?

**Ideal answer:** CoT asks the model to reason step-by-step before answering, improving multi-step/logical tasks and reducing shortcut errors. **Skip it** for simple lookups/classification (wastes tokens + latency) and when you need terse output — there you can reason internally then emit only the answer, or use a smaller model.

---

## Q4. How do you manage prompts as they grow (versioning, testing)?

**Ideal answer:** Treat prompts as code: version them, keep a regression eval suite, A/B test changes, and log which prompt version produced which output (critical for debugging prod). Externalise prompts from code where possible.

---

## Your notes / STAR angle
- _TODO: a prompt you iterated that moved a metric._
