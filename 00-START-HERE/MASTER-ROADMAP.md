# 🎯 Master Roadmap — AI/GenAI/Agentic Interview Prep

**Owner:** Venna Naga Durgaprasad — GenAI & Agentic AI Engineer (IBM)
**Goal:** Be interview-ready for AI Specialist / AI Integration / GenAI Engineer / Agentic AI Engineer / Forward Deployed Engineer roles.

**Structure:** **25 topic folders live at the repo root**, numbered `01`–`25` in the order of Balaji's **9-phase roadmap** from [balajichippada.com](https://balajichippada.com/) (interview-only topics come last). Layout is **topic (folder) → subtopics (checklist inside each README)** — no group folders. The phase headings below are the *study sequence*, not folders.

Each topic folder has: `README.md` (hub) · `concepts.md` (learn-first) · `cheatsheet.md` (recall skeleton) · `interview-qa.md` (Q/A) · `resources.md` · `projects/`.

> Interview questions are seeded from **AI_Engineer_Interview_Guide.pdf** (10 Qs) into the matching topic's `interview-qa.md`. Topics are mapped from **Venna_Naga_DurgaPrasad_Resume.pdf**. 🆕 = added vs the original roadmap.

---

## The 25 topics (in phase order)

### 01 · FOUNDATIONS
| # | Topic | Guide Q | Why |
|---|-------|:------:|-----|
| 01 | [Python (Core + Advanced)](../01-python/README.md) | — | Everything is built in it |
| 02 | [LLM Foundations](../02-llm-foundations/README.md) | Q01 | The mental model of an LLM |
| 03 | [Prompt Engineering & API Access](../03-prompt-engineering/README.md) | — | Cheapest quality lever |

### 02 · RETRIEVAL & KNOWLEDGE (RAG + Evaluation)
| # | Topic | Guide Q | Why |
|---|-------|:------:|-----|
| 04 | [Embeddings & Vector Search](../04-embeddings-vector-search/README.md) | Q07 | Foundation of RAG/search |
| 05 | [RAG](../05-rag/README.md) | Q04 | Most common prod pattern |
| 06 | [Evaluation](../06-evaluation/README.md) | Q06 | Ship responsibly |

### 03 · TOOLS, MCP & SINGLE AGENTS
| # | Topic | Guide Q | Why |
|---|-------|:------:|-----|
| 07 | [Agents & Tool Use](../07-agents-tool-use/README.md) | Q08 | **Headline of your profile** |
| 08 | [MCP & Custom Agent Skills](../08-mcp-and-custom-skills/README.md) | — | **Your differentiator** |

### 04 · MEMORY & CONTEXT ENGINEERING
| # | Topic | Why |
|---|-------|-----|
| 09 | [Context Engineering](../09-context-engineering/README.md) | **Differentiator — the agent superset skill** |
| 10 | [Memory Systems](../10-memory-systems/README.md) 🆕 | Remember across turns & sessions |

### 05 · MULTI-AGENT ORCHESTRATION
| # | Topic | Why |
|---|-------|-----|
| 11 | [LangChain & LangGraph](../11-langchain-langgraph/README.md) | Framework layer |
| 12 | [Multi-Agent Orchestration](../12-agent-orchestration/README.md) | **Your Agentic AI home turf** |
| 13 | [A2A — Agent-to-Agent](../13-a2a-agent-to-agent/README.md) 🆕 | Agent interop protocol |

### 06 · GUARDRAILS & LLMOps
| # | Topic | Guide Q | Why |
|---|-------|:------:|-----|
| 14 | [Safety & Guardrails](../14-safety-guardrails/README.md) | Q10 | Non-negotiable for prod |
| 15 | [Cost Optimization](../15-cost-optimization/README.md) | Q02 | Build within budget |
| 16 | [Monitoring & Production Debugging](../16-monitoring-and-debugging/README.md) | Q03 | Proves you've shipped |

### 07 · CLOUD INFRA & DEPLOYMENT
| # | Topic | Why |
|---|-------|-----|
| 17 | [AWS Core](../17-aws-core/README.md) | Cloud fundamentals |
| 18 | [AWS Bedrock & SageMaker](../18-aws-bedrock-sagemaker/README.md) | Cloud-AI surface |
| 19 | [IaC & DevOps](../19-iac-devops/README.md) | How it ships |
| 20 | [Deployment & API Serving](../20-deployment-api-serving/README.md) 🆕 | Notebook → running service |

### 08 · INTERVIEW CRAFT (integrative + interview-only)
| # | Topic | Guide Q | Why |
|---|-------|:------:|-----|
| 21 | [Agentic Coding Tools](../21-agentic-coding-tools/README.md) | — | You use agents daily |
| 22 | [LLM System Design](../22-llm-system-design/README.md) | Q09 | All topics combine |
| 23 | [Databases & SQL](../23-databases-sql/README.md) | — | Data layer |
| 24 | [Fine-Tuning](../24-fine-tuning/README.md) | Q05 | Trade-off judgment |
| 25 | [Behavioral & FDE](../25-behavioral-and-fde/README.md) | — | Half of every loop |

> **Phase sections 01–07 map to Balaji's phases 01–09** (Foundations folds phases 1–3; RAG+Eval and Cloud+Deploy each fold two phases). The **Interview Craft** section holds topics that are interview essentials but sit outside the learning roadmap: agentic coding tools, system design, databases, fine-tuning, behavioral/FDE. These are section headings for study order only — all 25 topic folders sit flat at the repo root.

---

## Study flow (follow the topic order 01 → 25)

**Topics 01–06 — Core fluency.** Python → LLM mental model → prompting → embeddings → RAG → evaluation. For each: read `concepts.md` → fill `cheatsheet.md` → rehearse `interview-qa.md` out loud. You should be able to *teach* each topic.

**Topics 07–13 — Your edge.** Agents, MCP & skills, context engineering, memory, LangGraph, multi-agent orchestration, A2A. Prepare a flagship agent build story — this is what makes you memorable.

**Topics 14–20 — Production & cloud.** Guardrails, cost, monitoring/LLMOps, AWS, IaC, and deployment/API serving. Working fluency; prioritise where your resume claims them.

**Topics 21–25 — Interview craft.** System design out loud (open with token math), production war-stories, 3–5 quantified STAR stories, and the tools you use daily.

**Then — Mock loops.** Full mocks mixing topics. Log every answer in `MOCK-INTERVIEW-LOG.md`; convert weak spots back into `concepts.md` / `cheatsheet.md` edits.

> **Interviewers reward built systems + trade-off decisions + clear communication** — not textbook recitation. Every topic ends in a project and a story.

---

## Daily loop (repeatable)
1. Pick the current topic (see `PROGRESS-TRACKER.md`).
2. 25–40 min study → update `concepts.md` + `cheatsheet.md`.
3. Rehearse 2–3 questions from `interview-qa.md` out loud.
4. Tick the topic's **Status** boxes.
5. Once/week: a mixed mock → log it.

## Companion docs
- [`PROGRESS-TRACKER.md`](PROGRESS-TRACKER.md) — status across all 25 topics.
- [`RESUME-MAPPING.md`](RESUME-MAPPING.md) — resume line → topic → proof project.
- [`MOCK-INTERVIEW-LOG.md`](MOCK-INTERVIEW-LOG.md) — mock answers + self-scores.

_Tip: run the `/interview-prep` skill to study, quiz yourself, fill topic content, or run a mock._
