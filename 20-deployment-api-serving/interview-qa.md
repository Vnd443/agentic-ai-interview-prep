# Deployment & API Serving — Interview Q&A

Copy-paste template for any deployment answer:

> **Wrap it** (FastAPI + streaming) → **package it** (Docker) → **run it** (Fargate/Lambda/EC2, by workload) → **make it safe & reliable** (secrets, health, timeouts, rate limits) → **long runs** (queue + worker) → a trade-off or number.

---

**Q. You've got an agent working in a notebook. How do you get it to production?**

I wrap the agent logic in a FastAPI service with a defined request and response model, and stream the output over SSE so the user sees tokens as they generate. I containerize it with Docker — slim image, secrets injected at runtime, not baked in. I deploy the container to ECS Fargate behind a load balancer with autoscaling and a health-check endpoint. Keys come from Secrets Manager via env vars. For long agent runs I don't block the request — I enqueue the job and let a worker process it. And the whole thing ships through a CI/CD pipeline: build image → push to ECR → deploy. That's the notebook-to-service path.

---

**Q. How do you stream an LLM's response instead of making the user wait for the whole thing?**

I use a streaming endpoint in FastAPI that yields tokens over Server-Sent Events as the model produces them, so text appears word-by-word — the ChatGPT typewriter effect. The total generation time is the same, but perceived latency drops dramatically because the user sees progress immediately instead of staring at a spinner for 20 seconds. For a 40-second answer that difference is the gap between "feels broken" and "feels responsive."

---

**Q. Where do the API keys go?**

Never in the code, the repo, or the Docker image — anyone who pulls the image would get them. They're injected at runtime as environment variables sourced from AWS Secrets Manager. The image stays generic and secret-free; the credentials are attached only when the container runs in its environment. Baking a key into an image is like gluing your house key to the front door.

---

**Q. Fargate vs Lambda vs EC2 for serving an LLM app — how do you choose?**

By the shape of the workload. A steady, long-lived API is ECS Fargate — I run containers without managing servers, and it's my default. Short, spiky, event-driven work suits Lambda, but cold starts and its execution time limit make long agent runs awkward. EC2 is for when I need full control or GPUs and can accept the ops burden. Analogy: Fargate is a rental car, Lambda is a taxi, EC2 is owning the car. For a typical agent API, Fargate.

---

**Q. An agent run takes 40 seconds. How do you handle that without timing out the request?**

I don't run it inline in the HTTP request. I accept the request, put the job on a queue, return a job id immediately, and let a background worker process it. The client polls for status or gets notified when it's done. That keeps the API responsive and the request thread free for other users, and it survives requests that would otherwise blow past a gateway timeout. It's the same task-lifecycle idea as A2A — submitted → working → completed.

---

**Q. How do you keep a served LLM endpoint from becoming a cost or reliability risk?**

Rate limiting and timeouts per client so no one can hammer the endpoint or trigger runaway token spend, autoscaling behind a load balancer to absorb real load, and health/readiness checks so the platform stops routing to a hung container and restarts it. On cost specifically, caps plus timeouts protect the model budget, and I'd pair that with the levers in cost optimization — routing, caching. The point is the endpoint is metered and self-protecting, not wide open.
