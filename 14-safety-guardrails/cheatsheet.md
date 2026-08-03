# Safety & Guardrails — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Mindset
- Safety scales with capability (blast radius) —
- Wrong sentence vs wrong action —
- Defence in depth (both sides of the model) —

## Input guardrails
- Prompt-injection detection —
- PII detection —
- Topic / off-scope filter —
- Rate limits, block at gateway —

## Output guardrails
- PII leakage —
- Toxic / harmful content —
- Off-topic / policy —
- Schema validation (valid JSON shape) —
- Secondary classifier / rules engine —

## Prompt injection
- Direct (in user message) —
- Indirect (hidden in retrieved doc) ⭐ —
- Core defence: content = data, NOT instructions —
- Separate instructions from data; sanitize/sandbox —

## Action guardrails (agents)
- Least privilege / permissions —
- Tool-call caps + retry limits —
- Read-only enforcement —
- Argument/query validation —
- HITL before irreversible actions —
- OWASP "excessive agency" —

## Red-teaming
- Jailbreaks, injection, data extraction —
- Grade graceful failure —
- Findings → regression tests —

## HITL
- High-stakes: medical / legal / financial —
- Fail gracefully: abstain / escalate —

## PII & privacy
- Redact before logging / 3rd-party —
- On-prem / VPC for sensitive —
- Retention limits + log access control —
- GDPR / HIPAA context —

## Standards (power move)
- OWASP LLM Top 10 (threat checklist) —
- NIST AI RMF (govern/map/measure/manage) —
- AWS Bedrock Guardrails (managed) —

## Numbers / facts worth quoting
- 
