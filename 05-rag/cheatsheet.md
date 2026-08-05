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

## Chunking (techniques menu)
- Size 200–500 tokens · overlap ~10–20% —
- Fixed-size (token/char) —
- Recursive character (LangChain default) —
- Document / layout-aware (headings, tables, code) —
- Sentence / sliding-window —
- Semantic (cut on topic shift) —
- Parent-document / small-to-big (the "large chunk" answer) —
- Agentic / propositional —
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

## Types of RAG (the ladder — JD names ★)
- Standard / Naive (retrieve once → stuff → generate) —
- Context-Augmented ★ (enrich chunk before embedding) —
- Corrective / CRAG ★ (grade chunks → fallback if weak) —
- Self-RAG (decide whether to retrieve + self-critique) —
- Agentic ★ (agent loops: when/what/how-many, cap iterations) —
- Graph ★ / Neo4j (traverse relations, multi-hop) —
- Pick simplest that hits the accuracy bar —
- CRAG grades trust ≠ re-ranking orders chunks —

## Tools (name with a reason)
- Frameworks: LangChain/LangGraph · LlamaIndex · Semantic Kernel · Haystack —
- Vector DBs: Pinecone · Qdrant · Weaviate · Milvus · Chroma · pgvector · FAISS · Bedrock KB —
- Parsers: Unstructured · LlamaParse (messy PDFs/tables) —
- Re-rankers: Cohere Rerank · BGE · Jina · Voyage —
- Eval: RAGAS · TruLens · LangSmith · DeepEval · Phoenix —

## Corpus size & file limits (do you even need RAG?)
- ≲ 200 pages → skip RAG, use long context —
- ~200–10,000+ pages → RAG / Agentic RAG, chunk 200–500 words —
- Millions / enterprise → RAG + metadata + access control —
- Managed platforms: hard per-file/token caps, auto-chunk (lose control) —
- Raw vector DBs: no file limit, you own parse→chunk→embed —

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
