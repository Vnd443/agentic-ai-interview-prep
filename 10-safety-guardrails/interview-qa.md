# Safety & Guardrails — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q10 (Safety & Alignment)**.

---

## Q1. How do you ensure an AI system is safe for production? ⭐ (guide Q10)

**Why they ask:** Safety isn't optional — regulators, legal, and users all care. Companies want engineers who build responsibly.

**Ideal answer — layers:**
1. **Input guardrails** — filter/flag harmful or adversarial inputs *before* the model: prompt-injection detection, PII detection, topic filtering; block/sanitise at the API gateway.
2. **Output guardrails** — validate outputs *before* they reach users: PII leakage, harmful content, off-topic, policy violations. A secondary classifier or rules engine as a safety net.
3. **Red teaming** — systematically try to break it: jailbreaks, **indirect prompt injection** (malicious content inside retrieved docs), data-extraction attacks, social engineering.
4. **Monitoring & observability** — log inputs/outputs (PII-redacted), track refusal rate, flagged-content rate, user reports; alert on anomalous patterns.
5. **Human-in-the-loop** — for high-stakes domains (medical, legal, financial), route uncertain/sensitive outputs to human review; design escalation so it fails *gracefully*, not dangerously.

**🔑 Power move:** Reference real standards: *"I follow **OWASP's LLM Top 10** for threat modelling and **NIST's AI RMF** for risk management."* Signals maturity.

**Follow-ups:**
- What is prompt injection, direct vs. indirect? → user-supplied vs. injected via retrieved/3rd-party content.
- Name a few OWASP LLM Top 10 items. → prompt injection, insecure output handling, training-data poisoning, sensitive-info disclosure, excessive agency…
- How do you guardrail an *agent* with tool access? → permissions, sandboxing, HITL approval, output validation before execution.

---

## Q2. What is prompt injection and how do you defend against it?

**Ideal answer:** An attacker embeds instructions that override your system prompt — **direct** (in the user message) or **indirect** (hidden in a web page / document the model retrieves). Defences: treat all retrieved/user content as **untrusted data, not instructions**; input classifiers; strict output validation; least-privilege tools; and never let model output directly trigger a high-stakes action without validation/approval.

**🔑 Power move:** "Indirect injection through RAG sources is the sneaky one — I sanitise and sandbox retrieved content."

**Follow-ups:** How does injection interact with agents/tools (excessive agency)?

---

## Q3. How do you handle PII and data privacy in an LLM pipeline?

**Ideal answer:** Detect + redact PII at ingress and before logging; minimise data sent to third-party APIs; consider on-prem/VPC model hosting for sensitive data; retention limits; and access controls on logs. Know your compliance context (GDPR, etc.).

---

## Your notes / STAR angle
- _TODO: guardrails you implemented and a red-team finding you fixed._
