# Python — Interview Q&A

> Starter set (not from the guide PDF). Your stack: **FastAPI, Boto3, Pandas, SQLAlchemy, async, testing.**

---

## Q1. How does async work in Python, and when does it help an AI service?

**Ideal answer:** `async`/`await` uses a single-threaded **event loop** to interleave I/O-bound work — while one coroutine awaits a network call (an LLM API), others run. It shines for **I/O-bound** workloads like an LLM gateway making many concurrent API calls; it does *not* speed up CPU-bound work (use processes for that). FastAPI is async-native, so you can fan out many model/tool calls concurrently.

**🔑 Power move:** "LLM apps are I/O-bound on API calls — async lets one worker handle many in-flight requests instead of blocking per call."

**Follow-ups:** `asyncio.gather` vs. sequential awaits? GIL and why threads don't help CPU work? Blocking calls inside async (run in executor)?

---

## Q2. Why FastAPI for an AI backend, and how do you structure one?

**Ideal answer:** FastAPI gives async support, **Pydantic** validation (great for typed request/response + LLM structured output), auto OpenAPI docs, and dependency injection. Structure: routers per domain, Pydantic models for I/O, a service layer for LLM/tool logic, streaming responses for chat, background tasks/queues for long jobs, and clear error handling.

**Follow-ups:** Streaming responses (SSE)? Handling long-running LLM calls (timeouts/background)?

---

## Q3. How do you use Boto3 well (and safely)?

**Ideal answer:** Use clients/resources per service (S3, Bedrock, etc.), rely on **IAM roles** not hardcoded keys, handle pagination + retries (botocore config), and stream large S3 objects. For Bedrock, call the runtime client and handle throttling with backoff.

---

## Q4. How do you test an LLM-dependent Python app?

**Ideal answer:** Unit-test deterministic logic; **mock the LLM/API** boundary for fast tests; keep a separate, smaller **eval suite** for actual model quality (not in unit tests). Use pytest, fixtures, and record/replay for external calls.

---

## Your notes / STAR angle
- _TODO: a Python service you built and a tricky async/perf bug you solved._
