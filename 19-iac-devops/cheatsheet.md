# IaC & DevOps — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Core idea
- Infrastructure as Code = infra in version-controlled code, not console clicks —
- Reproducible, peer-reviewed, rebuildable from scratch (no snowflakes) —

## Terraform (provisions)
- Declarative — describe desired end state —
- `plan` (diff) → `apply` (make reality match) —
- Manages the "what exists" layer —

## Terraform state ⭐
- Maps config → real resources —
- Remote state on a team (S3 + DynamoDB lock / TF Cloud) —
- Locking prevents concurrent corruption —
- Never commit state (holds secrets) —
- Drift = reality diverges from state; `plan` surfaces it —

## Ansible (configures)
- Configuration management — installs/config/deploy onto servers —
- Agentless (over SSH) —
- Idempotent (re-run = same result) —

## The split ⭐
- Terraform provisions, Ansible configures —
- "What exists" vs. "how it's configured" — they compose —

## CI/CD
- CI = build + test every change —
- CD = ship through envs to prod —
- Pipeline = ordered stages, fail fast —

## AI-service pipeline ⭐
- lint/test → build → eval gate → security scan → staging → smoke → approval → prod —
- Eval gate: run LLM eval suite, fail on quality regression —
- A prompt change is a deploy — same gate as code —

## Rollout / rollback
- Canary = small % traffic first, then ramp —
- Blue-green = two envs, switch traffic, instant rollback —

## Secrets
- Secrets manager / CI variables, injected at runtime —
- Never in code or state; least-privilege —

## Numbers / facts worth quoting
- Terraform state locking: 1 writer at a time (DynamoDB lock table) —
- Eval gate blocks a quality regression *before* prod, not after —
