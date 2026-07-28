# 🎯 Master Roadmap — AI/GenAI/Agentic Interview Prep

**Owner:** Venna Naga Durgaprasad — GenAI & Agentic AI Engineer (IBM)
**Goal:** Be interview-ready for AI Specialist / AI Integration / GenAI Engineer / Agentic AI Engineer / Forward Deployed Engineer roles.
**How this repo works:** one folder per resume topic. Each has a `README.md` (hub), `concepts.md`, `cheatsheet.md`, `interview-qa.md`, `resources.md`, and `projects/`.

> Interview questions are seeded from **AI_Engineer_Interview_Guide.pdf** (10 questions) into the matching topic's `interview-qa.md`. Topics are mapped from **Venna_Naga_DurgaPrasad_Resume.pdf**.

---

## The 20 topics, by tier

### Tier 1 — GenAI / Agentic Core (your headline; master these first)
| # | Topic | Guide Q | Why it's core |
|---|-------|---------|---------------|
| 01 | [LLM Foundations](../01-llm-foundations/README.md) | Q01 | Base of everything |
| 02 | [Prompt Engineering](../02-prompt-engineering/README.md) | — | Cheapest quality lever |
| 03 | [Embeddings & Vector Search](../03-embeddings-vector-search/README.md) | Q07 | Foundation of RAG/search |
| 04 | [RAG](../04-rag/README.md) | Q04 | Most common prod pattern |
| 05 | [Agents & Tool Use](../05-agents-tool-use/README.md) | Q08 | **Headline of your profile** |
| 06 | [MCP & Custom Skills](../06-mcp-and-custom-skills/README.md) | — | **Your differentiator** |
| 07 | [Fine-Tuning](../07-fine-tuning/README.md) | Q05 | Trade-off judgment |
| 08 | [Evaluation](../08-evaluation/README.md) | Q06 | Ship responsibly |
| 09 | [Cost Optimization](../09-cost-optimization/README.md) | Q02 | Build within budget |
| 10 | [Safety & Guardrails](../10-safety-guardrails/README.md) | Q10 | Non-negotiable for prod |

### Tier 2 — Frameworks & Platforms
| # | Topic | Why |
|---|-------|-----|
| 11 | [LangChain & LangGraph](../11-langchain-langgraph/README.md) | Framework layer |
| 12 | [AWS Bedrock & SageMaker](../12-aws-bedrock-sagemaker/README.md) | Cloud-AI surface |
| 13 | [Agentic Coding Tools](../13-agentic-coding-tools/README.md) | You use agents daily |

### Tier 3 — Engineering Backbone
| # | Topic | Why |
|---|-------|-----|
| 14 | [Python](../14-python/README.md) | Everything is built in it |
| 15 | [AWS Core](../15-aws-core/README.md) | Cloud fundamentals |
| 16 | [IaC & DevOps](../16-iac-devops/README.md) | How it ships |
| 17 | [Databases & SQL](../17-databases-sql/README.md) | Data layer |

### Tier 4 — Interview Craft (integrative)
| # | Topic | Guide Q | Why |
|---|-------|---------|-----|
| 18 | [LLM System Design](../18-llm-system-design/README.md) | Q09 | All topics combine |
| 19 | [Production Debugging](../19-production-debugging/README.md) | Q03 | Proves you've shipped |
| 20 | [Behavioral & FDE](../20-behavioral-and-fde/README.md) | — | Half of every loop |

---

## 5-Phase Study Flow

**Phase 1 — Core fluency (Tier 1).** Topics 01→10 in order. For each: read → notes → cheatsheet → rehearse Q&A out loud. You should be able to *teach* each topic.

**Phase 2 — Your edge (05, 06, 13).** Go deep on agents, MCP, custom skills, and agentic coding tools. Prepare a flagship build story — this is what makes you memorable.

**Phase 3 — Frameworks & backbone (11, 12, 14–17).** These support your stories; you need working fluency, not mastery. Prioritise where your resume claims them.

**Phase 4 — Integrative craft (18, 19, 20).** System design out loud (always start with token math), production war-stories, and 3–5 quantified STAR stories.

**Phase 5 — Mock loops.** Full mock interviews mixing topics. Log every answer in `MOCK-INTERVIEW-LOG.md`; convert weak spots back into `concepts.md`/`cheatsheet.md` edits.

> **Interviewers reward built systems + trade-off decisions + clear communication** — not textbook recitation. Every topic ends in a project and a story.

---

## Daily loop (repeatable)
1. Pick the current topic (see `PROGRESS-TRACKER.md`).
2. 25–40 min study → update `concepts.md` + `cheatsheet.md`.
3. Rehearse 2–3 questions from `interview-qa.md` out loud.
4. Tick the topic's **Status** boxes.
5. Once/week: a mixed mock → log it.

## Companion docs
- [`PROGRESS-TRACKER.md`](PROGRESS-TRACKER.md) — where you are across all 20 topics.
- [`RESUME-MAPPING.md`](RESUME-MAPPING.md) — resume line → topic → proof project.
- [`MOCK-INTERVIEW-LOG.md`](MOCK-INTERVIEW-LOG.md) — mock answers + self-scores.

_Tip: run the `/interview-prep` skill to study, quiz yourself, fill topic content, or run a mock._
