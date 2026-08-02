# Context Engineering — Resources

> Verify links live; the space moves fast. Read 2–3 primary sources, then write `concepts.md` in your own words.

## Primary (read these first)
- **Anthropic — "Effective context engineering for AI agents"** (Anthropic engineering blog). The canonical framing: context as a finite budget, compaction, sub-agent isolation. https://www.anthropic.com/engineering
- **LangChain — "Context Engineering"** blog + docs. Source of the **Write / Select / Compress / Isolate** framework. https://blog.langchain.com/
- **"Lost in the Middle: How Language Models Use Long Contexts"** (Liu et al., 2023). The paper behind the failure mode. https://arxiv.org/abs/2307.03172

## Failure modes & long-context behavior
- **"Context rot"** writeups (Chroma / community research) on degradation as context grows.
- Prompt-caching docs (Anthropic / OpenAI) — how to structure a stable prefix.

## Memory
- **MemGPT / Letta** — LLMs as an OS managing memory tiers (paper + framework). https://arxiv.org/abs/2310.08560
- LangGraph / LangMem memory docs — short-term vs long-term memory patterns.

## Tie-ins from this repo
- RAG as the *Select* lever → [[05-rag]]
- Sub-agent isolation → [[12-agent-orchestration]]
- Prompt caching economics → [[15-cost-optimization]]
- Token/window fundamentals → [[02-llm-foundations]]

## Watch/skim
- Conference talks on "context engineering for agents" (2025–2026) — search recent talks.

_TODO: after reading, paste your 3 best links + one takeaway each here._
