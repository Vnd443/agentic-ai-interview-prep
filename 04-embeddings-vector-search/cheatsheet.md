# Embeddings & Vector Search — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## What & where
- Embedding (meaning → vector) —
- Vector space & dimensions (e.g. 1536-D) —
- Matryoshka / truncatable dims —

## Similarity
- Cosine (angle / direction) —
- Dot product (angle + magnitude) —
- Euclidean / L2 (straight-line distance) —
- Normalized → cosine ≈ dot —

## Choosing a model
- MTEB leaderboard (starting point) —
- Benchmark on YOUR data —
- Same model for query + docs —
- Max input length / language / cost —

## Search at scale
- Exact search (O(N), fine < ~100k) —
- ANN = trade recall for speed —
- Recall vs latency trade-off —

## Indexes
- HNSW (graph; best recall/latency; high memory; `efSearch`) —
- IVF (clusters/buckets; cheaper memory; `nprobe`) —
- IVF-PQ (compression) —

## Vector DBs
- Pinecone (managed) —
- Weaviate / Qdrant / Milvus —
- pgvector (Postgres extension) —
- Choose by ops / scale / existing stack —

## Freshness & ops
- Upsert on change, delete on removal —
- Content hash to skip unchanged —
- Model change → re-embed whole corpus —
- Store model version in metadata —
- Metadata filtering (+ security / tenant isolation) —

## Numbers / facts worth quoting
- 
