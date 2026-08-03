# AWS Bedrock & SageMaker — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> Your cloud-AI surface. Builds on [[17-aws-core]]; RAG mechanics in [[05-rag]]; fine-tuning in [[24-fine-tuning]].

---

## 1. The core split — Bedrock vs. SageMaker (rent vs. own)

**Definition:** **Bedrock** = managed access to **foundation models via API** — no infra, pay-per-token. **SageMaker** = a full ML platform to **train, fine-tune, and host your own models** — you manage the instances and endpoints.

**Example — transport:** Bedrock is **calling a ride** — you just go, someone else owns and maintains the car. SageMaker is **owning and running a garage** — full control over the vehicles, but you handle maintenance, fuel, and parking.

**Why it matters:** *The* boundary interviewers test. *"Bedrock to ship fast on managed FMs; SageMaker when I need custom training/hosting or the data can't leave my account."* Know which side any requirement falls on.

---

## 2. Bedrock — managed foundation models

**Definition:** **Bedrock** gives one API over multiple FM providers (Anthropic Claude, Amazon Titan, Cohere, Meta, etc.). No servers, pay per token, and — crucially — **your data stays in your AWS account** and isn't used to train the base models.

**Example:** a **food court with one tray and one till** — many kitchens (model providers), one ordering counter (the API). You switch cuisines without learning a new checkout.

**Why it matters:** Fastest path to a GenAI app on AWS with enterprise data governance built in. *"Bedrock is a single managed API over many FMs, and data residency in-account is why enterprises pick it over calling a public API."*

---

## 3. Bedrock building blocks — Knowledge Bases, Agents, Guardrails

**Definition:** Managed higher-level features:
- **Knowledge Bases** — managed **RAG**: handles ingest → chunk → embed → store → retrieve for you.
- **Agents** — managed tool-use/orchestration (the agent loop, hosted).
- **Guardrails** — managed input/output **safety filtering** (PII, toxicity, denied topics).

**Example:** buying **pre-assembled furniture** vs. cutting the wood yourself. Knowledge Bases is a RAG pipeline that arrives built; you skip wiring chunking and a vector store by hand.

**Why it matters:** Trade some control for speed and less ops. *"I use Knowledge Bases when I want managed RAG fast, and roll my own when I need control over chunking, hybrid search, or re-ranking."* (→ [[05-rag]], [[14-safety-guardrails]])

---

## 4. SageMaker — the model lifecycle platform

**Definition:** **SageMaker** covers the whole custom-model lifecycle: **train**, **fine-tune**, and **host** (deploy behind endpoints), plus notebooks, pipelines, and **Model Monitor** for drift.

**Example:** a **fully-equipped workshop** — machines to build (train), refit (fine-tune), and a showroom to sell from (host). You own the process end to end.

**Why it matters:** Where you go when a managed FM isn't enough. *"SageMaker is for when I own the model — custom training or fine-tuning, specialized hosting, or strict control over the deployment."* (→ [[24-fine-tuning]])

---

## 5. SageMaker endpoint types (match to the workload)

**Definition:** Three hosting modes:
- **Real-time** — low-latency, always-on, one request at a time. For interactive apps.
- **Async** — queued, for **large payloads** or longer processing.
- **Batch transform** — **offline** bulk scoring of a dataset, no persistent endpoint.

**Example:** a **restaurant** (real-time, cook to order), a **dry cleaner** (async — drop off, pick up later), and a **catering run** (batch — cook 500 meals at once, offline).

**Why it matters:** Picking the wrong one wastes money or breaks latency. *"Real-time for interactive, async for big payloads, batch transform for offline bulk — I match the endpoint to the traffic shape."*

---

## 6. RAG on AWS (the full stack)

**Definition:** The AWS-native RAG pattern: **embeddings** via Bedrock (Titan/Cohere) → **vector store** (OpenSearch Serverless, **pgvector** on RDS/Aurora, or managed **Bedrock Knowledge Bases**) → **generation** via a Bedrock FM → **Bedrock Guardrails** for safety. Orchestrate with **Lambda / Step Functions**.

**Example:** the same open-book-exam idea from [[05-rag]], assembled from AWS parts — Bedrock embeds and answers, a vector store holds the "textbook," Knowledge Bases can run the whole retrieval loop for you.

**Why it matters:** A common "build it on AWS" whiteboard. *"Bedrock for embeddings and generation, a managed vector store or Knowledge Bases for retrieval, Guardrails for safety, Lambda/Step Functions to orchestrate."*

---

## 7. Autoscaling & cost

**Definition:** SageMaker endpoints **autoscale** on invocation metrics; **multi-model endpoints** pack several models on one endpoint to save cost; scale-to-zero options avoid paying for idle. Bedrock is pay-per-token (plus **provisioned throughput** for guaranteed capacity).

**Example:** **staffing a shop by footfall** — more cashiers at rush hour, fewer overnight, and one till serving several small departments (multi-model) instead of one each.

**Why it matters:** *"I autoscale endpoints on invocation load, use multi-model endpoints to avoid idle cost, and on Bedrock I reach for provisioned throughput only when I need guaranteed capacity."* (→ [[15-cost-optimization]])

---

## 8. VPC & data residency

**Definition:** For sensitive data, keep traffic **inside your VPC** (private endpoints, no public internet), and rely on Bedrock/SageMaker keeping data **in-account**. Meets compliance/residency requirements.

**Example:** a **private internal mail system** instead of the public post — nothing sensitive leaves the building.

**Why it matters:** Often *the* reason enterprises choose AWS AI over public APIs. *"When data can't leave our boundary, I run Bedrock/SageMaker in-VPC — data residency and in-account processing are the enterprise selling point."* (→ [[14-safety-guardrails]])

---

## Quick misconceptions to avoid
- ❌ "Bedrock and SageMaker overlap, pick either." → Bedrock = **call managed FMs**; SageMaker = **own the model lifecycle**. Different jobs.
- ❌ "Bedrock trains on my prompts." → No — data stays **in your account**, not used to train base models.
- ❌ "One endpoint type fits all." → **Real-time / async / batch** match different traffic shapes.
- ❌ "Knowledge Bases means I never build RAG." → Great for speed, but **roll your own** for control over chunking/hybrid/re-ranking.
- ❌ "SageMaker is just for hosting." → It's **train + fine-tune + host + monitor** — the whole lifecycle.
- ❌ "Cloud AI can't meet data-residency rules." → **In-VPC, in-account** processing is exactly why enterprises pick it.

---
_Related: [[17-aws-core]] · [[05-rag]] · [[24-fine-tuning]] · [[14-safety-guardrails]] · [[15-cost-optimization]] · [[20-deployment-serving]] · [[19-iac-and-devops]]_
