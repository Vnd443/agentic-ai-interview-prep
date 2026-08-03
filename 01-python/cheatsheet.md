# Python — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Async & concurrency
- Sync vs async (blocking vs non-blocking) —
- Event loop —
- `async` / `await` —
- `asyncio.gather` (concurrent fan-out) —
- Blocking call inside async → executor / `to_thread` —
- GIL (one thread runs bytecode at a time) —
- CPU-bound → multiprocessing —
- I/O-bound → async / threads —

## Types & data modeling
- Type hints (`str`, `-> int`) — hints only, no runtime check —
- Pydantic (runtime validation + coercion) —
- Structured LLM output → Pydantic model —
- Classes vs `@dataclass` —
- dataclass (internal) vs Pydantic (edge/validated) —

## HTTP & APIs
- REST verbs (GET/POST/PUT/PATCH/DELETE) —
- Status codes (200 / 400 / 401 / 404 / 429 / 5xx) —
- `requests` vs `httpx` (httpx = async) —
- `raise_for_status`, timeouts —
- Retry + backoff on 429 / 5xx —

## FastAPI
- Why FastAPI (async + Pydantic + docs + DI) —
- Structure: routers → models → service layer —
- Dependency injection (`Depends`) —
- Streaming responses / SSE —
- Background tasks —

## AWS / Boto3
- Client per service —
- IAM roles (not hardcoded keys) —
- Pagination (paginator) —
- Retries / backoff (throttling) —
- Bedrock runtime client —

## Data & DB
- Pandas DataFrame (filter / groupby / merge) —
- SQLAlchemy (ORM) —
- Connection pooling —
- Async driver (asyncpg) so loop isn't blocked —

## Environments & packaging
- venv (isolated per project) —
- pip / uv —
- Pin versions (requirements.txt / pyproject.toml) —
- `.env` / env vars for secrets (git-ignored) —

## Testing
- Mock the LLM boundary —
- Unit tests = deterministic + fast —
- Separate eval suite for quality —
- pytest / fixtures / pytest-asyncio —

## Numbers / facts worth quoting
- 
