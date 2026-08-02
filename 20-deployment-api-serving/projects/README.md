# Deployment & API Serving — Project Ideas

> Ship one thing end-to-end. A deployed URL you can open in an interview beats any slide.

## Idea 1 — Wrap an agent in FastAPI + stream + deploy (recommended)
Take an agent from [[07-agents-tool-use]] or a RAG app from [[05-rag]], put a FastAPI streaming endpoint in front of it, containerize with Docker, deploy to ECS Fargate.
- **Demo:** hit the live URL, watch tokens stream in.
- **Talking point:** async endpoint, SSE streaming, secrets from Secrets Manager, health check, autoscaling.

## Idea 2 — Long-run job queue
Add an async job pattern: POST starts an agent run, returns a job id; a worker processes it; GET polls status/result.
- **Talking point:** how you avoid request timeouts on 40s+ runs (queue + worker + poll).

## Idea 3 — Full CI/CD pipeline
Automate build → push to ECR → deploy to Fargate on every commit, gated by a health check.
- **Talking point:** repeatable shipping; ties into [[19-iac-devops]].

## Idea 4 — Streamlit front end
Put a thin Streamlit UI in front of the API so it's demoable to non-engineers.
- **Talking point:** the frontend surface from the capstone stack.

## What to capture for the interview
- The live URL (or a recorded demo of streaming).
- A number: p50/p95 latency, or requests/sec it handled.
- One reliability decision (secrets, health check, timeout) and why.

_Related: [[17-aws-core]] · [[19-iac-devops]] · [[22-llm-system-design]] · [[18-aws-bedrock-sagemaker]]_
