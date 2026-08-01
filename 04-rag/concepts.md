# RAG (Retrieval-Augmented Generation) — Concepts

> Ingest -> embed -> retrieve -> rerank -> generate -> eval

## Diagram (whiteboard version)
```mermaid
flowchart LR
    subgraph Ingest [Offline · ingestion]
        D[Docs] --> C[Chunk] --> E1[Embed] --> V[(Vector DB)]
    end
    subgraph Query [Online · query time]
        Q[User query] --> E2[Embed] --> R[Retrieve top-k]
        V --> R
        R --> RR[Re-rank] --> A[Assemble context] --> G[LLM generate] --> O[Answer + citations]
    end
    O -.->|faithfulness · RAGAS| EV[Eval]
```
> Power move: call out **re-ranking as its own step** — it's what separates a toy RAG from a production one.

## Overview
_Your study notes go here. Explain it like you would to an interviewer._

## Key ideas
- 

## Deep dives
- 

## Common misconceptions
- 
