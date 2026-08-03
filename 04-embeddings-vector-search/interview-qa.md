# Embeddings & Vector Search — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q07 (Embeddings & Vector Search)**.

---

**Q1. Explain embeddings and how you'd use them in production. ⭐ (guide Q07)**

An embedding is a dense vector that captures the *meaning* of text (or images/audio), so semantically similar items sit close together in vector space — it captures meaning, not keywords. A neural net maps each input to a fixed-size vector (e.g. 1536-dim), and cosine similarity or dot product between vectors measures closeness. In production I use them for semantic search, RAG retrieval, deduplication, recommendations, and clustering. Choosing a model, I weigh dimensionality, domain fit, language, and cost, and I benchmark on my own data rather than trusting the MTEB leaderboard alone. Past a few hundred thousand vectors, exact search is too slow so I switch to an ANN index (HNSW or IVF). The power move is to *draw it* on the whiteboard: query → embed → ANN search → top-k → re-rank → generate.

---

**Q2. Cosine vs dot product vs Euclidean — when do you use each?**

Cosine measures the angle between vectors, so it compares meaning-direction and ignores length — my default for text, because a short tweet and a long essay on the same topic should still count as similar. Dot product factors in magnitude too, so longer vectors score higher; it's equivalent to cosine when vectors are normalized. Euclidean is straight-line distance and can be misled by length differences. Rule of thumb: normalize and use cosine/dot for text unless the model was trained for a specific metric.

---

**Q3. How does approximate nearest neighbour (ANN) search work, and what's the trade-off?**

ANN trades a little recall for a huge speed and memory win over brute-force exact search. HNSW builds a multi-layer "small-world" graph you traverse greedily — like taking express subway lines to get near, then local stops; great recall and latency but memory-hungry, tuned with `efSearch`. IVF clusters vectors into buckets and only searches the nearest ones — like only walking the closest city districts; cheaper memory, tuned with `nprobe`. Lowering `efSearch`/`nprobe` speeds things up but drops recall, so I tune to hit a recall target within a latency budget.

---

**Q4. How do you keep an embedding index fresh and consistent?**

I store each chunk with metadata and a content hash, upsert on change and delete on removal, and skip unchanged chunks by hash. The big rule is embedding-model version drift: if I change the embedding model I must re-embed the entire corpus, because query and document vectors have to come from the same model — mixing versions is like comparing addresses from two different maps. So I keep the model version in metadata and batch-reprocess on any upgrade.

---

**Q5. Which vector database would you choose, and why?**

It depends on scale and existing stack. For a smaller app I'd use pgvector so I don't run a second database — it bolts a vector column onto the Postgres I already have. At large scale, or when I need heavy filtering and ops features, I'd use a purpose-built store like Pinecone (managed), Qdrant, or Weaviate. Either way I lean on metadata filtering to restrict by tenant/date before ranking — it fixes most relevance problems and enforces security so one user can't retrieve another's documents.

---

## Your notes / STAR angle
- _TODO: which embedding model + vector DB you used and why (corpus size, dims, recall/latency numbers)._
