# Deployment & API Serving — Cheatsheet

> Fill each `—` with your own one-line explanation from memory. Test recall.

## Big picture
- Notebook → service (what serving adds) —
- The "last mile" —

## API layer
- FastAPI (async, request/response models) —
- Streaming with SSE —
- Frontend surface (Streamlit / thin UI) —

## Packaging
- Docker image —
- Slim base image / .dockerignore —
- ECR (image registry) —

## Where it runs
- ECS Fargate (default for steady API) —
- Lambda (short, spiky, event-driven) —
- EC2 (full control / GPU) —
- Choose by: workload shape —

## Reliability
- Secrets Manager / runtime env vars (never in image) —
- Health / readiness checks —
- Rate limiting —
- Timeouts —
- Autoscaling & load balancing —

## Long runs
- Queue + async worker —
- Job id + poll / notify —

## Ship
- CI/CD: build → push → deploy —
- Health-check-gated rollout —
