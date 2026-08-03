# Databases & SQL — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Starter set (not from the guide PDF). Your stack: **MySQL, SQLite** (+ pgvector shows up in RAG).

---

**Q1. How do you design a schema for an LLM/RAG application?**

I separate concerns into tables. A documents/chunks table holds the id, source, content, metadata, a content hash, and the embedding-model version. A vectors store keeps the embeddings — either a pgvector column or an external vector DB keyed to the chunk id. A conversations/messages table holds chat history, and a feedback/eval table records the request id, a score, and thumbs up/down. I index the foreign keys and the columns I filter on, and — the part that matters most in production — I stamp every generated row with the prompt and model version. That's what makes a prod incident reproducible: when an answer goes wrong, I can see exactly which prompt and model produced it instead of guessing.

---

**Q2. Explain indexing and how you'd debug a slow query.**

An index, usually a B-tree, is a sorted side-structure that lets the database jump straight to matching rows instead of scanning the whole table — it speeds up lookups, filters, and joins at the cost of slower writes and extra storage. To debug a slow query I start with `EXPLAIN` or `EXPLAIN ANALYZE` and read the plan: I'm looking for full table scans, missing indexes on the WHERE or JOIN columns, and bad row estimates. Then I fix the actual problem — add or adjust an index, rewrite the query, or denormalise a hot path — rather than guessing. And I keep composite index column order in mind, because the leftmost-prefix rule decides which queries the index can actually serve. The cases where an index doesn't help are worth naming too: very small tables, low-selectivity columns, or a leading-wildcard `LIKE`.

---

**Q3. Normalization vs. denormalization — how do you choose?**

I normalize to around third normal form by default, so each fact lives in one place and writes stay consistent — that's the right call for transactional workloads. I denormalize selectively for read-heavy or analytics paths where the join cost genuinely hurts, accepting the duplicated data and the extra work to keep it in sync. So it's a read-versus-write and consistency-versus-speed trade-off, and I only denormalize a hot path after measuring that the joins are the bottleneck — never on a hunch.

---

**Q4. What SQL essentials do interviewers probe?**

The usual set: the JOIN types and when each applies, GROUP BY with HAVING, window functions like `ROW_NUMBER` and `RANK` and running totals, the difference between subqueries and CTEs, and transactions with ACID and isolation levels. I'm ready to write a query live — the classic is top-N per group, where I'd use `ROW_NUMBER() OVER (PARTITION BY … ORDER BY …)` and filter on the rank, because a plain GROUP BY collapses the row detail I need to return.

---

## Your notes / STAR angle
- _TODO: a schema you designed or a query you optimized (before/after latency)._
