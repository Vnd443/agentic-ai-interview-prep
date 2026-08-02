# IaC & DevOps — Interview Q&A

> Starter set (not from the guide PDF). Your stack: **Terraform, Ansible, GitLab CI/CD, Git.**

---

## Q1. Terraform vs. Ansible — what's the difference and when do you use each?

**Ideal answer:** **Terraform** = declarative **provisioning** of infrastructure (create/manage cloud resources) with **state**; you describe the desired end state and it plans the diff. **Ansible** = **configuration management** / procedural provisioning (install packages, configure servers, deploy apps), agentless over SSH. Common pattern: **Terraform provisions the infra, Ansible configures what runs on it.**

**🔑 Power move:** "Terraform for the 'what exists', Ansible for the 'how it's configured' — they compose."

**Follow-ups:** What is Terraform state and why is remote state + locking important? Idempotency in Ansible?

---

## Q2. What is Terraform state and how do you manage it on a team?

**Ideal answer:** State maps your config to real resources. On a team, store it **remotely** (S3 + DynamoDB lock, or Terraform Cloud) so it's shared and **locked** to prevent concurrent corruption. Never commit state (it holds secrets); use workspaces/modules; run `plan` in CI before `apply`.

**Follow-ups:** What happens on state drift? `plan` vs. `apply` in a pipeline?

---

## Q3. Design a CI/CD pipeline in GitLab for an AI service.

**Ideal answer:** Stages: **lint/test** → build (Docker image) → **eval gate** (run the LLM eval suite; fail on quality regression) → security scan → deploy to staging → smoke test → manual approval → deploy to prod (canary/blue-green). Use GitLab CI `stages`, cached deps, secrets in CI variables, and Terraform for infra changes.

**🔑 Power move:** "For AI services I add an **eval-suite gate** in CI so a prompt/model change can't ship a quality regression."

**Follow-ups:** Rollback strategy? How do you handle secrets? Canary vs. blue-green?

---

## Your notes / STAR angle
- _TODO: a pipeline or IaC setup you built and what it improved (lead time, reliability)._
