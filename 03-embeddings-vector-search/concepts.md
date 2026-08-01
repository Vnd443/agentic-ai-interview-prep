# Embeddings & Vector Search — Concepts

> Dense vectors, similarity, ANN/HNSW, vector databases

## Diagram (whiteboard version)
```mermaid
flowchart LR
    T[Doc text] --> M[Embedding model] --> Vec[Dense vector] --> DB[(Vector index<br/>HNSW / IVF)]
    Q[Query text] --> M2[Same embedding model] --> QV[Query vector] --> DB
    DB -->|cosine / dot-product · top-k| N[Nearest neighbors]
```
> Key point: **query and documents must use the same embedding model**. ANN (HNSW/IVF) trades a little recall for huge speed vs exact search.

## Overview
_Your study notes go here. Explain it like you would to an interviewer._

## Key ideas
- 

## Deep dives
- 

## Common misconceptions
- 
