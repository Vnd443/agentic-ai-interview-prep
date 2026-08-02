# AWS Bedrock & SageMaker — Interview Q&A

> Starter set (not from the guide PDF).

---

## Q1. Bedrock vs. SageMaker — when do you use each?

**Ideal answer:** **Bedrock** = managed access to foundation models via API (Anthropic, etc.) — no infra to manage, pay-per-token, fast to integrate, good for GenAI apps/RAG/agents. **SageMaker** = full ML platform for **training, fine-tuning, and hosting your own models** — you manage instances/endpoints, more control and cost ownership. Use Bedrock for calling FMs, SageMaker when you own the model lifecycle.

**🔑 Power move:** "Bedrock to ship fast on managed FMs; SageMaker when I need custom training/hosting or data can't leave my account."

**Follow-ups:** Bedrock Knowledge Bases / Agents / Guardrails? SageMaker real-time vs. batch/async endpoints?

---

## Q2. How do you do RAG on AWS with Bedrock?

**Ideal answer:** Embeddings via Bedrock (e.g. Titan/Cohere) → vector store (OpenSearch Serverless, pgvector on RDS/Aurora, or **Bedrock Knowledge Bases** which manages ingest→embed→retrieve) → generation via a Bedrock FM → **Bedrock Guardrails** for safety. Orchestrate with Lambda/Step Functions.

**Follow-ups:** Why Knowledge Bases vs. rolling your own? Data residency / VPC concerns?

---

## Q3. How do you deploy and scale a model endpoint on SageMaker?

**Ideal answer:** Package model + inference code → SageMaker endpoint (real-time for low latency, **async** for large payloads, **batch transform** for offline). Enable autoscaling on invocation metrics; use multi-model endpoints to save cost; monitor with CloudWatch + Model Monitor for drift.

---

## Your notes / STAR angle
- _TODO: your Bedrock/SageMaker usage at IBM/AWS — models, hosting, cost._
