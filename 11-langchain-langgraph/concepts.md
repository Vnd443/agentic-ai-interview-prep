# LangChain & LangGraph — Concepts

> Chains, agents, graphs, memory, tool integration

## Diagram (whiteboard version)
LangGraph models an agent as a **state graph** — nodes act, edges (incl. conditional) route:
```mermaid
flowchart TB
    START([START]) --> AGENT[Agent node]
    AGENT --> COND{Tool call?}
    COND -->|yes| TOOLS[Tool node] --> AGENT
    COND -->|no| END([END])
```
> Why LangGraph over vanilla LangChain: **explicit state + cycles + conditional edges** give you controllable, resumable agent loops (checkpointing, human-in-the-loop) instead of a hidden while-loop.

## Overview
_Your study notes go here. Explain it like you would to an interviewer._

## Key ideas
- 

## Deep dives
- 

## Common misconceptions
- 
