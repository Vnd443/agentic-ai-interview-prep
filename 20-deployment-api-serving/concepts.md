# Deployment & API Serving — Concepts (learn-first)

> Read top to bottom once. Each concept: plain definition → a real-world example → why it matters in an interview.
> The through-line: **how a notebook becomes a service that strangers can hit reliably.**

---

## 1. Why serving is its own skill

**Definition.** Serving is everything between "it works when I run the cell" and "thousands of users can call it over the internet, safely and fast." A model is only useful once it's reachable, reliable, and doesn't leak your keys.

**Real-world example.** Cooking a great dish at home vs opening a restaurant. The recipe (the model call) is the easy part. The restaurant needs a menu people can order from (API), a kitchen that scales at dinner rush (autoscaling), and a locked safe for the cash (secrets). That operational layer is serving.

**Why it matters.** "Get this agent to production" is a standard FDE prompt. Treating it as a real engineering problem — not "just deploy it" — is the whole signal.

---

## 2. FastAPI — putting an API in front of the model

**Definition.** **FastAPI** is a Python web framework used to wrap your LLM/agent logic in HTTP endpoints. You define a request model (what comes in) and a response model (what goes out), and it's async by default — which fits LLM calls that spend most of their time waiting on the model.

**Real-world example.** A restaurant menu. The customer doesn't walk into the kitchen; they pick from a defined menu (the endpoint + request schema) and get a plated dish back (the response). FastAPI is that menu-and-counter between the world and your kitchen.

**Why it matters.** It's the default in the capstone stack. Being able to say "async endpoint, Pydantic request/response models" shows you've actually served a model, not just called one.

---

## 3. Streaming responses (SSE)

**Definition.** Instead of waiting for the model to finish the whole answer and then replying, you **stream tokens as they're generated**, usually over **Server-Sent Events (SSE)**. The user sees text appear word-by-word.

**Real-world example.** A waiter bringing dishes as each is ready instead of making you wait until the entire table's food is plated. The ChatGPT typewriter effect is exactly this.

**Why it matters.** Long answers feel broken if the user stares at a spinner for 20 seconds. "How do you stream?" → "SSE from a FastAPI streaming endpoint, yielding tokens as the model produces them." Perceived latency drops even though total time is the same.

---

## 4. Docker — containerizing the app

**Definition.** **Docker** packages your app + its exact dependencies + runtime into an **image** that runs identically anywhere. No "works on my machine."

**Real-world example.** A shipping container. However weird your cargo, it fits the standard container, and any port/ship/truck can handle it. Your app's messy dependencies get sealed into a standard box.

**Why it matters.** It's the unit of deployment for Fargate/ECS. Bonus signals: slim base images, a `.dockerignore`, and — critically — **not baking secrets into the image** (see #6).

---

## 5. Where to run it — Fargate vs Lambda vs EC2

**Definition.** Three common AWS targets:
- **ECS Fargate** — run containers without managing servers. Good default for a long-lived API. (Capstone choice.)
- **Lambda** — serverless functions; great for short, spiky, event-driven work; cold starts + time limits make long agent runs awkward.
- **EC2** — raw VMs; most control, most ops burden; needed for GPUs or special setups.

**Real-world example.** Getting around a city: **Fargate** is a rental car (you drive, no garage to maintain), **Lambda** is a taxi (perfect for a quick one-off, pricey/awkward for a all-day trip), **EC2** is buying your own car (full control, you handle everything).

**Why it matters.** "Fargate vs Lambda vs EC2?" is a real question. Answer by workload shape: steady API → Fargate; short bursty events → Lambda; GPU/full control → EC2.

---

## 6. Secrets & config — keys don't go in the image

**Definition.** API keys and credentials are injected at **runtime** via environment variables or a manager like **AWS Secrets Manager** — never hardcoded in the code or baked into the Docker image.

**Real-world example.** You don't print your house key onto the front door. You keep it separate and use it when you arrive. A secret baked into an image is a key glued to the door for anyone who pulls the image.

**Why it matters.** "Where do the keys go?" is a trap check. "Not in the image or the repo — env vars from Secrets Manager at runtime" is the pass. Ties to [[14-safety-guardrails]] and [[17-aws-core]].

---

## 7. Health checks & readiness

**Definition.** Small endpoints (e.g. `/health`) the platform pings to know the container is **alive** and **ready** for traffic. If it fails, the orchestrator restarts or stops routing to it.

**Real-world example.** A shop's "Open / Closed" sign and a pulse check. The mall (load balancer) only sends customers to shops showing "Open." A dark shop gets skipped and someone checks on it.

**Why it matters.** Without health checks, a hung container keeps receiving traffic and users hit errors. It's a small detail that signals you've run things in production.

---

## 8. Rate limiting & timeouts

**Definition.** Caps on how many requests a client can make (**rate limiting**) and how long a call may run (**timeouts**) — protecting both the service and your model budget from abuse or runaway loads.

**Real-world example.** A theme-park ride with a max riders-per-hour and a "you must finish in N minutes." Keeps the queue moving and stops one group from hogging everything.

**Why it matters.** LLM calls cost money per token — an unthrottled endpoint is a budget and availability risk. Mentioning it unprompted shows cost + reliability awareness ([[15-cost-optimization]]).

---

## 9. Long runs — async workers & queues

**Definition.** When an agent run takes tens of seconds to minutes, you don't hold the HTTP request open. You accept the job, put it on a **queue**, let a **worker** process it, and let the client poll or get notified when it's done.

**Real-world example.** Dropping film off to be developed: you don't stand at the counter while they process it — you get a ticket and come back. The counter (API) stays free for the next customer.

**Why it matters.** "An agent run takes 40s — how do you avoid timing out?" → "Don't do it inline; enqueue it, process in a worker, return a job id, poll/notify on completion." Classic senior answer. (Mirrors the A2A task lifecycle in [[13-a2a-agent-to-agent]].)

---

## 10. CI/CD — build, push, deploy

**Definition.** An automated pipeline: on a commit, **build** the Docker image, **push** it to a registry (ECR), and **deploy** the new version to Fargate — with health checks gating the rollout.

**Real-world example.** A dishwasher on a cycle vs washing each plate by hand. Push code → the pipeline handles build/test/ship the same way every time, no manual fumbling at 2am.

**Why it matters.** Shows you ship repeatably, not by hand. Deep-dive lives in [[19-iac-devops]]; here just know the build → push → deploy chain for an LLM service.

---

## Quick misconceptions to avoid
- ❌ "Deploy = it's done." → Serving adds streaming, scaling, secrets, health, timeouts — the reliability layer.
- ❌ "Put the API key in the Dockerfile / env in the repo." → Inject at runtime from a secrets manager; never in the image or git.
- ❌ "Run the long agent inline in the request." → Enqueue it; workers process; client polls. Otherwise you time out.
- ❌ "Lambda for everything." → Cold starts + time limits hurt long LLM/agent runs; a steady API is usually Fargate.
- ❌ "Streaming is just cosmetic." → It slashes *perceived* latency, which is often what users actually judge.

_Related: [[17-aws-core]] · [[19-iac-devops]] · [[22-llm-system-design]] · [[15-cost-optimization]]_
