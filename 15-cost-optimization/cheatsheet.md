# Cost Optimization — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Mindset
- Per-token tax on every request, forever —
- Optimize recurring cost, not one-off —
- Quality floor first, then cut —

## Token cost model
- Pay input + output —
- Output ~3–5× input price —
- Bigger model = higher per-token —

## The levers (impact order)
- Model selection + routing ⭐ (60–80%) —
- Prompt optimization / compression —
- Semantic cache (~0 on hit) —
- Prompt caching (~90% off static prefix) —
- Batch API (~50% off) —
- Fine-tune small model (per-call cheap) —
- Output control: max_tokens + structured —

## Routing
- Smallest model that clears the bar —
- Cheap classifier/heuristic picks tier —
- Escalate only hard queries —
- Measure escalation rate —

## Caching risks
- Stale answers / false hits —
- Tune similarity threshold + TTL —

## Estimating
- req/day × (in + out tokens) × price/token —
- Split input vs output pricing —
- Buffer for retries + system prompt —
- Attack the biggest term —

## Don't over-optimize
- Cost per SUCCESSFUL task (not per call) —
- Cheap fail → retry + escalate = not cheap —

## Numbers / facts worth quoting
- Routing: 60–80% savings —
- Batch API: ~50% off —
- Prompt caching: ~90% off cached reads —
- Output tokens: ~3–5× input price —
