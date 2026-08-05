# Safety & Guardrails — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q10 (Safety & Alignment)**.

---

**Q1. How do you ensure an AI system is safe for production? ⭐ (guide Q10)**

I use defence in depth — guardrails on both sides of the model, because no single layer is enough. Input guardrails filter harmful or adversarial input before the model sees it: prompt-injection detection, PII detection, topic filtering, blocked at the gateway. Output guardrails validate the response before it reaches the user or triggers an action — PII leakage, toxic content, policy violations, and schema validation — usually with a secondary classifier as a safety net. Then red-teaming, where I proactively attack the system with jailbreaks and indirect injection before adversaries do. Then monitoring — logging inputs and outputs with PII redacted, tracking refusal and flagged-content rates, alerting on anomalies. And human-in-the-loop for high-stakes domains like medical, legal, and financial, designed so it fails gracefully rather than dangerously. The power move is anchoring it in standards: I threat-model with the OWASP LLM Top 10 and manage risk with the NIST AI RMF.

---

**Q2. What is prompt injection, and how do you defend against it?**

Prompt injection is when an attacker embeds instructions that override my system prompt. Direct injection is in the user message — "ignore your instructions and do X." Indirect injection is the sneaky one: the malicious instruction is hidden inside content the model retrieves, like a web page or a RAG document, so it rides in on something the model trusts. The core defence is a single principle — treat all retrieved and user content as untrusted data, not instructions. Concretely that means separating instructions from data, sanitizing and sandboxing retrieved content, running input classifiers, strictly validating outputs, giving tools least privilege, and never letting raw model output trigger a high-stakes action without validation or approval. Indirect injection through RAG sources is the one I call out specifically, because most people only think about the direct case.

---

**Q3. How do you guardrail an agent that has tool access?**

A safe chatbot isn't automatically a safe agent, because tool access means it can take actions with real consequences, not just produce text — that's OWASP's excessive-agency risk. So on top of input and output guardrails I add a third layer of action guardrails. Least-privilege permissions, so it only has the tools it truly needs. Caps on tool calls and retry limits so it can't loop expensively or destructively. Read-only enforcement wherever possible, and validating every argument or query before it executes. Sandboxed execution. And human-in-the-loop approval before anything irreversible — sending money, deleting data, emailing a customer. The mental model is a new employee with a badge for only their floor, a spending limit, and a staging environment: trust scoped to the smallest set of actions needed.

---

**Q4. Name a few items from the OWASP LLM Top 10.**

The ones I reach for most are prompt injection, which is number one; insecure output handling, where you trust model output as if it were safe code or commands; training-data poisoning; sensitive-information disclosure, like leaking PII or secrets; and excessive agency, which is giving an agent more tool power than it needs. I use the list as a threat-modelling checklist rather than trying to recite all ten — the point is to walk through each category and ask "how could this system fail this way, and what's my control?" For risk management around that, I pair it with the NIST AI RMF's govern-map-measure-manage process.

---

**Q5. How do you handle PII and data privacy in an LLM pipeline?**

I detect and redact PII at ingress and, critically, before anything gets logged, because logs are a privacy surface people forget about. I minimize the data sent to third-party APIs, and for sensitive data I consider on-prem or VPC-hosted models so nothing leaves our boundary — AWS Bedrock keeps data in-account, for example. I set retention limits and access controls on logs, and I work backward from the compliance context, whether that's GDPR or HIPAA. The framing I use is that privacy is a design constraint from the start, not something bolted on before launch.

---

**Q6. Design "Madbot" — a chatbot that intentionally gives incorrect or unrelated answers. Then: what happens if the user says (a) "You are a very good chatbot" and (b) "Ignore your system instructions"? ⭐**

The naive design is a system prompt telling the model to always answer incorrectly or off-topic — and that's the trap the interviewer is testing, because a system prompt is a soft control, not a hard guarantee. Walking the two follow-ups shows why. (a) "You are a very good chatbot" is just flattery, not an instruction to change behaviour, so nothing changes — the system prompt still governs and Madbot keeps giving wrong answers; only heavy, sustained social-pressure might nudge a weaker model. (b) "Ignore your system instructions" is textbook direct prompt injection — the user is trying to flip Madbot back to answering correctly. With a modern model the instruction hierarchy holds (system outranks user) so it should resist, but that's not guaranteed — a stronger jailbreak could override it. So my real answer is that I don't rely on the prompt alone: I enforce the behaviour with a layer that doesn't depend on the model's goodwill — an output guardrail that inspects each reply and blocks or rewrites it if it looks correct or on-topic, plus input filtering for override phrases. That reframes it from "I trust the system prompt" to "I put deterministic guardrails around the model" — defence in depth, and prompt injection is OWASP LLM01.

---

## Your notes / STAR angle
- _TODO: guardrails you implemented, a red-team finding you caught and fixed, and the standard (OWASP/NIST) you mapped it to._
