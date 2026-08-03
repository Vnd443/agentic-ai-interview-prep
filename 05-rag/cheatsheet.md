# RAG (Retrieval-Augmented Generation) — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Why RAG
- Parametric vs non-parametric knowledge —
- Open-book vs closed-book —
- Fixes: stale / private / hallucination —
- RAG vs fine-tuning (facts vs skill) —
- Order: prompt → RAG → fine-tune —

## Pipeline (6 stages)
- Ingest —
- Chunk —
- Embed + index (metadata) —
- Retrieve (top-k) —
- Re-rank —
- Generate (+ eval loop) —

## Chunking
- Size 200–500 tokens —
- Overlap ~10–20% —
- Structure-aware (headings/paragraphs) —
- Chunk enrichment / contextual retrieval —

## Retrieval
- Top-k similarity —
- Hybrid: vector + BM25 —
- Fusion (RRF) —
- Metadata filtering —
- Query rewriting / HyDE —

## Re-ranking ⭐
- Cross-encoder reranker (Cohere / BGE) —
- Retrieve wide → re-rank narrow —
- The step most candidates skip —

## Generation
- Answer only from context —
- Citations —
- Abstain / "I don't know" —

## Advanced
- Agentic RAG (decide when/what to retrieve, re-query) —
- Graph RAG / Neo4j (multi-hop) —

## Debugging
- Isolate retrieval vs generation —
- Right chunks in top-k? —
- Retrieval fix vs generation fix —

## Eval (RAG Triad)
- Context relevance —
- Groundedness / faithfulness —
- Answer relevance —
- Retrieval metrics: Precision@k / Recall@k / MRR / nDCG —
- Tools: RAGAS / LangSmith —

## Numbers / facts worth quoting
- 
