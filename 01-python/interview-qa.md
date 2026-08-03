# Python — Interview Q&A

> Simple format. To add a new question, just copy this and fill it in:
>
> **Q. your question?**
>
> your answer in plain words.
>
> ---

> Starter set (not from the guide PDF). Your stack: **FastAPI, Boto3, Pandas, SQLAlchemy, async, testing.**

---

**Q1. How does async work in Python, and when does it help an AI service?**

`async`/`await` runs on a single-threaded **event loop** that interleaves I/O-bound work: while one coroutine awaits a slow call (an LLM API), the loop runs others. It shines for **I/O-bound** workloads like an LLM gateway making many concurrent API calls, and does *not* speed up CPU-bound work (use processes for that). Use `asyncio.gather` to fan out many calls at once instead of awaiting them one by one. Key line: "LLM apps are I/O-bound on API calls — async lets one worker handle many in-flight requests instead of blocking per call." Watch out for a blocking call inside async — it freezes the whole loop, so push it to a thread/executor.

---

**Q2. What is the GIL, and why don't threads speed up CPU work?**

The GIL (Global Interpreter Lock) lets only one thread execute Python bytecode at a time, so threads take turns rather than running Python truly in parallel. For CPU-bound work (parsing, math) more threads don't help — they just wait for the lock — so you use multiprocessing, where each process has its own GIL and gets a real core. For I/O-bound work threads and async *do* help, because waiting on I/O releases the lock. Short version: "processes for CPU, async/threads for I/O."

---

**Q3. Why FastAPI for an AI backend, and how do you structure one?**

FastAPI is async-native, validates I/O with Pydantic (great for typed requests and structured LLM output), auto-generates OpenAPI docs, and supports dependency injection. I structure it as routers per domain, Pydantic models for input/output, a service layer for the LLM/tool logic, streaming responses for chat, and background tasks for long jobs. The async support lets one worker fan out many model/tool calls concurrently.

---

**Q4. How do you handle type safety and structured output in Python?**

Type hints document shapes but don't enforce anything at runtime; Pydantic turns those hints into a real validator that checks and coerces incoming data and raises a clear error when it doesn't fit. I use dataclasses for internal containers and Pydantic models at the boundaries — API input and LLM output — so I can make the model return JSON and parse it straight into a Pydantic model, and bad shapes fail loudly instead of leaking downstream.

---

**Q5. How do you call external APIs (like an LLM) safely from Python?**

Every LLM call is an HTTP call, so I use `httpx` (async, so it fits FastAPI), set an explicit timeout, and call `raise_for_status()` to turn 4xx/5xx into exceptions. I retry with backoff on 429 (rate-limited) and 5xx (server error), and keep the auth token in an env var, not the code. The mindset is treating the model endpoint like any unreliable network dependency — timeouts and retries, not blind trust.

---

**Q6. How do you use Boto3 well (and safely)?**

Use a client per service (S3, Bedrock, DynamoDB), rely on IAM roles instead of hardcoded keys, handle pagination with paginators (don't assume one page of results), and configure retries/backoff for throttling. For Bedrock I call the runtime client and back off on ThrottlingException. The headline is IAM roles, not secrets in code.

---

**Q7. How do you manage database connections in a high-traffic service?**

I use SQLAlchemy with a connection pool so requests borrow an already-open connection instead of opening a new one each time — opening per request is like redialing every call and exhausts the database. I cap the pool size, and under async I use an async driver like asyncpg with `create_async_engine` so DB calls don't block the event loop. Connection exhaustion is a classic outage, so pooling plus a sane cap is the answer.

---

**Q8. How do you test an LLM-dependent Python app?**

I split it in two. Unit tests cover my deterministic code (parsing, routing, error handling) with the LLM boundary mocked, so they're fast, free, and repeatable. A separate, smaller eval suite measures actual model quality, since that's non-deterministic and slow and doesn't belong in CI unit tests. Tools: pytest, fixtures, pytest-asyncio, and `unittest.mock` for the model boundary.

---

## Your notes / STAR angle
- _TODO: a Python service you built and a tricky async/perf or connection-pool bug you solved (with the before → after number)._
