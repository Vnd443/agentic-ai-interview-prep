# Databases & SQL — Interview Q&A

> Starter set (not from the guide PDF). Your stack: **MySQL, SQLite** (+ pgvector shows up in RAG).

---

## Q1. How do you design a schema for an LLM/RAG application?

**Ideal answer:** Separate concerns: a **documents/chunks** table (id, source, content, metadata, content hash, embedding-model version), a **vectors** store (pgvector column or external vector DB keyed to chunk id), a **conversations/messages** table for chat history, and a **feedback/eval** table (request id, score, 👍/👎). Index on foreign keys and filter columns; store the model/prompt version with each generated row for debugging.

**🔑 Power move:** "I stamp every generated row with prompt + model version — that's what makes prod incidents reproducible."

**Follow-ups:** pgvector vs. dedicated vector DB? How do you index metadata for hybrid filtering?

---

## Q2. Explain indexing and how you'd debug a slow query.

**Ideal answer:** An index (usually B-tree) speeds lookups/filters/joins at the cost of write speed and storage. Debug with **`EXPLAIN`/`EXPLAIN ANALYZE`**: look for full table scans, missing indexes on WHERE/JOIN columns, and bad row estimates. Add/adjust indexes, rewrite the query, or denormalise hot paths. Composite index order matters (leftmost prefix).

**Follow-ups:** When does an index *not* help? Covering indexes? N+1 query problem?

---

## Q3. Normalization vs. denormalization — how do you choose?

**Ideal answer:** Normalize (3NF) to remove redundancy and keep writes consistent — good default for OLTP. Denormalize selectively for read-heavy/analytics paths where join cost hurts. It's a read-vs-write and consistency-vs-speed trade-off; measure before denormalising.

---

## Q4. SQL essentials interviewers probe.

**Ideal answer:** JOIN types, GROUP BY + HAVING, window functions (`ROW_NUMBER`, `RANK`, running totals), subqueries vs. CTEs, transactions/ACID, and isolation levels. Be ready to write a query live (e.g. top-N per group with a window function).

---

## Your notes / STAR angle
- _TODO: a schema you designed or a query you optimized (before/after latency)._
