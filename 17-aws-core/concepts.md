# AWS Core — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> The cloud fundamentals behind your data/AI work. ML services live in [[18-aws-bedrock-sagemaker]]; automating this in [[19-iac-and-devops]].

---

## 1. IAM — who can do what (and the golden rule)

**Definition:** **IAM (Identity and Access Management)** controls *who* (identity) can do *what* (action) on *which* resource. It's the front door to everything in AWS.

**Example — a building's badge system:** IAM decides which badge opens which door. Get it wrong and either nobody can work or a stranger walks into the vault.

**Why it matters:** It's the #1 AWS interview and security topic, and misconfigured IAM is the top cloud breach cause. The golden rule is **least privilege** (§3). *"IAM is where I start any AWS design — least privilege by default, everything else builds on it."*

---

## 2. Users vs. roles vs. policies

**Definition:**
- **Users** — long-lived human identities with permanent credentials. Avoid for services.
- **Roles** — *assumable* identities that hand out **temporary** credentials. Use for services (Lambda, EC2, SageMaker) and cross-account access.
- **Policies** — JSON documents attached to identities/resources that **allow or deny** specific actions.

**Example:** a **user** is your permanent employee badge. A **role** is a **visitor pass** you assume for a specific job and hand back — no permanent keys left lying around. A **policy** is the printed list of doors a badge opens.

**Why it matters:** *The* power move here. *"Services get roles, never hardcoded keys — temporary credentials via role assumption."* Hardcoded access keys in code are the classic breach.

---

## 3. Least privilege (start from zero)

**Definition:** Grant **only** the specific actions on the specific resources an identity needs — nothing more. Start from nothing and add; scope with **conditions**; audit with **IAM Access Analyzer**.

**Example:** a **hotel key that opens only your room** and the gym, not every room on the floor. If it's lost, the blast radius is one room.

**Why it matters:** Limits the damage of any leak or compromise. *"I start from deny-all and grant the minimum, scope with conditions, and run Access Analyzer to catch over-broad grants."* (→ [[14-safety-guardrails]])

---

## 4. S3 — object storage (not a filesystem)

**Definition:** **S3** is an **object store**: **buckets** hold **objects** addressed by a **key**. It's not a filesystem — there are no real folders, just key prefixes. Massively durable and scalable.

**Example:** a **giant coat-check**. You hand over an item (object) and get a ticket (key); you don't care which shelf it's on. It's not your closet at home (a filesystem with folders) — it's flat storage you retrieve by ticket.

**Why it matters:** S3 is the backbone of nearly every AWS data/AI pipeline — the data lake, model artifacts, logs. *"S3 is object storage addressed by key — durable, cheap, and the default landing zone for data."*

---

## 5. S3 cost & data management (classes, versioning, lifecycle, partitioning)

**Definition:** Key features: **storage classes** (Standard / Infrequent-Access / Glacier) trade retrieval speed for cost; **versioning** keeps object history; **lifecycle policies** auto-move or expire old data; **encryption** (SSE-KMS); and **prefixes/partitioning** organize data for cheap querying.

**Example:** a **warehouse with shelves and deep archives** — fast front shelves for daily items (Standard), cheap deep storage for records you rarely pull (Glacier), and a rule that automatically moves old stock to the back (lifecycle).

**Why it matters:** *"I match storage class to access pattern, set lifecycle rules to age data into cheaper tiers, and partition by date/key so Athena scans less — that's where the cost savings live."* (→ [[15-cost-optimization]])

---

## 6. Compute — EC2 vs. Lambda (server vs. serverless)

**Definition:** **EC2** = virtual servers you run and manage (always-on, full control). **Lambda** = **serverless** functions that run on an event and scale to zero — you pay per invocation, no servers to manage.

**Example:** EC2 is **owning a car** — always available, you maintain it, you pay whether or not you drive. Lambda is a **taxi** — appears when you call, you pay per trip, disappears after.

**Why it matters:** *"Lambda for event-driven, spiky, or glue workloads — no idle cost; EC2 (or containers) for long-running, high-throughput, or stateful services."*

---

## 7. The serverless data pipeline (the walkthrough)

