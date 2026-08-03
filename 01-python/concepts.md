# Python — Concepts

> Read this top-to-bottom the first time. Each idea = **plain definition → everyday example → why it matters**.
> Cheatsheet (`cheatsheet.md`) is for revising *after* you understand these.
> Scope = the Python that comes up for **AI/backend** roles: async, FastAPI, AWS/Boto3, data, DB, testing.

---

## 1. Sync vs Async (one waiter, many tables)

**Definition:** **Synchronous** code does one thing at a time — it *blocks* (waits) until each step finishes. **Asynchronous** code can start a slow job, walk away to do other work, and come back when the result is ready — all on **one thread**.

**Example — a restaurant waiter:**
- *Sync waiter:* takes table 1's order, walks to the kitchen, and **stands there** until the food is cooked before serving anyone else. Tables 2 and 3 wait.
- *Async waiter:* takes table 1's order, hands it to the kitchen, and **while it cooks** goes to take orders from tables 2 and 3. When a dish is ready, they serve it. One waiter keeps many tables moving.

**Why it matters:** An LLM app spends most of its time *waiting* on API calls (the "kitchen"). Async lets one worker keep hundreds of those calls in flight instead of blocking on each one. Interview line: *"LLM apps are I/O-bound on API calls — async handles many in-flight requests per worker instead of blocking per call."*

---

## 2. The event loop & `asyncio` (the thing running the waiter)

**Definition:** The **event loop** is the scheduler behind async — it keeps a list of coroutines, runs one until it hits an `await` (a "wait here"), then switches to another that's ready. `async def` defines a coroutine; `await` is the point where it can hand control back.

**Example:**
```python
import asyncio

async def fetch(name):
    await asyncio.sleep(1)      # pretend this is an LLM/API call
    return f"{name} done"

async def main():
    # sequential: ~3s total (await one, then the next)
    a = await fetch("A"); b = await fetch("B"); c = await fetch("C")

    # concurrent: ~1s total (all three in flight at once)
    a, b, c = await asyncio.gather(fetch("A"), fetch("B"), fetch("C"))

asyncio.run(main())
```

**Why it matters:** `asyncio.gather` is the interview keyword — it's how you fan out many model/tool calls **concurrently**. Awaiting them one by one throws away all the benefit. Trap: a **blocking** call (heavy CPU, or a sync library) inside async **freezes the whole loop** — push it to `asyncio.to_thread(...)` / an executor.

---

## 3. The GIL (the talking stick)

**Definition:** The **GIL (Global Interpreter Lock)** means only **one thread runs Python bytecode at a time**, even on a multi-core CPU. Threads take turns; they don't truly run Python in parallel.

**Example — a talking stick:** in a room of ten people, only the person **holding the stick** may speak. Adding more people doesn't get more *talking* done — they just wait for the stick. But if speaking means "go fetch a book from the library" (waiting on I/O), the speaker can **pass the stick while they wait** — so others get things done during the wait.

**Why it matters:** This is *the* Python concurrency question. The rule:
- **CPU-bound work** (parsing, math, image resize) → GIL blocks you → use **multiprocessing** (separate processes, separate GILs).
- **I/O-bound work** (API calls, DB, disk) → threads/async help, because waiting releases the stick.

One-liner: *"Threads don't speed up CPU work in Python because of the GIL — use processes for CPU, async/threads for I/O."*

---

## 4. Concurrency choices — async vs threads vs processes

**Definition:** Three tools, matched to the bottleneck:

| Tool | Best for | Why |
|---|---|---|
| **asyncio** | Many I/O waits (LLM/API/DB calls) | One thread juggles thousands of awaits, cheaply |
| **Threads** | I/O with blocking/sync libraries | GIL is released during I/O; simpler than rewriting to async |
| **Processes** (multiprocessing) | CPU-heavy work | Each process has its own GIL → real parallelism |

**Example:** Calling 500 LLM endpoints → **async**. Resizing 500 images → **multiprocessing**. Using an old sync SDK that can't `await` → run it in a **thread pool**.

**Why it matters:** Interviewers probe whether you pick the tool by the *bottleneck*, not by habit. Naming the trade-off is the senior signal.

---

## 5. Type hints & Pydantic (the form checker at the door)

**Definition:** **Type hints** (`name: str`, `-> int`) document the shapes of data; they don't enforce anything at runtime by themselves. **Pydantic** turns those hints into a **runtime validator** — it checks and coerces incoming data against a model, and raises a clear error if it doesn't fit.

