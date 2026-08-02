# Memory Systems — Interview Q&A

Copy-paste template for any memory answer:

> **What kind of memory** (short vs long-term, which type) → **where it's stored** (vector / key-value / relational) → **how it's written & retrieved** → **how it's kept fresh** (forget/decay) → a number or trade-off.

---

**Q. What's the difference between memory and just using a bigger context window?**

The context window is short-term memory — it only holds the current conversation and it disappears when the session ends or gets trimmed. A bigger window is more expensive, slower, and the model still degrades when it's stuffed (lost-in-the-middle). Memory is an *external* store I retrieve from, so information survives across sessions and I only pull the few relevant pieces into the window at a time. The key line: memory is a retrieval problem, not a storage problem — I keep everything outside and fetch what's relevant per turn.

---

**Q. Your chatbot forgets the user between sessions. How do you fix it?**

I add long-term, user-keyed memory. At the end of a session I write the durable facts (preferences, decisions, unresolved issues) to a store keyed by user ID — DynamoDB for exact profile lookups, a vector store if I need semantic recall of past interactions. At the start of each new session I load that user's memory and inject a compact summary into the system prompt. So the window is fresh each time, but the *user memory* persists. I don't just enlarge the window — that wouldn't survive a new session anyway.

---

**Q. How do you decide what to write to long-term memory versus letting it go?**

I use a write filter — not every message deserves saving. I save durable, reusable facts (semantic: name, preferences), meaningful events (episodic: "cancelled order X on this date"), and learned procedures. I skip small talk and anything transient. On top of that I have a forget policy: TTL on time-sensitive items, relevance decay for old unused memories, and update-on-contradiction so a new fact overrides the stale one. Without a write filter and a forget policy, memory grows forever and starts contradicting itself.

---

**Q. With thousands of stored memories, how do you retrieve the right one?**

I treat it like RAG over memories. I rank candidates by a blend of semantic similarity to the current query, recency, and importance — not pure similarity, because the newest or flagged-important memory often matters more than the closest match. I retrieve the top few and add only those to the context. For exact needs (this user's profile) I do a direct key lookup instead of a search. Matching the retrieval method to the access pattern is the point.

---

**Q. What are episodic, semantic, and procedural memory?**

Episodic is specific events — "on this date the user did X." Semantic is general facts — "the user's name is Prasad, prefers metric." Procedural is how-to knowledge — the steps or skills the agent has learned to perform a task. Most production memory is semantic (user facts) plus episodic (past interactions); procedural shows up as reusable tool sequences or skills. Naming the three shows I've thought past "just save the transcript."

---

**Q. How would you implement memory in LangGraph?**

Two layers. A checkpointer persists the graph's state between steps and lets a thread resume — that's short-term/thread memory. The store holds long-term memories across threads, namespaced by user, which I read at the start and write to at the end of a run. So short-term thread continuity comes from the checkpointer; durable cross-session memory comes from the store. That split maps cleanly onto short-term vs long-term memory.
