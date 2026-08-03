# AWS Bedrock & SageMaker — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Starter set (not from the guide PDF).

---

**Q1. Bedrock vs. SageMaker — when do you use each?**

Bedrock is managed access to foundation models through an API — Anthropic, Titan, Cohere, and others — with no infrastructure to manage and pay-per-token pricing, so it's the fast path for GenAI apps, RAG, and agents. SageMaker is the full ML platform for training, fine-tuning, and hosting my own models, where I manage the instances and endpoints and get more control but also own the cost and ops. The way I frame the choice: Bedrock to ship fast on managed foundation models, SageMaker when I need custom training or hosting, or when the data can't leave my account and I need full control of the deployment. Bedrock is renting a ride; SageMaker is running a garage.

---

**Q2. How do you build RAG on AWS with Bedrock?**

I generate embeddings with a Bedrock model like Titan or Cohere, store them in a vector store — OpenSearch Serverless, or pgvector on RDS/Aurora if I want it in my existing database — and generate answers with a Bedrock foundation model, wrapping the whole thing in Bedrock Guardrails for safety. I orchestrate the steps with Lambda or Step Functions. The shortcut is Bedrock Knowledge Bases, which manages the entire ingest-embed-store-retrieve loop for me. So the trade-off I'd raise is Knowledge Bases when I want managed RAG quickly, versus rolling my own pipeline when I need control over chunking, hybrid search, or re-ranking — the levers that actually drive retrieval quality.

---

**Q3. How do you deploy and scale a model endpoint on SageMaker?**

I package the model with its inference code and deploy it behind an endpoint, choosing the type by workload: real-time for low-latency interactive traffic, async for large payloads or longer processing, and batch transform for offline bulk scoring with no persistent endpoint. I enable autoscaling on invocation metrics so capacity tracks load, and use multi-model endpoints to pack several models onto one endpoint and avoid paying for idle infrastructure. For monitoring I use CloudWatch for the operational metrics and SageMaker Model Monitor to catch data and model drift, which ties back to the production-debugging discipline of watching input distributions.

---

**Q4. Why do enterprises choose Bedrock over calling a public model API?**

The big one is data governance. With Bedrock, my data stays in my AWS account and isn't used to train the base models, and I can run everything inside my VPC with private endpoints so nothing sensitive touches the public internet. That directly satisfies data-residency and compliance requirements that a public API can't. On top of that it's one managed API over many providers, so I can switch or compare models without re-plumbing, and it integrates with the rest of my AWS stack — IAM for access, CloudWatch for monitoring, Guardrails for safety. So it's speed plus enterprise-grade data control in one place.

---

**Q5. When would you use Bedrock Guardrails versus building your own guardrails?**

Bedrock Guardrails is managed input and output filtering — denied topics, PII redaction, toxicity, and prompt-attack filtering — configured rather than coded, so I reach for it to get a solid safety baseline fast and consistently across models. I'd layer my own guardrails on top when I need domain-specific rules it doesn't cover, custom output schema validation, or business-logic checks before a tool call executes. So it's not either-or: Guardrails handles the common safety layer, and I add application-specific validation for the rest, keeping defence in depth.

---

## Your notes / STAR angle
- _TODO: your Bedrock/SageMaker usage — which models, hosting choices, data-residency constraints, and cost outcome._
