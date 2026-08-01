# LLM Foundations — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Big picture
- AI → ML → GenAI → LLM —
- What is an LLM / foundation model —
- Where NLP fits —
- Generative AI vs Traditional ML —

## Basics
- Tokens —
- Embeddings —

## How it works inside
- Autoregressive generation —
- Parameters & scale (7B/70B) —
- Transformer & self-attention (Q/K/V) —
- Multi-head attention —
- Positional encoding —
- Architecture flavors (decoder / encoder / encoder-decoder) —
- Training: Pretraining → SFT (Supervised Fine-Tuning) → RLHF (Reinforcement Learning from Human Feedback) / DPO (Direct Preference Optimization) —
- GPT = Generative Pre-trained Transformer —
- Model families (GPT / Claude / Llama / Gemini; open vs closed) —
- Types by size/specialty (LLM / SLM / MoE / reasoning / multimodal) —
- In-context learning (zero / one / few-shot) —

## Using it
- Context window —
- Lost in the middle —
- Temperature —
- top_p / top_k —
- Greedy decoding —
- Hallucinations —
- How to reduce hallucinations —
- Log-probs —
- Self-consistency —
- Perplexity —

## Engineering disciplines
- Prompt engineering —
- Context engineering —
- Loop engineering —
- Orchestration engineering —
- Agent loop (looping) —

## Memory
- Human → AI map (short-term→window, long-term→store, experience→episodic, facts→semantic, skills→procedural) —
- By lifetime: short-term / working vs long-term —
- By type: episodic / semantic / procedural —
- Other terms: scratchpad, contextual, entity, tool, vector —
- Storage techniques: buffer / sliding window / summarization / vector store / KV / knowledge graph —
- Real tools: Redis (chat), Postgres/MongoDB (long), Pinecone/Weaviate (vector), LangGraph (state), JSON logs (tools) —
- Problems: context overflow / memory drift / stale memory / retrieval noise / memory hallucination / cost+privacy —