**Example — a strict form at reception:** you hand in a form where *age* says `"twenty"`. Pydantic is the clerk who rejects it — *"age must be a number"* — and, if you wrote `"25"`, quietly converts it to the integer `25`.
```python
from pydantic import BaseModel

class ChatRequest(BaseModel):
    prompt: str
    max_tokens: int = 512      # default
    temperature: float = 0.0

ChatRequest(prompt="hi", max_tokens="800")   # "800" → 800 automatically
ChatRequest(prompt="hi", temperature="hot")  # ← raises ValidationError
```

**Why it matters:** It's the backbone of a safe API and of **structured LLM output** — you make the model return JSON, then parse it straight into a Pydantic model so bad shapes fail loudly instead of leaking downstream. Ties into [[07-agents-tool-use]] (tool schemas) and [[03-prompt-engineering]] (structured output).

---

## 6. OOP & dataclasses (blueprints vs quick containers)

**Definition:** A **class** is a blueprint bundling data + behavior. A **`@dataclass`** is a shortcut for a class that mostly just *holds data* — Python auto-writes the `__init__`, `__repr__`, and equality for you.

**Example:**
```python
from dataclasses import dataclass

@dataclass
class Message:
    role: str
    content: str
# Message("user", "hi") just works — no boilerplate __init__ to write.
```
Use a full class when there's real behavior (methods, inheritance); use a dataclass (or Pydantic model) when it's mostly a typed container. Rule of thumb: **dataclass for internal data, Pydantic for data crossing a boundary** (API input, LLM output) where you need validation.

**Why it matters:** Shows you write clean, typed Python. Interviewers like the *dataclass vs Pydantic* distinction — internal vs validated-at-the-edge.

---

## 7. HTTP & REST — calling APIs (`requests` / `httpx`)

**Definition:** **REST** is a convention for talking to a service over HTTP: a **verb** (GET read, POST create, PUT/PATCH update, DELETE remove), a **URL**, headers (auth), a JSON body, and a **status code** back. `requests` is the classic sync client; **`httpx`** is the modern one that also does **async** (so it fits FastAPI).

**Example:**
```python
import httpx

async with httpx.AsyncClient(timeout=30) as client:
    r = await client.post(
        "https://api.example.com/v1/chat",
        headers={"Authorization": f"Bearer {token}"},
        json={"prompt": "hi"},
    )
    r.raise_for_status()        # turn a 4xx/5xx into an exception
    data = r.json()
```

**Status codes worth knowing:** `200` OK · `201` created · `400` bad request (your fault) · `401/403` auth · `404` not found · `429` rate-limited (back off + retry) · `5xx` server error (retry).

**Why it matters:** Every LLM call *is* an HTTP call. Handling `429`/`5xx` with retry + backoff and setting **timeouts** is what separates a toy script from a production client.

---

## 8. FastAPI (the receptionist for your service)

**Definition:** **FastAPI** is an async-native Python web framework. It routes requests, validates input/output with **Pydantic**, auto-generates OpenAPI docs, and supports **dependency injection**.

**Example — how a clean AI backend is structured:**
```python
from fastapi import FastAPI, Depends
app = FastAPI()

@app.post("/chat")
async def chat(req: ChatRequest, svc: LLMService = Depends(get_llm)):
    return await svc.answer(req.prompt)     # validated in, structured out
```
Layers: **routers** per domain → **Pydantic models** for I/O → a **service layer** for LLM/tool logic → **streaming** responses for chat → **background tasks** for long jobs.

**Why it matters:** The default answer to *"how do you expose an LLM as an API?"* Say **async support + Pydantic validation + auto docs + DI**, then describe the layering. See deployment in [[20-deployment-api-serving]].

---

## 9. Streaming responses (watching it type)

**Definition:** Instead of waiting for the whole answer, the server sends tokens **as they're generated** — usually via **SSE (Server-Sent Events)** or a chunked response. In FastAPI that's a `StreamingResponse` over an async generator.

**Example:** the live *"typing…"* effect in ChatGPT — words appear one at a time. Same idea: your endpoint `yield`s each token chunk as the model produces it.

**Why it matters:** Perceived latency. The user sees output in ~200ms instead of staring at a spinner for 8 seconds — even though total time is the same. Expected for any chat UI.

---

## 10. Boto3 & the AWS SDK (the universal remote for AWS)

**Definition:** **Boto3** is Python's AWS SDK — one client per service (S3, Bedrock, DynamoDB…). You call methods; it signs and sends the HTTP requests.

