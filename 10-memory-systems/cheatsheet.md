# Memory Systems — Cheatsheet

> Fill each `—` with your own one-line explanation from memory. Don't pre-read the answer; test recall.

## Big picture
- Memory vs context window —
- Memory is a retrieval problem, not a storage problem —

## Types
- Short-term (in-window) —
- Long-term (external store) —
- Episodic —
- Semantic —
- Procedural —
- Session memory —
- User / cross-session memory —

## Conversation patterns
- Buffer —
- Buffer-window —
- Summary —
- Summary-buffer (default) —

## The loop
- Write (filter: what's worth saving) —
- Read / retrieve (relevant, not all) —
- Forget (TTL / decay / contradiction) —

## Storage choices
- Vector DB (semantic recall) —
- Key-value / DynamoDB (exact lookup) —
- RDS / PostgreSQL (structured) —
- Scratchpad / state object —

## Retrieval ranking
- Semantic similarity —
- Recency —
- Importance —

## Frameworks
- LangGraph checkpointer (short-term thread state) —
- LangGraph store (long-term cross-thread) —
- LangChain buffer/summary memory —

## Failure modes
- Stale memory —
- Contradictory memories —
- Memory poisoning —
- Unbounded growth —
