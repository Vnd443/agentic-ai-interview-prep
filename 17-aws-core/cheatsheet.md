# AWS Core — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## IAM
- Who / what / which resource —
- Users (long-lived) vs roles (assumable, temporary) vs policies (JSON) —
- Services get roles, never hardcoded keys ⭐ —
- Least privilege: start from zero, scope with conditions —
- Audit with Access Analyzer —
- Identity-based vs resource-based policies —

## S3
- Object store, not a filesystem (key prefixes) —
- Buckets + objects + keys —
- Storage classes: Standard / IA / Glacier —
- Versioning + lifecycle policies —
- Encryption (SSE-KMS) —
- Partition/prefix for query cost —

## Compute
- EC2 = servers you manage (own a car) —
- Lambda = serverless, event-driven, scale to zero (taxi) —
- Lambda for spiky/glue; EC2 for long-running/stateful —

## Serverless data pipeline (walkthrough)
- S3 land → Lambda/Glue transform → S3 partitioned —
- Glue Data Catalog (schema) —
- Athena (serverless SQL over S3) —
- Lake Formation (governance) —
- CloudWatch (logs/metrics/alarms) —
- Orchestrate: Step Functions / EventBridge —

## Glue / Athena / Lake Formation
- Glue = ETL + Data Catalog —
- Athena = SQL over S3, pay per scan —
- Lake Formation = column/row governance —
- Partition to cut Athena scan cost —

## Monitoring
- CloudWatch logs / metrics / alarms / dashboards —
- X-Ray tracing —
- Alert on SLO breach, not raw thresholds —

## State stores
- RDS PostgreSQL = relational (joins, transactions) —
- DynamoDB = key-value/NoSQL (scale, single-key) —
- pgvector on RDS = vectors in one DB —

## Numbers / facts worth quoting
- 
