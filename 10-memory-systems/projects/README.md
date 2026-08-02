# Memory Systems — Project Ideas

> Build one small thing you can demo and talk through in an interview. Map it to a resume claim.

## Idea 1 — "Assistant that remembers you across sessions" (recommended)
A chatbot that, on each new session, loads a user profile and past-interaction summary from a store, then updates it when the session ends.
- **Stack:** LangGraph store (or DynamoDB) for user memory + a vector store for past-interaction recall.
- **Demo the difference:** run session 1 ("I prefer metric, I'm working on project X"), close it, start session 2 → it greets you with that context.
- **Talking point:** write filter (what you chose to save), retrieval blend (similarity + recency), forget policy (TTL).

## Idea 2 — Summary-buffer conversation memory
Implement the summary-buffer pattern by hand: keep the last N turns verbatim + a running summary of older turns; show the token count staying flat as the chat grows.
- **Talking point:** this is the Compress lever from [[09-context-engineering]]; show before/after token counts.

## Idea 3 — Memory-decay / contradiction resolution
Add a forget policy to Idea 1: TTL on time-sensitive facts + update-on-contradiction (new "I moved to Bangalore" overrides old city).
- **Talking point:** memory poisoning and how you prevent stale facts.

## What to capture for the interview
- A number: tokens saved vs full-buffer, or retrieval latency.
- The trade-off you made (which store, why).
- One failure you hit (e.g. stale memory) and how you fixed it.

_Related: [[09-context-engineering]] · [[05-rag]] · [[11-langchain-langgraph]]_
