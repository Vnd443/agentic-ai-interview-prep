# LLM System Design — Concepts

> Scaling LLM systems, back-of-envelope cost math

## Diagram (reference architecture — draw this first)
```mermaid
flowchart TB
    U[User] --> GW[API gateway · auth · rate limit]
    GW --> ORCH[Orchestration layer]
    ORCH --> CACHE{Semantic cache hit?}
    CACHE -->|yes| RESP[Response]
    CACHE -->|no| RAG[Retrieval · vector DB]
    RAG --> ROUTER[Model router · cheap vs frontier]
    ROUTER --> LLM[LLM inference]
    LLM --> GUARD[Output guardrails · validation]
    GUARD --> RESP
    LLM -.-> OBS[Logging · tracing · eval]
```
> Open the answer with **back-of-envelope token math**, then sketch this and talk trade-offs (cache → route → retrieve → guard → observe).

## Overview
_Your study notes go here. Explain it like you would to an interviewer._

## Key ideas
- 

## Deep dives
- 

## Common misconceptions
- 
