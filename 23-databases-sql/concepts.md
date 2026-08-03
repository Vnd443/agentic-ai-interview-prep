# Databases & SQL — Concepts

> Read top-to-bottom the first time. Each idea = **plain definition → everyday analogy → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> The data layer under every app. Schema design for LLM apps, indexing, and live SQL are the usual probes. Ties to the vector store in [[04-embeddings-vector-search]] and [[05-rag]].

---

## 1. Schema design for an LLM/RAG app ⭐

**Definition:** Separate concerns into tables: **documents/chunks** (id, source, content, metadata, content hash, embedding-model version), a **vectors** store (pgvector column or external vector DB keyed to chunk id), **conversations/messages** for chat history, and a **feedback/eval** table (request id, score, 👍/👎). Index foreign keys and filter columns; **stamp every generated row with the prompt + model version**.

**Example:** a **well-organised warehouse** — raw stock (documents), a location index (vectors), the order log (messages), and a returns/quality log (feedback), each in its own aisle rather than one giant pile.

**Why it matters:** The version stamp is the power move. *"I stamp every generated row with prompt + model version — that's what makes a prod incident reproducible instead of a mystery."*

---

## 2. Indexes (a book's index, not the whole book)

**Definition:** An **index** (usually a **B-tree**) is a sorted side-structure that lets the DB jump to matching rows without scanning the whole table. It speeds reads/filters/joins at the cost of **slower writes** and extra storage.

**Example:** the **index at the back of a textbook** — you find "photosynthesis, p.240" instantly instead of flipping every page. But every time the book changes, the index has to be updated too.

**Why it matters:** The core read-vs-write trade-off. *"An index trades write speed and storage for fast lookups — I add them on the columns I filter and join on, not everywhere."*

---

## 3. Debugging a slow query (EXPLAIN first)

**Definition:** Use **`EXPLAIN` / `EXPLAIN ANALYZE`** to see the query plan. Look for **full table scans**, **missing indexes** on WHERE/JOIN columns, and **bad row estimates**. Fix by adding/adjusting indexes, rewriting the query, or denormalising a hot path. **Composite index order matters** (leftmost-prefix rule).

**Example:** a **GPS trip replay** showing where you got stuck in traffic — you don't guess the slow leg, you look at the actual route and fix that segment.

**Why it matters:** Guessing at performance is a red flag. *"I always `EXPLAIN ANALYZE` before touching anything — a full table scan on a filtered column is usually a missing index, not a query I need to rewrite."*

---

## 4. Normalization vs. denormalization

**Definition:** **Normalize** (to ~3NF) to remove redundancy so each fact lives in one place — the safe OLTP default. **Denormalize** selectively (duplicate data) for read-heavy/analytics paths where join cost hurts. It's a read-vs-write and consistency-vs-speed trade-off.

**Example:** normalized = one **master address book** everyone references; denormalized = **writing the address on every envelope** so you don't look it up — faster to send, but a move means updating many envelopes.

**Why it matters:** *"Normalize by default for consistent writes; denormalize a hot read path only after measuring — never on a hunch."*

---

## 5. Transactions & ACID

**Definition:** A **transaction** groups operations so they succeed or fail as a unit. **ACID** = **A**tomicity (all-or-nothing), **C**onsistency (valid state to valid state), **I**solation (concurrent txns don't corrupt each other), **D**urability (committed = survives a crash).

**Example:** a **bank transfer** — debit one account and credit the other must both happen or neither; you never lose the money in between.

**Why it matters:** *"ACID transactions are why a partial failure doesn't leave corrupt data — debit and credit commit together or not at all."* **Isolation levels** (read committed, repeatable read, serializable) trade concurrency for consistency.

---

## 6. SQL essentials interviewers probe

**Definition:** Be fluent in **JOIN** types, **GROUP BY + HAVING**, **window functions** (`ROW_NUMBER`, `RANK`, running totals), **subqueries vs. CTEs**, and transactions/isolation. Expect to write a query live — a classic is **top-N per group** with a window function.

**Example:** window functions are like **ranking each student within their class** while still seeing the whole school in one result — you don't collapse the rows the way GROUP BY does.

**Why it matters:** Live SQL is a near-guaranteed probe. *"For top-N-per-group I reach for `ROW_NUMBER() OVER (PARTITION BY … ORDER BY …)` and filter on the rank — GROUP BY can't return the row detail."*

---

## 7. pgvector vs. a dedicated vector DB

**Definition:** **pgvector** adds a vector column to Postgres — keeps embeddings **next to your relational data**, one system to run, great for moderate scale and hybrid metadata filters. A **dedicated vector DB** (Pinecone, Weaviate, Milvus) scales to huge indexes with advanced ANN tuning.

**Example:** a **spare room fitted with shelving** (pgvector — reuse the house you have) vs. **renting a purpose-built warehouse** (dedicated DB — for when the volume outgrows the spare room).

**Why it matters:** A common RAG follow-up. *"I start with pgvector to keep vectors beside my data and metadata filters in one query; I move to a dedicated vector DB when scale or ANN tuning demands it."* (→ [[04-embeddings-vector-search]])

---

## Quick misconceptions to avoid
- ❌ "Add indexes everywhere for speed." → Indexes **slow writes** and cost storage; index what you filter/join on.
- ❌ "Guess which query is slow." → **`EXPLAIN ANALYZE`** — read the plan, don't guess.
- ❌ "Always normalize / always denormalize." → It's a **trade-off**; denormalize a hot read path only after measuring.
- ❌ "Composite index column order doesn't matter." → It does — **leftmost prefix** decides what it can serve.
- ❌ "GROUP BY can do top-N-per-group." → Use a **window function**; GROUP BY collapses the row detail.
- ❌ "Store the vector, skip the version." → **Stamp prompt/model/embedding version** or incidents aren't reproducible.

---
_Related: [[04-embeddings-vector-search]] · [[05-rag]] · [[16-monitoring-and-debugging]] · [[22-llm-system-design]] · [[17-aws-core]]_
