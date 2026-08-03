# AWS Bedrock & SageMaker — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## The core split ⭐
- Bedrock = managed FMs via API (rent a ride) —
- SageMaker = train/fine-tune/host your own (own a garage) —
- Bedrock to ship fast; SageMaker when you own the model or data can't leave —

## Bedrock
- One API over many FM providers —
- Pay-per-token —
- Data stays in-account (not used to train) —
- Provisioned throughput for guaranteed capacity —

## Bedrock building blocks
- Knowledge Bases = managed RAG —
- Agents = managed tool-use/orchestration —
- Guardrails = managed input/output safety —

## SageMaker
- Full lifecycle: train / fine-tune / host —
- Notebooks, Pipelines, Model Monitor (drift) —

## Endpoint types
- Real-time (low latency, interactive) —
- Async (large payloads, queued) —
- Batch transform (offline bulk) —

## RAG on AWS
- Embed: Bedrock (Titan/Cohere) —
- Store: OpenSearch Serverless / pgvector / Knowledge Bases —
- Generate: Bedrock FM —
- Safety: Bedrock Guardrails —
- Orchestrate: Lambda / Step Functions —

## Cost & scale
- Autoscale on invocation metrics —
- Multi-model endpoints (share, save cost) —
- Scale-to-zero for idle —

## VPC / residency
- In-VPC, in-account = enterprise selling point —
- Compliance / data residency —

## Numbers / facts worth quoting
- 
