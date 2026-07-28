# Embeddings & Vector Search — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q07 (Embeddings & Vector Search)**.

---

## Q1. Explain embeddings and how you'd use them in production. ⭐ (guide Q07)

**Why they ask:** Embeddings underpin search, recommendations, and RAG. Deep understanding signals strong fundamentals.

**Ideal answer:**
- **What they are** — dense vector representations of text (or images/audio) where semantically similar items are close in vector space. They capture *meaning*, not keywords.
- **How they work** — a neural net maps input to a fixed-size vector (e.g. 1536-dim). **Cosine similarity / dot product** between vectors measures semantic closeness ("King" is nearer "Queen" than "Bicycle").
- **Production use cases** — semantic search, RAG retrieval, duplicate detection, recommendations, clustering/topic modelling, text anomaly detection.
- **Choosing a model** — weigh dimensionality (higher = more expressive but slower/costlier), domain fit, multilingual support, and cost. **Benchmark on YOUR data**, not just MTEB leaderboards.
- **Scaling** — beyond a few hundred thousand vectors, exact search is too slow; use **ANN** indexes (**HNSW**, IVF). Pick the index for your recall/latency trade-off.

**🔑 Power move:** In a whiteboard, *draw it*: `query → embed → ANN search → top-k → re-rank → generate`. Visual communication is a differentiator.

**Follow-ups:**
- Cosine vs. dot product vs. Euclidean — when each?
- HNSW vs. IVF trade-offs? → HNSW: great recall/latency, more memory; IVF: cheaper memory, tune nprobe.
- Why benchmark on your own data over MTEB?

---

## Q2. How does approximate nearest neighbour (ANN) search work, and what do you trade off?

**Ideal answer:** ANN trades a little **recall** for a huge **speed/memory** win vs. brute-force exact search. **HNSW** builds a navigable small-world graph you greedily traverse; **IVF** clusters vectors and only searches the nearest clusters (`nprobe` controls how many). You tune parameters to hit a recall target within a latency budget.

**Follow-ups:** What happens to recall as you lower `efSearch`/`nprobe`? How do you re-index on updates?

---

## Q3. How do you keep an embedding index fresh and consistent?

**Ideal answer:** Store chunk + metadata + a content hash; upsert on change, delete on removal. Watch for **embedding-model version drift** — if you change the embedding model you must re-embed the whole corpus (query and doc vectors must come from the same model). Batch re-embeds; keep the model version in metadata.

---

## Your notes / STAR angle
- _TODO: which embedding model + vector DB you used and why._
