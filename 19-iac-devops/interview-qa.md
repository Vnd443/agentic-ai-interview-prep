# IaC & DevOps — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Starter set (not from the guide PDF). Your stack: **Terraform, Ansible, GitLab CI/CD, Git.**

---

**Q1. Terraform vs. Ansible — what's the difference and when do you use each?**

Terraform is declarative provisioning of infrastructure — it creates and manages cloud resources, keeps state, and works by me describing the desired end state so it can plan the diff and apply it. Ansible is configuration management: installing packages, editing config, and deploying apps onto servers, agentless over SSH and idempotent so re-running is safe. The common pattern is that they compose — Terraform provisions the infra, Ansible configures what runs on it. The way I frame it in one line: Terraform for the "what exists", Ansible for the "how it's configured" — they're complementary, not competitors, so I don't pick one, I use each for its layer.

---

**Q2. What is Terraform state and how do you manage it on a team?**

State is the mapping from my config to the real resources Terraform created, so it knows what already exists and what to change. On a team I store it remotely — S3 with a DynamoDB lock table, or Terraform Cloud — so the state is shared and locked, which stops two concurrent applies from corrupting it. I never commit state to Git because it can hold secrets, I split things with modules and workspaces, and I run `plan` in CI so the diff is reviewed before anyone applies. If reality drifts from state — someone clicked in the console — `plan` surfaces it as a diff, which is exactly why I keep changes in code instead of the console.

---

**Q3. Design a CI/CD pipeline in GitLab for an AI service.**

I'd run ordered stages: lint and test, then build the Docker image, then an eval gate that runs the LLM eval suite and fails the pipeline on a quality regression, then a security scan, deploy to staging, a smoke test, a manual approval, and finally deploy to prod with a canary or blue-green rollout. I use GitLab CI stages with cached dependencies, keep secrets in CI variables rather than code, and let Terraform handle any infra changes as part of the flow. The piece that matters for AI specifically is that eval-suite gate — a prompt or model change is a deploy, so it gets the same gate as code and can't quietly ship a quality regression. For rollback I lean on blue-green for an instant traffic switch back, or canary to limit blast radius on the way up.

---

## Your notes / STAR angle
- _TODO: a pipeline or IaC setup you built and what it improved (lead time, reliability)._