**Example — badge vs sticky-note password:**
- ❌ Hardcoding access keys in code = writing your password on a sticky note.
- ✅ **IAM roles** = your server *wears a badge*; AWS hands it temporary credentials automatically. No secrets in code.
```python
import boto3
s3 = boto3.client("s3")                       # picks up the role automatically
for page in s3.get_paginator("list_objects_v2").paginate(Bucket="my-bucket"):
    ...                                        # pagination: don't assume one page
```

**Why it matters:** Three things interviewers listen for: **IAM roles, not hardcoded keys**; **pagination** (results come in pages); **retries/backoff** on throttling (esp. Bedrock `ThrottlingException`). See [[17-aws-core]] and [[18-aws-bedrock-sagemaker]].

---

## 11. Pandas essentials (Excel, in code)

**Definition:** **Pandas** works with tabular data. A **DataFrame** is a spreadsheet you manipulate with code — filter rows, group, join, aggregate.

**Example:**
```python
import pandas as pd
df = pd.read_csv("evals.csv")
df[df.score < 0.5]                      # filter failing rows
df.groupby("model").score.mean()        # average score per model
```

**Why it matters:** For AI work it's the glue for **eval results, logs, and datasets** — loading test cases, scoring outputs, summarizing a run. You don't need deep Pandas; you need to slice, group, and merge fluently.

---

## 12. SQLAlchemy & DB connectivity (a pool of open phone lines)

**Definition:** **SQLAlchemy** is Python's ORM/DB toolkit — map tables to classes, run queries without hand-writing every SQL string. It manages a **connection pool** so you reuse open DB connections.

**Example — pre-opened phone lines:** opening a fresh DB connection per request is like **redialing** every call — slow. A **connection pool** keeps a set of lines already open; each request **borrows** one and **returns** it. Under async, use an async driver (`asyncpg`) + `create_async_engine` so DB calls don't block the event loop.

**Why it matters:** Connection exhaustion is a classic production outage. Say *"pool the connections, cap the pool size, and use an async driver so the loop isn't blocked."* Deeper SQL is in [[23-databases-sql]].

---

## 13. Environments & packaging (a separate toolbox per project)

**Definition:** A **virtual environment** (`venv`) is an isolated set of packages for one project. **`pip`** installs them; **`uv`** is a much faster modern installer/resolver. A **`.env`** file holds secrets/config as environment variables, read at runtime — never committed.

**Example:** project A needs `langchain==0.2` and project B needs `0.3`. Separate venvs = separate toolboxes, so the versions don't clash. Secrets (API keys) go in `.env` (git-ignored) = a sealed envelope kept out of the code.

**Why it matters:** *"How do you manage dependencies and secrets?"* → venv/uv per project, pin versions (`requirements.txt`/`pyproject.toml`), secrets in env vars, `.env` git-ignored. Reproducibility + no leaked keys.

---

## 14. Testing an LLM-dependent app (mock the model, eval separately)

**Definition:** Split testing in two: **unit tests** for your deterministic code (with the LLM **mocked**), and a separate **eval suite** for model *quality* (which is non-deterministic and slow).

**Example — a stand-in actor:** in unit tests you replace the real LLM (slow, costs money, different answer every time) with a **stand-in** that returns a canned response — so you're testing *your* parsing/routing/error handling, not the model. Then a separate, smaller eval run measures actual answer quality.
```python
def test_parses_reply(mocker):
    mocker.patch("app.llm.complete", return_value='{"ok": true}')
    assert handle("hi") == {"ok": True}     # fast, deterministic, free
```

**Why it matters:** *"How do you test something non-deterministic?"* → **mock the LLM boundary** for fast/repeatable unit tests; keep a **separate eval suite** (not in CI unit tests) for quality. Ties to [[06-evaluation]]. Tools: `pytest`, fixtures, `pytest-asyncio`, `unittest.mock`.

---

## Quick misconceptions to avoid
- ❌ "Threads make Python faster." → Not for CPU work — the **GIL** serializes it. Use **processes** for CPU.
- ❌ "Async is always faster." → Only for **I/O-bound** work; for CPU it adds nothing (and a blocking call freezes the loop).
- ❌ "Type hints validate data." → They don't at runtime; **Pydantic** does the actual checking/coercion.
- ❌ "Store AWS keys in the code/.env on the server." → Use **IAM roles**; keep secrets out of the repo.
- ❌ "Unit tests should call the real LLM." → Mock it; test quality in a **separate eval suite**.

---
_Related: [[07-agents-tool-use]] · [[20-deployment-api-serving]] · [[17-aws-core]] · [[18-aws-bedrock-sagemaker]] · [[23-databases-sql]] · [[06-evaluation]] · [[03-prompt-engineering]]_
