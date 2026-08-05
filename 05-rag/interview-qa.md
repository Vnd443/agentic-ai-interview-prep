# RAG (Retrieval-Augmented Generation) — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Source flagged: **AI_Engineer_Interview_Guide.pdf → Q04 (RAG Architecture)**.

---

**Q1. Walk me through how you'd design a RAG pipeline. ⭐ (guide Q04)**

Six stages. First, ingestion: parse the docs (PDF/HTML/DB), clean them, and split into semantic chunks — usually 200–500 tokens with overlap, split structure-aware on headings. Second, embed each chunk and store it in a vector DB with metadata for filtering. Third, at query time embed the query and do a similarity search for the top-k, ideally hybrid (vector + BM25) so exact terms and IDs also match. Fourth — the step most people skip — re-rank the top-k with a cross-encoder to get true relevance, retrieving wide then narrowing down. Fifth, generation: feed the reranked context plus the query to the LLM with a system prompt that says answer only from context, cite sources, and abstain if it's not there. Sixth, an eval loop measuring retrieval recall, faithfulness, and answer relevance. The power move is calling out re-ranking as its own step — it's what separates a toy RAG from a production one.

---

**Q2. Why does RAG exist — why not just fine-tune the model on your data?**

An LLM only knows its training data (parametric knowledge), which is stale, doesn't include private data, and can hallucinate. RAG gives it non-parametric knowledge by retrieving real documents at query time — it turns a closed-book exam into an open-book one. Fine-tuning changes style, format, or a narrow skill but doesn't reliably add facts, and you'd never run a training course just to tell someone today's price — you hand them the sheet. So the rule is: facts and freshness go to RAG, consistent behavior/format goes to fine-tuning, and I try prompting first, RAG second, fine-tune last.

---

**Q3. RAG isn't returning relevant chunks. How do you debug it?**

I isolate the stage. First I check retrieval on its own: are the right chunks even in the top-k? If they're not, it's a retrieval problem — fix chunking, try a different embedding model, raise k then re-rank down, add hybrid search, or add metadata filters. If the right chunks are there but the answer is still wrong, it's a generation/prompt problem. A wrong open-book answer means either I opened the wrong page or misread the right one, and those are fixed very differently — so I always measure retrieval and generation separately. Most "RAG is bad" complaints are actually retrieval failures.

---

**Q4. What is re-ranking and why does it matter so much?**

Vector retrieval is fast but imprecise — it grabs roughly-relevant chunks. A cross-encoder re-ranker reads each query-and-chunk pair together and re-scores true relevance, so I retrieve a wide top-k (say 50) then re-rank down to the best 5. It's like skimming titles to pull 50 books, then actually opening each to keep the 5 that really answer the question. It's slower per item, so I only run it on the shortlist — but it's the single biggest precision win in a RAG system and the detail most candidates miss.

---

**Q5. How do you evaluate a RAG system?**

I use the RAG Triad and measure the legs separately, because a system can be faithful but irrelevant or relevant but ungrounded. Context relevance asks whether retrieval fetched the right chunks; groundedness/faithfulness asks whether the answer is actually supported by those chunks (no hallucination); answer relevance asks whether it addresses the question. For retrieval I also track Precision@k, Recall@k, MRR, and nDCG. Tools like RAGAS and LangSmith let me score these in a CI-style regression suite so quality doesn't drift.

---

**Q6. When is RAG the wrong choice?**

When the task needs reasoning or style rather than facts (prompt or fine-tune instead), when the knowledge already fits in the context window (just include it directly), or when the latency budget can't afford the retrieval hop. RAG adds moving parts — a vector store, retrieval, re-ranking — so I use it specifically when the requirement is grounding answers in a large, changing, or private corpus, not as a default for everything.

---

**Q7. Is RAG dead now that models have million-token context windows? And what's the difference between RAG, Agentic RAG, and Agentic AI? ⭐**

No — what's dying is *naive* one-shot vector RAG, not retrieval itself. "Just paste the whole corpus into a 1M-token window" fails on three things I always name: cost (you pay for every token on every call — a 300-page doc is ~200k tokens per question), accuracy ("lost in the middle" / context rot — models miss facts buried in a huge prompt), and reality (enterprise corpora are gigabytes with access control and daily updates, which never fit any window). So retrieval never goes away; it just gets smarter. The progression is naive RAG → agentic retrieval. On the naming: **Agentic AI** is the umbrella — a model in a plan → act → observe → reflect loop that can call tools, keep state, and pursue a goal. **Agentic RAG** is the retrieval piece *inside* that loop: instead of retrieving once, the agent decides whether to retrieve, writes its own query, grades what came back, and re-queries if it's weak. My one-liner is *"Agentic RAG is the retrieval module inside an Agentic AI workflow — retrieval is one tool the agent chooses to use, alongside code, APIs, or SQL."* Concrete example: "How did our revenue growth compare year-over-year over three years?" — naive RAG does one search and stuffs whatever it gets; agentic RAG runs three targeted retrievals (one per year), notices a gap, re-queries, then synthesizes. The honest trade-off, and the power move to close on: agentic adds latency, tokens, and non-determinism, so I gate it with a **max-iteration cap and a budget guard**, and in production I run a tiered system — cheap naive RAG for simple lookups, escalate to agentic only for multi-step questions.

---

## Your notes / STAR angle
- _TODO: your production RAG system — corpus size, stack, chunking choice, and eval numbers (recall / faithfulness before → after)._
