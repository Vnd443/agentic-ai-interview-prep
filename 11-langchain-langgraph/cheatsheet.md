# LangChain & LangGraph — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## LangChain
- Glue: prompts / models / retrievers / parsers / memory / tools —
- Chains = predefined step pipeline (deterministic) —
- Fast prototyping + integrations —

## LangGraph
- Agent as a state graph —
- Nodes = steps (LLM / tool / decision) —
- Edges + conditional edges = routing —
- Shared state threaded through —
- Cycles = ReAct-style loops w/ stop conditions —

## State
- Checkpointing = save / resume / replay / rewind —
- Human-in-the-loop nodes (pause → approve → resume) —

## Memory
- Short-term: conversation buffer (trim / summarize) —
- Long-term: vector DB or keyed DB per user/session —
- Be explicit about what's carried forward —

## Observability
- LangSmith: trace I/O, latency, cost —
- Run eval datasets / regression —

## Framework vs SDK (the call)
- Plain SDK: single-shot, least magic —
- LangChain: compose + integrate fast —
- LangGraph: stateful, cyclic, multi-step —
- Cost: abstraction opacity + version churn —
- Least abstraction that works —

## Numbers / facts worth quoting
- 
