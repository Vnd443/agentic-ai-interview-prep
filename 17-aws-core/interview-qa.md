# AWS Core — Interview Q&A

> Starter set (not from the guide PDF). Services on your resume: **IAM, S3, EC2, Lambda, Glue, Athena, Lake Formation, CloudWatch.**

---

## Q1. Explain IAM roles vs. users vs. policies. How do you grant least privilege?

**Ideal answer:** **Users** = long-lived identities (avoid for services). **Roles** = assumable identities with temporary credentials — use these for services (Lambda, EC2, SageMaker) and cross-account access. **Policies** = JSON documents attached to identities/resources granting/denying actions. **Least privilege:** start from nothing, grant only the specific actions on specific resources needed, scope with conditions, and audit with Access Analyzer.

**🔑 Power move:** "Services get roles, never hardcoded keys — temporary credentials via role assumption."

**Follow-ups:** Resource-based vs. identity-based policies? How does a Lambda get S3 access?

---

## Q2. Walk through a serverless data pipeline on AWS.

**Ideal answer:** Data lands in **S3** → event triggers **Lambda** (or **Glue** job for heavier ETL) → transformed data back to S3 (partitioned) → **Glue Data Catalog** tracks schema → query with **Athena** (serverless SQL over S3) → **Lake Formation** governs table/column access → **CloudWatch** for logs/metrics/alarms. Orchestrate with Step Functions/EventBridge.

**Follow-ups:** When Glue vs. Lambda? Why partition S3 data for Athena (cost/scan)? What does Lake Formation add over bucket policies?

---

## Q3. How do you monitor and debug an AWS workload?

**Ideal answer:** **CloudWatch** logs + metrics + alarms + dashboards; structured logging from Lambda; X-Ray for tracing; alarms on error rate/latency/throttles; and cost monitoring. Alert on SLO breaches, not raw thresholds.

---

## Q4. S3 basics an interviewer expects.

**Ideal answer:** Object store (not a filesystem); buckets + keys; storage classes for cost (Standard/IA/Glacier); versioning; lifecycle policies; encryption (SSE-KMS); access via IAM + bucket policies; and prefixes/partitioning for query performance.

---

## Your notes / STAR angle
- _TODO: an AWS architecture you owned — services, scale, cost/reliability wins._
