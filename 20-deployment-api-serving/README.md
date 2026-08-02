# 20 — Deployment & API Serving

> Turning an LLM app from a notebook into a **running service** — wrapped in an API, containerized, deployed, and able to handle real traffic (streaming, scaling, secrets, health checks).

**Tier:** 1 — Production skill  |  **Interview frequency:** High for FDE / integration roles

## Why this matters for your interviews
A model that only runs on your laptop is a demo, not a product. This is the "last mile" that separates people who *use* LLMs from people who *ship* them. FDE and integration interviewers lean here: can you put a FastAPI in front of an agent, stream tokens back, containerize it, run it on ECS Fargate, and keep secrets out of the image? This is where the capstone stack (FastAPI + Docker + ECS Fargate) lives.

## Subtopics checklist
- [ ] **FastAPI** for LLM apps — request/response models, async endpoints
- [ ] **Streaming responses** — Server-Sent Events (SSE) so tokens appear as they generate
- [ ] **Docker** — containerizing the app; slim images; `.dockerignore`
- [ ] **ECS Fargate** (serverless containers) vs Lambda vs EC2 — when to use which
- [ ] **Autoscaling & load balancing** — handling concurrent requests
- [ ] **Secrets & config** — env vars, AWS Secrets Manager (never bake keys into the image)
- [ ] **Health checks & readiness** — so the orchestrator knows the app is alive
- [ ] **Rate limiting & timeouts** — protecting the app and the model budget
- [ ] **Async workers / queues** — long agent runs off the request thread
- [ ] **CI/CD** — build image → push to registry → deploy (ties to [[19-iac-devops]])
- [ ] **Frontend surface** — Streamlit / a thin UI in front of the API

## Suggested learning flow
1. Read a primary resource in `resources.md` (FastAPI + streaming, ECS Fargate).
2. Write the "notebook → service" path in your own words in `concepts.md`.
3. Distil `cheatsheet.md`.
4. Rehearse `interview-qa.md` out loud.
5. Build a project in `projects/` — wrap an agent in FastAPI, containerize, deploy.

## Interview angles (how they'll probe you)
- "You've got an agent working in a notebook. How do you get it to production?"
- "How do you stream an LLM's response to the user instead of waiting for the full answer?"
- "Where do the API keys go? Not in the Docker image — so where?"
- "Fargate vs Lambda vs EC2 for serving an LLM app — how do you choose?"
- "An agent run takes 40 seconds. How do you handle that without timing out the request?"

## Files in this folder
- **README.md** — this hub.
- **concepts.md** — the notebook→service path, streaming, containers, deploy targets.
- **cheatsheet.md** — terms + one-liners.
- **interview-qa.md** — Q → answer → copy-paste template.
- **resources.md** — primary sources.
- **projects/** — build ideas.

## Status
- [ ] Concepts read & internalized
- [ ] Cheatsheet reviewed
- [ ] Q&A rehearsed out loud
- [ ] Project built / mapped
- [ ] Can teach this topic from memory

_Related: [[17-aws-core]] · [[19-iac-devops]] · [[18-aws-bedrock-sagemaker]] · [[22-llm-system-design]]_
_Back to the plan: [`../00-START-HERE/MASTER-ROADMAP.md`](../00-START-HERE/MASTER-ROADMAP.md)_
