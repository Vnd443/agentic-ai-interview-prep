# MCP & Custom Agent Skills — Concepts

> Model Context Protocol servers + custom agent skills (your edge)

## Diagram (whiteboard version)
```mermaid
flowchart LR
    subgraph Host [Host app · Claude / IDE]
        LLM[LLM] --- C1[MCP client]
        LLM --- C2[MCP client]
    end
    C1 -->|stdio / HTTP| S1[MCP server A]
    C2 -->|stdio / HTTP| S2[MCP server B]
    S1 --> R1[Tools · Resources · Prompts]
    S2 --> R2[DB / API / files]
```
> One-liner: **MCP is USB-C for LLM tools** — a standard so any client can talk to any server. Servers expose three primitives: **tools** (actions), **resources** (data), **prompts** (templates). MCP = agent↔tools; A2A = agent↔agent.

## Overview
_Your study notes go here. Explain it like you would to an interviewer._

## Key ideas
- 

## Deep dives
- 

## Common misconceptions
- 
