# IaC & DevOps — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> How your systems ship and stay reproducible. Deploys the infra behind [[17-aws-core]] and [[18-aws-bedrock-sagemaker]]; the eval gate ties to [[06-evaluation]] and [[16-monitoring-and-debugging]].

---

## 1. Infrastructure as Code (a blueprint, not hand-built)

**Definition:** **IaC** means defining your infrastructure — servers, networks, buckets, permissions — in **version-controlled code** instead of clicking in a console. The code is the source of truth; you can recreate the whole environment from it.

**Example — IKEA instructions vs. a hand-carved chair:** with the instructions (code), anyone can rebuild the exact same chair, a hundred times, identically. A hand-carved one (console clicks) can never be reproduced precisely, and only the carver knows how.

**Why it matters:** Reproducibility, review, and disaster recovery. *"Infra in code means every environment is identical, changes are peer-reviewed in Git, and I can rebuild prod from scratch — no snowflake servers."*

---

## 2. Terraform — declarative provisioning

**Definition:** **Terraform** **provisions** cloud resources **declaratively**: you describe the *desired end state*, and it computes the diff (**`plan`**) and makes reality match (**`apply`**). It creates/updates/destroys the resources themselves.

**Example:** a **thermostat** — you set "21°C" (desired state) and it figures out whether to heat or cool. You don't script "turn on for 10 minutes"; you declare the target.

**Why it matters:** Declarative + plan-before-apply is safer than imperative scripts. *"Terraform is declarative — I describe what should exist, review the plan diff, then apply. It manages the 'what exists' layer of the stack."*

---

## 3. Terraform state (and why remote + locking)

**Definition:** Terraform keeps **state** — a mapping from your config to the real resources it created. On a **team**, store state **remotely** (S3 + DynamoDB lock, or Terraform Cloud) so it's shared and **locked** against concurrent writes. **Never commit state** (it can hold secrets).

**Example:** state is the **master inventory list** of what Terraform built. If two people edit the warehouse from two copies of the list at once, the inventory corrupts — so you keep one shared list and let only one person write at a time (locking).

**Why it matters:** State corruption is a classic team failure. *"State maps config to real resources; on a team I use remote state with locking so concurrent applies can't corrupt it, and state never goes in Git because it holds secrets."* **Drift** = reality diverging from state (someone clicked in the console); `plan` surfaces it.

---

## 4. Ansible — configuration management

**Definition:** **Ansible** handles **configuration**: installing packages, editing config files, deploying apps *onto* servers. It's **agentless** (works over SSH) and **idempotent** (running it twice leaves the same result).

**Example:** if Terraform **builds the empty house** (walls, plumbing, power), Ansible is the **crew that furnishes and wires it up** — installs the appliances, sets the thermostat, hangs the pictures. And idempotent means: run the crew again and they only fix what's missing, not re-buy furniture you already have.

**Why it matters:** *"Ansible configures what runs on the infra, agentless over SSH, and it's idempotent so re-running is safe and converges to the same state."*

---

## 5. Terraform provisions, Ansible configures (they compose)

**Definition:** The canonical split: **Terraform creates the infrastructure** (the "what exists"), **Ansible configures what runs on it** (the "how it's set up"). They're complementary, not competitors.

**Example:** Terraform pours the foundation and frames the building; Ansible paints the rooms and installs the furniture. Different jobs on the same project.

**Why it matters:** *The* power move for this topic. *"Terraform for the 'what exists', Ansible for the 'how it's configured' — they compose. I don't pick one; I use each for its layer."*

---

## 6. CI/CD (automate the path to production)

**Definition:** **CI (Continuous Integration)** = automatically build and test every change. **CD (Continuous Delivery/Deployment)** = automatically ship it through environments to production. A **pipeline** runs ordered **stages**, failing fast on any gate.

**Example:** a **car assembly line with quality checkpoints** — the vehicle only advances if it passes each station; a failed inspection stops the line before a defect reaches the customer.

**Why it matters:** Faster, safer, repeatable releases. *"CI/CD turns 'deploy' from a risky manual event into an automated, gated, repeatable pipeline — small changes, shipped often, each one tested."*

---

## 7. A CI/CD pipeline for an AI service (with the eval gate) ⭐

**Definition:** Stages for an AI service: **lint/test** → **build** (Docker image) → **eval gate** (run the LLM eval suite; **fail on quality regression**) → **security scan** → deploy to **staging** → **smoke test** → **manual approval** → deploy to **prod** (canary/blue-green). Secrets live in CI variables; Terraform handles infra changes.

**Example:** the assembly line above, but with an extra inspector who **tastes the food** (runs the eval suite) before it leaves the kitchen — a prompt tweak that quietly drops faithfulness fails the inspection and never ships.

**Why it matters:** The differentiator that shows AI-production maturity. *"For AI services I add an eval-suite gate in CI so a prompt or model change can't ship a quality regression — a prompt change is a deploy and gets the same gate as code."* (→ [[06-evaluation]], [[16-monitoring-and-debugging]])

---

## 8. Safe rollout & rollback — canary vs. blue-green

**Definition:** **Canary** = release to a **small % of traffic** first, watch metrics, then ramp up. **Blue-green** = run two full environments and **switch traffic** from old (blue) to new (green), enabling instant rollback by switching back.

**Example:** canary = a **coal-mine canary / small taste test** — expose a few first and watch for trouble. Blue-green = a **two-stage theatre** where you flip the audience from one fully-set stage to the other, and flip back instantly if the new one flops.

**Why it matters:** *"Canary to limit blast radius on a slice of traffic, blue-green for instant switch-and-rollback — either way I can revert fast when a deploy goes wrong."*

---

## 9. Secrets management

**Definition:** Never hardcode credentials or commit them (or Terraform state) to Git. Use a **secrets manager** (AWS Secrets Manager, Vault) or **CI variables**, injected at runtime with least-privilege access.

**Example:** you don't **tape the safe combination to the safe** or write it in the shared notebook — you keep it in a locked key box that only the right people can open, handed out when needed.

**Why it matters:** Leaked secrets are a top breach vector. *"Secrets go in a manager or CI variables, injected at runtime, never in code or state — and scoped least-privilege."* (→ [[14-safety-guardrails]], [[17-aws-core]])

---

## Quick misconceptions to avoid
- ❌ "Terraform and Ansible do the same thing." → Terraform **provisions** infra; Ansible **configures** it — they compose.
- ❌ "Just apply Terraform directly." → Review the **`plan`** diff first, and run it in CI.
- ❌ "Commit the state file so the team shares it." → **Never** — use **remote state + locking**; state holds secrets.
- ❌ "Clicking in the console is fine for a quick fix." → That causes **drift**; change it in code or `plan` will fight you.
- ❌ "A CI pipeline for AI is the same as for any app." → Add an **eval gate** — a prompt/model change can regress quality silently.
- ❌ "Rollback = redeploy the old code and wait." → Use **canary/blue-green** for fast, low-risk revert.

---
_Related: [[17-aws-core]] · [[18-aws-bedrock-sagemaker]] · [[06-evaluation]] · [[16-monitoring-and-debugging]] · [[20-deployment-serving]] · [[14-safety-guardrails]]_
