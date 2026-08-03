# Databases & SQL — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Schema for LLM/RAG apps ⭐
- documents/chunks (id, source, content, metadata, hash, embed-model version) —
- vectors (pgvector column / external DB keyed to chunk id) —
- conversations/messages (chat history) —
- feedback/eval (request id, score, 👍/👎) —
- Stamp prompt + model version on every generated row —

## Indexing
- B-tree = sorted side-structure, jump to rows w/o full scan —
- Speeds reads, slows writes + costs storage —
- Index the columns you filter/join on —
- Composite index: leftmost-prefix rule —

## Debug a slow query
- EXPLAIN / EXPLAIN ANALYZE — read the plan —
- Look for full table scans, missing indexes, bad row estimates —
- Fix: add/adjust index, rewrite, or denormalise hot path —

## Normalize vs. denormalize
- Normalize (3NF) = no redundancy, consistent writes — OLTP default —
- Denormalize = duplicate for read-heavy paths — measure first —

## Transactions / ACID
- Atomicity, Consistency, Isolation, Durability —
- Isolation levels: read committed → repeatable read → serializable —

## SQL to be fluent in
- JOIN types, GROUP BY + HAVING —
- Window functions (ROW_NUMBER, RANK, running totals) —
- Subqueries vs. CTEs —
- Top-N-per-group = ROW_NUMBER() OVER (PARTITION BY … ORDER BY …) —

## pgvector vs. dedicated vector DB
- pgvector = vectors beside relational data, one system, hybrid filters —
- Dedicated (Pinecone/Weaviate/Milvus) = huge scale + ANN tuning —

## Numbers / facts worth quoting
- Version-stamp every generated row → reproducible incidents —
- Index = faster reads at the cost of slower writes —
