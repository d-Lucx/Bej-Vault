For people who keep asking what to build in AI Engineering.

- Build your own Context Assembler :

(token-budgeted memory + retrieval + tools)

- Build your own Retrieval Stack :

(chunker + BM25 + dense search + reranker)

- Build your own Model Router :

(cost/latency/quality routing + fallbacks)

- Build your own Semantic Cache :

(embedding similarity + hit-rate tracking)

- Build your own Agent Orchestrator :

(deterministic state machine, no LangChain)

- Build your own MCP Server and Client :

(raw JSON-RPC, no SDK)

- Build your own Multi-Agent Consensus :

(weighted voting + judge + escalation)

- Build your own Sandboxed Tool Executor :

(isolated execution + resource limits)

- Build your own Guardrails Middleware :

(injection detection + PII redaction)

- Build your own Durable Workflow Engine :

(checkpoint/resume, mini-Temporal)

- Build your own Streaming Proxy :

(SSE + TTFT and ITL metrics)

- Build your own LLM Tracer :

(OpenTelemetry-style spans for every hop)

- Build your own Eval Harness :

(trajectory grading + CI regression gates)

- Build your own Prompt Registry :

(versioning + A/B routing + rollback)

- Build your own Data Flywheel :

(feedback to synthetic data to LoRA loop)

Pick 3. Build them from scratch.

Document every decision.

Most people import libraries.

Builders understand what happens underneath.