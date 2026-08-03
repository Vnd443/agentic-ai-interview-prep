# AWS Core — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Starter set (not from the guide PDF). Services on your resume: **IAM, S3, EC2, Lambda, Glue, Athena, Lake Formation, CloudWatch.**

---

**Q1. Explain IAM roles vs. users vs. policies. How do you grant least privilege?**

Users are long-lived identities with permanent credentials, so I avoid them for services. Roles are assumable identities that hand out temporary credentials, and that's what I use for services like Lambda, EC2, and SageMaker, and for cross-account access. Policies are JSON documents attached to identities or resources that allow or deny specific actions. For least privilege I start from nothing and grant only the specific actions on the specific resources needed, scope them further with conditions, and audit with Access Analyzer to catch anything over-broad. The power move is that services get roles, never hardcoded keys — temporary credentials via role assumption, because a hardcoded access key in code is the classic breach.

---

**Q2. Walk me through a serverless data pipeline on AWS.**

Data lands in S3, and an event triggers either a Lambda for light work or a Glue job for heavier ETL. The transformed data goes back to S3, partitioned — usually by date — so queries scan less. The Glue Data Catalog tracks the schema, and then I query it with Athena, which is serverless SQL directly over S3 so there's no database to run. Lake Formation governs access at the table and column level, above raw bucket policies. CloudWatch handles logs, metrics, and alarms across the whole thing, and I orchestrate the steps with Step Functions or EventBridge. The reason to partition for Athena is cost — Athena charges per data scanned, so partitioning is the difference between scanning a day and scanning the whole lake.

---

**Q3. When would you use Glue versus Lambda, and what does Lake Formation add over bucket policies?**

Lambda is for light, fast, event-driven transforms — it has a short timeout and limited memory, so it's the glue between services. Glue is for heavier ETL: large batch jobs, Spark-scale transforms, and it comes with the Data Catalog. So I reach for Lambda when the transform is small and event-triggered, and Glue when it's a real ETL workload. Lake Formation adds fine-grained governance that bucket policies simply can't express — column-level and row-level access control over cataloged tables — so I can let one team query a table but hide the salary column, rather than the all-or-nothing access a bucket policy gives.

---

**Q4. What S3 basics does an interviewer expect?**

That it's an object store, not a filesystem — buckets hold objects addressed by a key, and the "folders" are really just key prefixes. Storage classes trade retrieval speed for cost — Standard, Infrequent-Access, Glacier — and I match the class to the access pattern. Versioning keeps object history, lifecycle policies automatically age data into cheaper tiers or expire it, and encryption is typically SSE-KMS. Access is controlled through IAM and bucket policies. And prefixes and partitioning matter for query performance and cost, because that's what lets Athena scan only the relevant slice.

---

**Q5. How do you monitor and debug an AWS workload?**

CloudWatch is the core — logs, metrics, alarms, and dashboards — and I emit structured logs from Lambda so they're queryable. For distributed tracing across services I use X-Ray to follow a request end-to-end. I set alarms on the signals that matter: error rate, latency, and throttles, plus cost monitoring so a runaway job doesn't surprise me. The discipline I bring from production work is alerting on SLO breaches and deltas from baseline rather than raw fixed thresholds, because that's what actually correlates with users being affected.

---

## Your notes / STAR angle
- _TODO: an AWS architecture you owned — the services, the scale, and a cost or reliability win with numbers._