**Definition:** The canonical AWS data flow interviewers ask you to draw:
**S3** (data lands) → event triggers **Lambda** (light) or a **Glue** job (heavy ETL) → transformed data back to **S3** (partitioned) → **Glue Data Catalog** tracks schema → query with **Athena** (serverless SQL over S3) → **Lake Formation** governs access → **CloudWatch** for logs/metrics/alarms. Orchestrate with **Step Functions / EventBridge**.

**Example:** a **mailroom + filing system**: mail arrives (S3), a clerk sorts it (Lambda/Glue), files it by date (partitioned S3), updates the index card (Data Catalog), and anyone can look things up (Athena) — but only through the records desk that checks permissions (Lake Formation).

**Why it matters:** Being able to narrate this end-to-end is the core AWS-data skill on your resume. *"Data lands in S3, Glue or Lambda transforms it, the Catalog tracks schema, Athena queries it, Lake Formation governs it, CloudWatch watches it."*

---

## 8. Glue vs. Athena vs. Lake Formation

**Definition:**
- **Glue** — managed **ETL** (extract/transform/load) + the **Data Catalog** (schema/metadata store).
- **Athena** — **serverless SQL** directly over S3; pay per data scanned.
- **Lake Formation** — fine-grained **governance** (table/column/row access) over the lake, above raw bucket policies.

**Example:** **Glue** is the prep cook (cleans and reshapes ingredients + keeps the recipe index). **Athena** is the waiter who fetches exactly what you order from the pantry. **Lake Formation** is the manager deciding which staff can see which shelves.

**Why it matters:** *"Glue for ETL and cataloging, Athena for ad-hoc SQL without a database, Lake Formation for column-level governance that bucket policies can't express."* Partitioning S3 data cuts Athena's scan cost.

---

## 9. CloudWatch — logs, metrics, alarms

**Definition:** **CloudWatch** is the observability service: **logs** (from Lambda etc.), **metrics** (latency, errors, throttles), **alarms**, and **dashboards**. Pair with **X-Ray** for distributed tracing.

**Example:** the **dashboard + warning lights in a car** — speed and fuel (metrics), the trip log (logs), and the check-engine alert (alarm) when something crosses a line.

**Why it matters:** Ties AWS to production discipline. *"I use CloudWatch for logs, metrics, and alarms, X-Ray for tracing, and I alert on SLO breaches — error rate, latency, throttles — not raw thresholds."* (→ [[16-monitoring-and-debugging]])

---

## 10. App/agent state — RDS vs. DynamoDB

**Definition:** For application/agent state: **RDS (PostgreSQL)** = managed **relational** DB for structured, related data and complex queries. **DynamoDB** = managed **key-value/NoSQL** for high-scale, low-latency lookups by key.

**Example:** **RDS is a filing cabinet with cross-referenced folders** (joins, relationships); **DynamoDB is a coat-check with a ticket** — instant retrieval by key, but no rich cross-referencing.

**Why it matters:** *"RDS Postgres when I need relationships, joins, and transactions; DynamoDB when I need massive scale and single-key lookups — and for vectors, pgvector on RDS keeps it in one database."* (→ [[23-databases-sql]], [[04-embeddings-vector-search]])

---

## Quick misconceptions to avoid
- ❌ "Use IAM users for my app/service." → Use **roles** with temporary credentials; never hardcode keys.
- ❌ "S3 is a filesystem with folders." → It's an **object store**; 'folders' are just **key prefixes**.
- ❌ "Grant broad permissions, tighten later." → Start **least privilege**; broad grants are the top breach cause.
- ❌ "Lambda is always cheaper than EC2." → Great for spiky/event work; **long-running high-throughput** can be cheaper on EC2/containers.
- ❌ "Athena needs a database." → It's **serverless SQL over S3** — but **partition** or you pay to scan everything.
- ❌ "Bucket policies are enough governance." → **Lake Formation** adds column/row-level control bucket policies can't.

---
_Related: [[18-aws-bedrock-sagemaker]] · [[19-iac-and-devops]] · [[23-databases-sql]] · [[16-monitoring-and-debugging]] · [[14-safety-guardrails]] · [[15-cost-optimization]]_
