# RAG (Retrieval-Augmented Generation) — Interview Q&A

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q04 (RAG Architecture)**.

---

## Q1. Walk me through how you'd design a RAG pipeline. ⭐ (guide Q04)

**Why they ask:** RAG is the most common production LLM pattern. Can't design it end-to-end → struggle in any applied role.

**Ideal answer — the full pipeline:**
1. **Document ingestion** — parse (PDF/HTML/DB), clean, and split into semantic **chunks**. Chunk size matters: too small loses context, too large dilutes relevance. Typically **200–500 tokens with overlap**.
2. **Embedding & indexing** — embed chunks (e.g. `text-embedding-3-small` or open-source), store in a vector DB (Pinecone, Weaviate, pgvector, Qdrant) **with metadata** for filtering.
3. **Retrieval** — embed the query, similarity search (cosine/dot product), retrieve **top-k**. Consider **hybrid search** (vector + BM25 keyword) for better recall.
4. **Re-ranking** — raw retrieval isn't enough. A **cross-encoder reranker** (Cohere Rerank, BGE) re-scores the top-k for true relevance. Big precision win.
5. **Generation** — feed reranked context + query to the LLM with a system prompt that says *answer only from context and cite sources*.
6. **Evaluation loop** — measure retrieval recall, answer **faithfulness** (matches source?), and answer relevance. Use RAGAS or a custom eval suite.

**🔑 Power move:** "Call out **re-ranking as its own step** — most candidates skip it, and it's what separates a toy RAG from a production one."

**Follow-ups:**
- Chunking strategy for tables / code / long docs? → structure-aware or hierarchical chunking.
- Why hybrid search over pure vector? → keyword recall for names, IDs, rare terms.
- How do you cite sources / attribute answers?
- What if retrieval returns nothing relevant? → abstain / "I don't know" rather than hallucinate.

---

## Q2. RAG isn't returning relevant chunks. How do you debug it?

**Ideal answer:** Isolate the stage. Check **retrieval in isolation** (are the right chunks even in top-k? → chunking or embedding-model problem). If they are but the answer is bad → **generation/prompt** problem. Levers: better chunking, hybrid search, add a reranker, raise k then rerank down, query rewriting/expansion, metadata filtering.

**🔑 Power move:** "I always measure retrieval and generation *separately* — most 'RAG is bad' issues are actually retrieval issues."

**Follow-ups:** Retrieval recall vs. answer faithfulness — which failed? Query rewriting / HyDE?

---

## Q3. When is RAG the wrong choice?

**Ideal answer:** When the task needs *reasoning/style* not *facts* (fine-tune or prompt instead), when the knowledge fits in the context window (just include it), or when latency budget can't afford the retrieval hop. RAG adds moving parts — use it when grounding in a changing corpus is the actual requirement.

---

## Your notes / STAR angle
- _TODO: your production RAG system — corpus size, stack, eval numbers._
