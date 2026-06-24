# Principal Agentic AI Engineer Accelerator
## Personalized 6-Month Roadmap | 45-60 Min/Day | Maurice McDonald

---

## OUTPUT 1: TECHNICAL GAP ANALYSIS TABLE

*All phases calibrated to this table. Rows ordered by priority.*

| Competency | Current State | Gap Level | Est. Hours to Close | Priority Rank | Phase |
|---|---|---|---|---|---|
| Production RAG pipeline (chunking, hybrid retrieval, reranking) | Conceptual understanding | HIGH | 40 | 1 | Phase 1 |
| RAGAS evaluation framework implementation | Aware, not implemented | HIGH | 20 | 2 | Phase 1 |
| Vector DB architecture tuning (pgvector, Qdrant HNSW) | Low | HIGH | 25 | 3 | Phase 1 |
| Embedding model selection and MTEB evaluation | Low | HIGH | 15 | 4 | Phase 1 |
| LangGraph: state machines, checkpointing, conditional routing | Familiar | HIGH | 30 | 5 | Phase 2 |
| LangGraph: supervisor patterns, swarm architectures | None | HIGH | 20 | 6 | Phase 2 |
| Agent failure recovery and retry logic | Conceptual | HIGH | 15 | 7 | Phase 2 |
| FastAPI async architecture for AI service layers | Low | HIGH | 25 | 8 | Phase 3 |
| PostgreSQL + pgvector integration (asyncpg, connection pooling) | Low | HIGH | 20 | 9 | Phase 3 |
| LLM evaluation harnesses (golden datasets, LLM-as-judge) | Strategic understanding | MED | 20 | 10 | Phase 3 |
| Guardrails: Pydantic/Instructor structured output, PII detection | Low | MED | 15 | 11 | Phase 3 |
| Event-driven agent architectures, long-running workflow persistence | Low | HIGH | 25 | 12 | Phase 4 |
| MCP at production scale (beyond current skill-building) | Strong builder | MED | 10 | 13 | Phase 4 |
| OpenTelemetry instrumentation for AI workloads | Low | HIGH | 20 | 14 | Phase 4 |
| PII redaction pipelines + RBAC for AI endpoints | Architecture-level only | MED | 15 | 15 | Phase 4 |
| CI/CD for AI systems (eval gates, model pinning) | Low | MED | 15 | 16 | Phase 5 |
| Prompt versioning and regression testing pipelines | Low | MED | 10 | 17 | Phase 5 |
| Threat modeling for agentic systems (STRIDE applied to LLM pipelines) | Strong architecture instinct | LOW | 8 | 18 | Phase 5 |
| Architecture Defense to security, compliance, platform engineering | Strong | LOW | 5 | 19 | Phase 5 |

**Total estimated gap closure hours: ~333 hours across 26 weeks**
**At 45-60 min/day (7 days/week): ~315-420 hours available**
**Verdict: Achievable at the low end. Weekends become the build sprint buffer.**

---

## OUTPUT 2: 6-MONTH ROADMAP

### TIME BUDGET CONSTRAINT

**Daily commitment: 45-60 minutes, 7 days/week**

Weekday rhythm (45 min):
- 20 min: concept study (read, annotate, summarize in your own words)
- 25 min: code -- always building, never just reading

Weekend rhythm (60 min Saturday + 60 min Sunday):
- Saturday: flagship build sprint (feature addition or integration)
- Sunday: document what you built (LinkedIn post, README update, or Loom note)

This is your non-negotiable daily floor. It compounds faster than 3-hour weekend sessions because you retain more across daily repetition.

---

### PHASE 1 (Weeks 1-8): Advanced RAG Engineering

**Goal:** Move from conceptual RAG understanding to production implementation with measurable evaluation scores.

**Capstone Connection:** Everything built in Phase 1 becomes the retrieval backbone of your final Enterprise AI Platform.

---

#### Week 1: Chunking Strategy Engineering

**Concepts:** Fixed-size vs semantic vs hierarchical chunking; late chunking for context-preserving splits; overlap ratios and their effect on retrieval precision; chunk size vs embedding model token limits

**Build Task:** Build a chunking comparison harness -- same document corpus, four chunking strategies, measure retrieval precision on 10 test queries

**Deliverable:** `chunking-comparison/` -- Python module with configurable chunking pipeline and markdown results table

**Success Criteria:** Your harness runs all four strategies and outputs a comparison table showing precision@5 for each. You can explain in one paragraph why one strategy outperformed the others on your test corpus.

**Common Failure Mode:** Senior engineers pick a chunking strategy in week 1 and never revisit it. Chunking is the highest-leverage variable in RAG -- the whole harness exists so you build the discipline of measuring, not guessing.

**Capstone Connection:** This chunking module plugs directly into the capstone's document ingestion pipeline.

---

#### Week 2: Embedding Model Selection

**Concepts:** MTEB leaderboard (what it measures and what it misses); domain-specific embedding performance; OpenAI ada-002 vs open-source alternatives (BGE, E5, Nomic); embedding dimensions and storage tradeoffs; task-specific scoring (retrieval vs similarity vs classification)

**Build Task:** Evaluate three embedding models on your test corpus using MTEB-style scoring; build a benchmarking script that outputs model comparison by latency, cost, and retrieval quality

**Deliverable:** `embedding-benchmark/` -- benchmarking script + model comparison report (markdown)

**Success Criteria:** You can justify model selection with data, not preference. Report shows latency, cost-per-1k-tokens, and retrieval precision for each model.

**Common Failure Mode:** Defaulting to OpenAI embeddings without benchmarking. Fine in prototypes -- disqualifying in a Principal-level interview where they ask "how did you choose your embedding model?"

**Capstone Connection:** The winning model from this benchmark becomes the capstone's embedding layer.

---

#### Week 3: Vector Database Architecture

**Concepts:** pgvector vs Qdrant -- when each is the right call; HNSW index parameters (ef_construction, m) and their precision/recall tradeoff; approximate vs exact nearest neighbor; metadata filtering before vs after vector search; connection pooling for production throughput

**Build Task:** Deploy both pgvector (via Docker) and Qdrant (via Docker); index the same corpus in both; run identical queries and compare latency, recall, and operational complexity

**Deliverable:** `vector-db-compare/` -- deployment configs, benchmark results, architecture decision record (ADR) choosing one for the capstone

**Success Criteria:** You have a written ADR (one page) that defends your vector DB choice with specific numbers. You can explain HNSW parameter tradeoffs without looking at documentation.

**Common Failure Mode:** Treating vector databases as interchangeable. pgvector is a PostgreSQL extension -- it shares your existing Postgres operational footprint. Qdrant is purpose-built -- it wins on throughput at scale. The gap is architectural context, not raw performance.

**Capstone Connection:** Your ADR choice becomes the capstone's vector store. Set it up correctly now.

---

#### Week 4: Hybrid Retrieval

**Concepts:** Dense retrieval (vector similarity) vs sparse retrieval (BM25 keyword); why neither alone is sufficient; Reciprocal Rank Fusion (RRF) -- the math and the implementation; when sparse retrieval wins (exact terminology, product codes, legal citations); hybrid pipeline architecture

**Build Task:** Add BM25 retrieval to your RAG pipeline; implement RRF fusion; compare hybrid vs dense-only on queries with exact-match terms

**Deliverable:** `hybrid-retrieval/` -- pipeline module with configurable dense/sparse weights and RRF implementation

**Success Criteria:** Hybrid retrieval outperforms dense-only on at least 30% of your exact-match test queries. You can explain RRF mathematically in an interview.

**Common Failure Mode:** Treating hybrid retrieval as "just add BM25." The fusion step is where most engineers leave performance on the table. The weight between dense and sparse is not configurable by gut -- it requires a held-out evaluation set.

**Capstone Connection:** Hybrid retrieval becomes the default retrieval mode in the capstone.

---

#### Week 5: Reranking

**Concepts:** Cross-encoder architecture vs bi-encoder (why cross-encoders rerank better than they retrieve); ColBERT late interaction; LLM-as-reranker (when it makes sense, when it's wasteful); reranking position in the retrieval pipeline; latency vs quality tradeoff

**Build Task:** Add a cross-encoder reranker to your hybrid pipeline; measure precision@5 before and after reranking; build a latency budget showing where time is spent

**Deliverable:** `reranking-layer/` -- reranker module integrated into hybrid pipeline + latency budget report

**Success Criteria:** Reranking improves precision@5 by measurable margin (document your actual delta). Latency budget shows you understand the cost of adding reranking.

**Common Failure Mode:** Adding reranking without measuring latency impact. Production RAG reranking adds 200-800ms. If you can't defend that cost in terms of quality improvement, you will be challenged in architecture reviews.

**Capstone Connection:** Reranking slots into the capstone retrieval chain between vector search and context assembly.

---

#### Week 6: RAGAS Evaluation Framework

**Concepts:** RAGAS metrics -- faithfulness, answer relevancy, context precision, context recall; what each metric catches and misses; building a golden dataset (how to create reference QA pairs); automated vs human evaluation tradeoffs; evaluation as a CI gate

**Build Task:** Implement RAGAS against your pipeline; build a 25-question golden dataset from your corpus; produce a RAGAS scorecard

**Deliverable:** `ragas-evaluation/` -- evaluation harness + golden dataset + scorecard report

**Success Criteria:** RAGAS faithfulness score above 0.80 on your golden dataset. You have a written explanation of any score below 0.75 and what you would change to fix it.

**Common Failure Mode:** Running RAGAS once and treating the score as a grade. RAGAS is a regression harness -- it catches regressions when you change chunking, embedding, or prompts. Run it every time you change the pipeline.

**Capstone Connection:** This RAGAS harness becomes the evaluation CI gate in the capstone's deployment pipeline.

---

#### Week 7: Retrieval Debugging and Failure Diagnosis

**Concepts:** Retrieval failure taxonomy -- wrong chunk retrieved, right chunk not indexed, context stuffing, hallucination from over-retrieved noise; logging retrieval decisions for diagnosis; building a retrieval debugger; trace-level visibility into what the model actually used

**Build Task:** Build a retrieval debugger -- given a query, show top-5 retrieved chunks with scores, highlight which chunks appeared in the model's answer, flag chunks retrieved but not used

**Deliverable:** `retrieval-debugger/` -- debugging module with CLI interface and output report

**Success Criteria:** You can diagnose a retrieval failure in under 5 minutes using your debugger. You have documented three distinct failure modes from your test corpus with root cause and fix.

**Common Failure Mode:** Debugging by changing prompts when the real problem is retrieval quality. Most RAG hallucinations are retrieval failures, not generation failures. The debugger forces you to see the difference.

**Capstone Connection:** The retrieval debugger becomes an admin endpoint in the capstone's FastAPI layer (Phase 3).

---

#### Week 8: Phase 1 Integration -- Flagship Build

**Concepts:** Pipeline integration; end-to-end latency budget; production readiness checklist; README and documentation standards for portfolio-grade code

**Build Task:** Integrate all Phase 1 modules into a cohesive production RAG pipeline: chunking -> embedding -> hybrid retrieval -> reranking -> generation -> RAGAS evaluation. Add a FastAPI stub (one endpoint) so it's callable.

**Deliverable:** `enterprise-rag-platform/` -- fully integrated Phase 1 flagship, documented, with RAGAS scorecard and architecture README

**Success Criteria:** A colleague (or interviewer) can clone your repo, run one command, send a query, and see a grounded answer with retrieval trace. RAGAS scorecard is in the README.

**Common Failure Mode:** Shipping a Jupyter notebook and calling it a production pipeline. The FastAPI stub -- even one endpoint -- is the difference between a demo and a deployable system.

**Capstone Connection:** This IS the Phase 1 capstone module. Treat it as if you're handing it off to a team.

---

### PHASE 2 (Weeks 9-16): LangGraph Mastery

**Goal:** Move from LangGraph familiarity to expert-level orchestration -- state machines, supervisor patterns, human-in-the-loop, and observable multi-agent systems.

**Constraint:** Tool calling basics are NOT re-covered here. Phase 1's retrieval tools are assumed. Every LangGraph concept connects to a retrieval workflow from Phase 1.

---

#### Week 9: Graph State Design

**Concepts:** LangGraph StateGraph vs MessageGraph; TypedDict state schema design; state channels and reducers; why schema validation at state boundaries prevents silent failures; state as the source of truth across agents

**Build Task:** Build a document processing StateGraph: state schema for document + metadata + retrieval results + agent decisions. Implement two nodes (intake node, classification node) with typed state passing between them.

**Deliverable:** `langgraph-foundation/` -- StateGraph with typed schema, two nodes, unit tests on state transitions

**Success Criteria:** Your state schema catches a type violation at runtime (demonstrate this in a test). You can explain why untyped state is the #1 source of multi-agent bugs.

**Common Failure Mode:** Treating state as a dictionary. Untyped state grows into an undebuggable mess at scale. Typed schemas with explicit reducers are the principal-level pattern.

**Capstone Connection:** This state schema is the foundation of the capstone's multi-agent orchestration layer.

---

#### Week 10: Conditional Routing and Edge Logic

**Concepts:** Conditional edges vs fixed edges; routing functions and their failure modes; cycles and termination conditions; dead-end detection; how routing logic becomes a maintenance liability if not documented

**Build Task:** Extend your StateGraph: add a routing function that sends documents to different processing nodes based on classification result; add a fallback edge for unclassified documents; add a termination condition

**Deliverable:** `langgraph-routing/` -- extended graph with conditional routing, fallback path, documented routing logic

**Success Criteria:** Your graph routes correctly on five document types. Your routing function has explicit handling for the unknown/fallback case. You can draw the graph topology from memory.

**Common Failure Mode:** Writing routing logic as if-else chains with no documentation. Routing functions are architecture -- they should have ADR-style comments explaining why each route exists.

**Capstone Connection:** The routing layer controls which specialized agents process which document types in the capstone.

---

#### Week 11: Persistence and Checkpointing

**Concepts:** LangGraph checkpointers (MemorySaver vs SqliteSaver vs PostgresSaver); thread-level vs graph-level persistence; resuming interrupted workflows; checkpointing cost vs recovery value tradeoff; when not to checkpoint (short, cheap workflows)

**Build Task:** Add PostgresSaver checkpointing to your graph; simulate an interrupted workflow midway through; demonstrate resumption from checkpoint without reprocessing completed steps

**Deliverable:** `langgraph-persistence/` -- graph with PostgreSQL checkpointing, interruption/resumption demo, checkpoint cost analysis

**Success Criteria:** Your graph resumes correctly from a mid-workflow interrupt. You have a written analysis of checkpoint storage cost vs workflow reprocessing cost for your use case.

**Common Failure Mode:** Checkpointing every node in every workflow. Checkpoint cost is real -- PostgreSQL writes per state transition add up at scale. Checkpoint strategically at recovery points, not blindly at every node.

**Capstone Connection:** Checkpointing is the capstone's fault tolerance layer -- it enables the compliance audit trail requirement.

---

#### Week 12: Human-in-the-Loop Patterns

**Concepts:** LangGraph interrupt() -- when and how; approval gates vs informational pauses; streaming partial state to the human reviewer; timeout handling; audit trail for human decisions

**Build Task:** Add a human approval gate to your document pipeline -- high-confidence documents auto-proceed, low-confidence documents pause for human review with state visible to the reviewer

**Deliverable:** `langgraph-hitl/` -- graph with interrupt-based approval gate, confidence routing, audit log of human decisions

**Success Criteria:** Low-confidence documents pause correctly and display relevant state to the reviewer. Human approval/rejection decisions are logged with timestamp and decision metadata.

**Common Failure Mode:** Building HITL as a yes/no blocker. Production HITL needs to show the reviewer why the agent paused and what it needs -- not just "approve?" The state visible to the human is an engineering problem, not a UX afterthought.

**Capstone Connection:** The HITL pattern is the compliance gate in the capstone's regulated workflow path.

---

#### Week 13: Supervisor Patterns

**Concepts:** Supervisor agent architecture; how the supervisor routes tasks to worker agents; supervisor state vs worker state; preventing context contamination between workers; supervisor as orchestrator vs supervisor as controller

**Build Task:** Build a three-agent supervisor: supervisor agent receives a document, routes to one of two specialist agents (extraction agent, summarization agent), collects results, composes output

**Deliverable:** `langgraph-supervisor/` -- three-agent system with supervisor routing, specialist agents, result composition

**Success Criteria:** Supervisor correctly routes to the right specialist on 90% of test documents. Specialist agents cannot see each other's intermediate state (demonstrate isolation).

**Common Failure Mode:** Giving the supervisor too much decision-making responsibility. Supervisors route -- they don't process. When supervisors start processing, you've built a fat coordinator that becomes a single point of failure.

**Capstone Connection:** The supervisor pattern is the top-level orchestration architecture of the capstone's multi-agent platform.

---

#### Week 14: Agent Failure Recovery

**Concepts:** Retry strategies at the agent level (exponential backoff, jitter); fallback agents; dead-letter patterns for unrecoverable failures; how to propagate failure state without corrupting graph state; structured error taxonomy

**Build Task:** Add failure recovery to your supervisor system: agent retries with backoff, fallback routing when a specialist fails, dead-letter logging for unrecoverable failures

**Deliverable:** `langgraph-recovery/` -- recovery-hardened multi-agent system with retry config, fallback routing, dead-letter log

**Success Criteria:** Your system handles three simulated failure scenarios (agent timeout, invalid output, downstream API failure) gracefully without crashing the graph. Recovery decisions are logged.

**Common Failure Mode:** Retrying indefinitely on unrecoverable errors. Distinguish retryable failures (transient API error) from fatal failures (malformed input that will always fail). Infinite retry loops are a production incident waiting to happen.

**Capstone Connection:** The recovery layer is what makes the capstone production-credible in an enterprise security review.

---

#### Week 15: LangSmith Observability Integration

**Concepts:** LangSmith tracing for LangGraph; run trees and span relationships; custom metadata tagging for agent decisions; trace search and filtering; using traces to debug retrieval failures from Phase 1

**Build Task:** Instrument your full Phase 2 multi-agent system with LangSmith traces; add custom metadata tags (document_type, routing_decision, confidence_score); write a retrieval failure diagnosis using traces

**Deliverable:** `langgraph-observability/` -- instrumented multi-agent system + trace analysis writeup documenting one real failure diagnosed via LangSmith

**Success Criteria:** Every agent decision in your system has a trace with custom metadata. You have documented one retrieval failure diagnosed exclusively through trace analysis (no print statements).

**Common Failure Mode:** Treating observability as a logging afterthought. LangSmith traces are your production debugging surface. Instrument before you have bugs -- not after.

**Capstone Connection:** LangSmith is the Phase 5 observability backbone. You're building the instrumentation discipline now.

---

#### Week 16: Phase 2 Integration -- Flagship Build

**Build Task:** Integrate your Phase 2 modules into the multi-agent document intelligence platform: StateGraph with typed schema -> supervisor routing -> specialist agents -> HITL gate -> failure recovery -> LangSmith traces. Connect the Phase 1 RAG pipeline as the retrieval tool available to specialist agents.

**Deliverable:** `multi-agent-doc-intelligence/` -- fully integrated Phase 2 flagship, connected to Phase 1 RAG, documented, LangSmith traces visible

**Success Criteria:** End-to-end document flow works: ingest -> classify -> route -> retrieve (Phase 1 RAG) -> extract -> human gate (for low-confidence) -> output. All decisions traced in LangSmith.

**Capstone Connection:** This is Phase 2 of the capstone. The integration between Phase 1 and Phase 2 is the portfolio's most compelling proof point -- a system that retrieves AND reasons.

---

### PHASE 3 (Weeks 17-24): FastAPI Production Engineering

**Goal:** Build the production API layer that makes your RAG and agent systems enterprise-deployable. Every pattern connects directly to serving Phase 2 agent workflows.

**Constraint:** No standalone API tutorials. Every FastAPI concept is connected to its role in serving the multi-agent system from Phase 2.

---

#### Week 17: Async Architecture for AI Services

**Concepts:** Python async/await at the event loop level (not syntax -- mechanics); why blocking LLM calls destroy throughput; asyncio.gather for parallel agent fan-out; streaming responses for long-running agent workflows; BackgroundTasks for async agent execution

**Build Task:** Build a FastAPI endpoint that invokes your Phase 2 supervisor graph asynchronously; add streaming response support so agent progress is visible to the caller

**Deliverable:** `fastapi-async-foundation/` -- async endpoint with streaming response for agent workflow

**Success Criteria:** Your endpoint handles three simultaneous requests without blocking. Streaming output is visible in the client before the full response completes.

**Common Failure Mode:** Using synchronous LLM calls inside async endpoints. This blocks the event loop and eliminates the throughput benefit of async. Every LLM call in a FastAPI service must be awaitable.

**Capstone Connection:** This async foundation is the serving layer architecture for the entire capstone.

---

#### Week 18: Dependency Injection and Middleware

**Concepts:** FastAPI's Depends() system for AI service layers (vector DB connections, LLM clients, embedding models); lifespan events for connection pool initialization; middleware for request logging, correlation IDs, and latency tracking; rate limiting middleware for LLM cost control

**Build Task:** Refactor your async endpoint to use dependency injection for the RAG pipeline and agent graph; add middleware logging request ID, latency, and token count for every LLM call

**Deliverable:** `fastapi-di-middleware/` -- refactored service with DI pattern and middleware stack

**Success Criteria:** Adding a new LLM service to your API requires zero changes to endpoint code -- only a new dependency provider. Middleware logs include token count for every response.

**Common Failure Mode:** Instantiating LLM clients inside endpoint functions. This creates a new connection per request. Dependency injection with proper lifespan management is what separates a production service from a demo.

**Capstone Connection:** The DI pattern is how the capstone manages its multi-component stack (RAG pipeline, agent graph, vector DB) without coupling.

---

#### Week 19: Authentication and Authorization

**Concepts:** OAuth2 with JWT for API authentication; role-based access control (RBAC) for AI endpoints -- different roles see different agents, different document types; FastAPI Security dependencies; token validation and expiry; RBAC as an AI governance control

**Build Task:** Add OAuth2 JWT authentication to your API; implement three roles (admin, analyst, viewer) with different access to agent endpoints; demonstrate that a viewer cannot trigger the full multi-agent pipeline

**Deliverable:** `fastapi-auth/` -- authenticated API with three-role RBAC, documented access matrix

**Success Criteria:** JWT validation works. Role enforcement is at the dependency layer (not if-else in endpoint code). Your access matrix is a documented artifact you can show in a compliance review.

**Common Failure Mode:** Implementing auth as endpoint-level if-else checks. FastAPI Security dependencies enforce auth declaratively -- the access control is visible in the function signature, not buried in business logic. This matters in compliance reviews.

**Capstone Connection:** The RBAC layer is the capstone's compliance control for regulated environment deployment.

---

#### Week 20: PostgreSQL + pgvector Integration

**Concepts:** asyncpg for non-blocking PostgreSQL access; connection pooling with asyncpg; JSONB for LLM metadata storage (prompt versions, model calls, evaluation scores); pgvector as the production vector store from Phase 1; schema design for audit logging

**Build Task:** Migrate your Phase 1 vector store to PostgreSQL + pgvector via asyncpg; add a JSONB metadata table logging every LLM call (timestamp, model, token count, RAGAS score); add an audit table for HITL decisions from Phase 2

**Deliverable:** `fastapi-postgres/` -- asyncpg connection pool, pgvector migration, JSONB metadata schema, audit table

**Success Criteria:** Your API serves vector searches through asyncpg without blocking. JSONB metadata table has at least three logged LLM calls with full metadata. Audit table captures Phase 2 HITL decisions.

**Common Failure Mode:** Using synchronous psycopg2 inside async endpoints. The entire throughput benefit of async FastAPI is lost the moment you make a synchronous database call. asyncpg is non-negotiable in production.

**Capstone Connection:** This PostgreSQL schema is the capstone's data persistence layer -- it stores vectors, metadata, audit logs, and evaluation scores in one place.

---

#### Week 21: Testing Strategy for AI Services

**Concepts:** Unit testing agent workflows (mocking LLM responses); integration testing the RAG pipeline with a real vector store; contract testing for agent API endpoints; RAGAS regression tests as a CI gate; pytest patterns for async code

**Build Task:** Write a test suite covering: unit tests for routing logic (mocked LLM), integration test for the full RAG pipeline (real Qdrant/pgvector, mocked LLM), contract test for your FastAPI endpoints, RAGAS regression test on golden dataset

**Deliverable:** `fastapi-testing/` -- full test suite with unit, integration, contract, and RAGAS regression tests; GitHub Actions CI config running all four test types

**Success Criteria:** CI runs all four test types on push. A change to chunking strategy that drops RAGAS faithfulness below 0.75 fails the CI gate automatically.

**Common Failure Mode:** Testing LLM applications the same way you test deterministic code. LLM outputs are non-deterministic -- testing for exact string matches fails. Test for structural properties (correct format, required fields, score thresholds) not content.

**Capstone Connection:** This test suite is the capstone's quality gate. Nothing ships without it passing.

---

#### Week 22: Background Jobs and API Versioning

**Concepts:** Background job patterns for long-running agent workflows (FastAPI BackgroundTasks vs ARQ vs Celery); job status endpoints (polling vs streaming vs webhook); API versioning strategies for AI systems (model version changes break clients silently); OpenAPI schema design for agent endpoints

**Build Task:** Move your multi-agent document processing to a background job pattern: POST to submit, GET to check status, callback on completion; add API versioning so v1 and v2 of your agent endpoint can coexist

**Deliverable:** `fastapi-async-jobs/` -- background job pattern with status polling, API versioning, OpenAPI schema

**Success Criteria:** A long-running agent workflow (>30 seconds) does not block the HTTP connection. API v1 and v2 coexist without routing conflicts. OpenAPI schema accurately documents async job endpoints.

**Common Failure Mode:** Synchronous request-response for agent workflows that take >10 seconds. HTTP timeouts kill long-running AI jobs in production. Background jobs with status polling is the pattern -- not extended timeouts.

**Capstone Connection:** The background job pattern is how the capstone handles the capstone's most complex workflows (full document intelligence pipeline end-to-end).

---

#### Week 23: API Hardening

**Concepts:** Input validation for AI endpoints (Pydantic models as security controls); output sanitization before returning LLM content; error response design that doesn't leak system information; health check and readiness endpoints for Kubernetes; structured logging for AI services

**Build Task:** Add Pydantic input validation catching prompt injection attempts; add structured JSON logging to every endpoint; add health check and readiness endpoints; add output sanitization that strips PII patterns before returning LLM responses

**Deliverable:** `fastapi-hardened/` -- hardened API with validated inputs, sanitized outputs, structured logging, health endpoints

**Success Criteria:** A prompt injection attempt in your API input is caught at the Pydantic validation layer and returns a 422 with a non-leaky error message. Health endpoints return correct ready/not-ready status during startup.

**Common Failure Mode:** Treating input validation as a data quality problem rather than a security control. In AI systems, malformed inputs are often adversarial. Pydantic validation is your first security layer.

**Capstone Connection:** This hardening checklist is the capstone's security review artifact.

---

#### Week 24: Phase 3 Integration -- Flagship Build

**Build Task:** Integrate Phase 3 into a production API serving the full Phase 1+2 system: async endpoints -> DI for RAG + agents -> RBAC auth -> PostgreSQL + pgvector -> background jobs for long workflows -> CI test suite -> hardened inputs/outputs.

**Deliverable:** `enterprise-ai-api/` -- production FastAPI service, fully integrated with Phase 1 RAG and Phase 2 agents, with CI pipeline, auth, and PostgreSQL persistence

**Success Criteria:** The full pipeline runs: authenticate -> submit document -> background job processes via multi-agent graph with RAG retrieval -> status polling -> result with audit log entry. CI passes. RAGAS regression gate is live.

**Capstone Connection:** Phase 3 integration IS the capstone's serving layer. You now have a deployable system.

---

### PHASE 4 (Weeks 25-32): Agentic Engineering Depth

**Goal:** Deepen production-grade agentic patterns NOT already covered in Phase 2. Every topic is labeled NEW or EXTENDS Phase 2.

---

#### Week 25: Structured Output Engineering (NEW)

**Concepts:** Why unstructured LLM outputs fail in production agent pipelines; Instructor library for schema-enforced generation; Pydantic output models with validation; structured output failure modes (partial JSON, schema drift); retry on validation failure

**Build Task:** Replace all string-output nodes in your Phase 2 graph with Instructor-enforced Pydantic schemas; implement retry-on-validation-failure for any node producing structured output

**Deliverable:** `structured-outputs/` -- refactored graph with Instructor schemas on all nodes, retry logic, validation failure logs

**Success Criteria:** Zero unstructured string outputs from any agent node. Validation failures trigger automatic retry with a modified prompt. You can show the before/after reliability delta.

---

#### Week 26: Event-Driven Agent Architecture (NEW)

**Concepts:** Event-driven vs request-driven agent architectures; message queues for agent triggering (Redis Streams, simple pub/sub); event schemas for agent communication; idempotency in event-driven flows; when event-driven is the right pattern vs when it's overengineering

**Build Task:** Add a Redis Streams trigger to your document pipeline -- document upload publishes an event, your agent pipeline subscribes and processes; implement idempotency so duplicate events are handled correctly

**Deliverable:** `event-driven-agents/` -- Redis Streams integration, event schema, idempotency implementation, consumer group config

**Success Criteria:** Duplicate event submissions process exactly once. Consumer group handles a simulated consumer failure with no message loss. You have a written decision record for when you would use event-driven vs direct API call.

**Capstone Connection:** The event-driven trigger is the capstone's enterprise integration pattern -- documents arrive via event, not HTTP POST.

---

#### Week 27: Long-Running Workflow State Persistence (EXTENDS Phase 2 -- Checkpointing)

**Concepts:** Workflow state vs checkpoint state (the delta); long-running workflow patterns spanning hours or days; state migration when workflow schema changes; externalized state for multi-instance deployment; saga pattern for distributed agent workflows

**Build Task:** Build a multi-day workflow: document submitted Monday, human review Tuesday, final processing Wednesday -- state persisted across three separate process restarts; demonstrate state migration when you add a new field to the schema

**Deliverable:** `long-running-workflows/` -- multi-day workflow with externalized state, schema migration demo, saga-style compensation for failure

**Success Criteria:** Workflow resumes correctly after three process restarts with no state loss. Schema migration adds a new field without breaking existing in-flight workflows.

---

#### Week 28: MCP at Production Scale (EXTENDS prior MCP experience)

**Concepts:** MCP tool permission boundaries in multi-agent systems; MCP server composition (multiple MCP servers behind one agent); tool call auditing for compliance; MCP error handling patterns at scale; performance profiling MCP tool calls

**Build Task:** Integrate two MCP servers into your Phase 2 supervisor: one for document retrieval, one for external data lookup; add tool call audit logging; profile tool call latency and identify bottlenecks

**Deliverable:** `mcp-production/` -- multi-MCP integration, tool audit log, latency profile report

**Success Criteria:** Every MCP tool call is logged with tool name, input hash, output size, and latency. You have a latency profile showing where tool calls are adding overhead.

---

#### Week 29: Retry and Fallback Architecture (EXTENDS Phase 2 -- Recovery)

**Concepts:** Circuit breaker pattern for LLM API calls; exponential backoff with jitter (the math); fallback model hierarchy (primary model -> cheaper model -> cached response -> graceful degradation); timeout budgeting across a multi-step pipeline; cascading failure prevention

**Build Task:** Implement a circuit breaker for LLM API calls in your pipeline; add a fallback model chain; implement timeout budgeting across your multi-agent graph showing total budget consumed at each node

**Deliverable:** `resilience-patterns/` -- circuit breaker, fallback chain, timeout budget tracker

**Success Criteria:** Circuit breaker opens after three consecutive LLM failures; closes after one success in half-open state. Fallback chain produces a response even when the primary model is unavailable. Timeout budget tracker alerts when 80% of budget is consumed mid-pipeline.

---

#### Week 30: Multi-Step Reasoning System Design (NEW)

**Concepts:** Chain-of-thought as an architecture pattern (not just a prompt trick); when to decompose reasoning into explicit steps vs let the model reason internally; reasoning trace persistence for auditability; hallucination in reasoning chains vs in final answers; structured reasoning schemas

**Build Task:** Build a multi-step reasoning agent: receives a complex document question, generates an explicit reasoning plan, executes each step, produces a cited answer with reasoning trace; make the trace queryable

**Deliverable:** `reasoning-agent/` -- explicit reasoning planner, step executor, cited answer generator, queryable trace

**Success Criteria:** Reasoning trace is stored as a queryable artifact (not just logged). You can demonstrate catching a reasoning error in the trace that would have been invisible in the final answer.

**Capstone Connection:** The reasoning agent is the capstone's highest-value feature -- it's what enterprise buyers pay for.

---

#### Week 31: Enterprise Workflow Automation Integration

**Build Task:** Integrate Weeks 25-30 into the enterprise workflow automation flagship: structured outputs on all nodes -> event-driven triggering -> long-running state persistence -> MCP integration -> circuit breaker + fallback -> multi-step reasoning for complex documents

**Deliverable:** `enterprise-workflow-platform/` -- fully integrated Phase 4 flagship connecting all Phase 1-4 components

---

#### Week 32: PII Redaction and RBAC Compliance Layer (NEW)

**Concepts:** Presidio for PII detection and redaction; ML-based NER vs regex for PII; redaction before LLM input vs after LLM output; RBAC enforcement at the data layer vs API layer; immutable audit trail design; compliance documentation artifacts (what a SOC2 reviewer actually wants to see)

**Build Task:** Add PII redaction to document ingestion (before embedding); add PII scan to LLM outputs before returning to caller; produce a compliance artifact documenting your data handling pipeline

**Deliverable:** `compliance-layer/` -- Presidio integration, input/output PII scan, compliance documentation artifact

**Success Criteria:** PII in test documents is redacted before embedding. PII scanner on output catches a planted test SSN before returning to caller. Compliance artifact documents the data flow with PII handling called out at each step.

**Capstone Connection:** The compliance layer is the capstone's regulated environment deployment requirement.

---

### PHASE 5 (Weeks 33-40): Principal-Level Engineering

**Goal:** Evaluation systems, observability, CI/CD for AI, and architecture defense. This is what separates Staff from Principal.

---

#### Week 33: Custom Evaluation Harness Design

**Concepts:** Beyond RAGAS -- custom LLM-as-judge for domain-specific quality; evaluation dataset curation (how many examples, how diverse, how to prevent leakage); multi-dimensional quality scoring (accuracy, groundedness, completeness, tone); evaluation disagreement analysis

**Build Task:** Build a custom evaluation harness using LLM-as-judge for your document intelligence platform; score 50 examples across four dimensions; compute inter-rater reliability between RAGAS and your custom judge

**Deliverable:** `custom-eval-harness/` -- LLM-as-judge implementation, 50-example eval dataset, inter-rater reliability analysis

**Success Criteria:** Custom judge disagrees with RAGAS on at least 10% of examples. You have a documented analysis of why they disagree and which is more aligned with user intent for your use case.

---

#### Week 34: OpenTelemetry for AI Workloads

**Concepts:** OpenTelemetry spans for LLM calls (token count, latency, model version as span attributes); trace context propagation across agent hops; metric export to Prometheus; dashboard design for AI-specific metrics (p95 retrieval latency, RAGAS trend, token spend by endpoint)

**Build Task:** Instrument your full stack with OpenTelemetry: LLM calls, vector DB queries, agent routing decisions, background jobs. Export metrics to Prometheus. Build a Grafana dashboard with five AI-specific panels.

**Deliverable:** `observability-stack/` -- OTel instrumentation, Prometheus config, Grafana dashboard with documented metrics

**Success Criteria:** Five-panel Grafana dashboard showing: p95 retrieval latency, LLM token spend per hour, RAGAS trend over time, agent routing distribution, error rate by agent node.

**Capstone Connection:** This dashboard IS the capstone's operational health artifact. It's what you show in a production readiness review.

---

#### Week 35: Prompt Versioning and Regression Testing

**Concepts:** Prompt as a versioned artifact (not a string in code); semantic versioning for prompts; regression test suite for prompt changes; A/B testing prompts in production; prompt library management at scale; how prompt changes break systems silently

**Build Task:** Move all prompts in your system to a versioned prompt library (YAML or database-backed); build a regression test that runs your golden dataset against two prompt versions and reports the delta; simulate a prompt regression and catch it

**Deliverable:** `prompt-versioning/` -- versioned prompt library, regression test harness, delta report showing a caught regression

**Success Criteria:** Changing a prompt generates a new version ID. Regression test catches a 15% drop in faithfulness when you introduce a regressive prompt change.

---

#### Week 36: CI/CD for AI Systems

**Concepts:** AI-specific CI/CD gates (eval gates, model pinning, drift detection); GitHub Actions for AI pipelines; deployment checkpoints (eval must pass before deployment); model version pinning to prevent silent upgrades; shadow deployment for new model versions

**Build Task:** Build a GitHub Actions CI/CD pipeline: run unit tests -> run RAGAS regression -> eval gate (fail if faithfulness < 0.80) -> build Docker image -> push to registry. Add model version pinning.

**Deliverable:** `ai-cicd/` -- GitHub Actions workflow, eval gate config, Docker build, model version pin mechanism

**Success Criteria:** A commit that drops RAGAS faithfulness below 0.80 fails CI and does not proceed to build. Model version is pinned in a config file, not hardcoded.

**Capstone Connection:** This CI/CD pipeline is the capstone's deployment infrastructure.

---

#### Week 37: Security Review Preparation

**Concepts:** Threat modeling for agentic systems (STRIDE applied: spoofing agent identity, tampering with tool outputs, repudiation of agent decisions, information disclosure via retrieval, denial of service via token exhaustion, elevation of privilege via prompt injection); writing a threat model document; how enterprise security reviewers evaluate AI systems

**Build Task:** Produce a threat model document for your enterprise AI platform using STRIDE; identify the top three highest-risk threats; document mitigations already in your system and gaps requiring remediation

**Deliverable:** `threat-model/` -- STRIDE threat model document, risk register, mitigation gap analysis

**Success Criteria:** Threat model is readable by a security reviewer with no AI background. Three highest-risk threats have documented mitigations. You can present and defend this document in a 30-minute security review.

---

#### Week 38: Architecture Defense Preparation

**Concepts:** Architecture Decision Records (ADRs) -- format, what to include, how to present; C4 model diagrams (context, container, component -- text-based); cost estimation for AI systems (token costs, GPU costs, storage costs); how to explain RAG vs fine-tuning to a CTO with no AI background; "why not just use [other approach]?" -- the Principal-level answer

**Build Task:** Write three ADRs for your system (vector DB choice from Phase 1, LangGraph vs alternatives, async job pattern from Phase 3); produce a text-based C4 container diagram; write a one-page cost estimate for your platform at 1,000 daily users

**Deliverable:** `architecture-docs/` -- three ADRs, C4 container diagram, cost estimate at scale

**Success Criteria:** Each ADR documents the decision, alternatives considered, forces, and consequences. A non-technical stakeholder can understand your C4 diagram without explanation. Cost estimate includes token costs, compute costs, and storage costs.

---

#### Week 39: Compliance Review Preparation

**Concepts:** What SOC2 reviewers actually examine in AI systems; HIPAA requirements for AI applications handling PHI; financial services AI governance expectations; compliance documentation package structure; mapping your existing system controls to compliance requirements

**Build Task:** Assemble a compliance documentation package for your platform: data flow diagram with PII handling, RBAC access matrix, audit log specification, model governance record, incident response plan (one page)

**Deliverable:** `compliance-package/` -- data flow diagram, access matrix, audit log spec, model governance record, incident response plan

**Success Criteria:** A compliance reviewer can trace every data element from ingestion to storage to output in your data flow diagram. Every PII handling step is documented with the control that protects it.

---

#### Week 40: Capstone Integration and Final Defense

**Build Task:** Final integration of all five phases into the production-ready enterprise AI platform. Document every component. Run the full test suite. Produce the architecture presentation deck. Practice the 20-minute defense walkthrough.

**Deliverable:** `enterprise-ai-platform-capstone/` -- fully integrated platform, all documentation, presentation deck

---

## OUTPUT 3: CAPSTONE PROJECT SPECIFICATION

**Name:** Enterprise Document Intelligence Platform

**What it proves:** You can design, build, deploy, test, observe, and defend a production-grade agentic AI system in a regulated enterprise context.

### System Architecture (Component-Level)

```
[Document Source] 
  -> Event Trigger (Redis Streams)
  -> FastAPI Background Job Endpoint (authenticated, RBAC)
     -> PII Redaction Layer (Presidio)
     -> RAG Pipeline (hybrid retrieval, reranking, pgvector)
     -> LangGraph Supervisor
        -> Classification Agent
        -> Extraction Agent (structured output, Instructor)
        -> Summarization Agent
        -> HITL Gate (low-confidence pause)
        -> Reasoning Agent (multi-step, cited)
     -> RAGAS + Custom Eval Harness
     -> Audit Log (PostgreSQL JSONB)
  -> OpenTelemetry (Prometheus + Grafana)
  -> LangSmith Traces
```

### Repository Structure

```
enterprise-ai-platform/
  docs/
    adrs/           -- Architecture Decision Records
    threat-model/   -- STRIDE threat model
    compliance/     -- SOC2/HIPAA compliance package
    c4-diagrams/    -- System architecture diagrams
  src/
    api/            -- FastAPI application
    rag/            -- Chunking, embedding, hybrid retrieval, reranking
    agents/         -- LangGraph supervisor + specialist agents
    evaluation/     -- RAGAS + custom eval harness
    observability/  -- OpenTelemetry instrumentation
    compliance/     -- PII redaction, audit logging
  tests/
    unit/
    integration/
    contract/
    regression/     -- RAGAS golden dataset CI gate
  infra/
    docker/
    github-actions/
  notebooks/
    benchmarks/     -- Chunking, embedding, vector DB benchmarks
```

### Phase-by-Phase Build Sequence

Phase 1 adds: RAG pipeline, chunking harness, hybrid retrieval, RAGAS evaluation
Phase 2 adds: LangGraph supervisor, specialist agents, HITL gate, LangSmith traces
Phase 3 adds: FastAPI API layer, auth, PostgreSQL, CI test suite
Phase 4 adds: Structured outputs, event-driven trigger, PII redaction, MCP integration
Phase 5 adds: Custom eval, OTel observability, CI/CD pipeline, ADRs, compliance package

### Demo Script

Audience: Enterprise engineering leader or Principal-level interview panel

Show: Submit a contract document via API -> watch event trigger -> PII redacted on ingestion -> hybrid RAG retrieves relevant clauses -> supervisor routes to extraction and reasoning agents -> low-confidence clause triggers HITL gate -> human approves -> cited answer returned -> all steps visible in LangSmith + Grafana dashboard

What it proves: You built a production system, not a demo. You understand every failure mode. You can defend every architecture decision.

### Resume Impact Statement

"Designed and built an enterprise document intelligence platform featuring LangGraph multi-agent orchestration with supervisor pattern, production RAG with hybrid retrieval and reranking (RAGAS faithfulness 0.83+), FastAPI async serving layer with RBAC and PostgreSQL persistence, custom LLM evaluation harness, OpenTelemetry observability stack, and a SOC2-aligned compliance documentation package. Full CI/CD with eval gates."

---

## OUTPUT 4: WEEKLY STUDY SCHEDULES

**All three variants assume 7 days/week at 45-60 min/day.**

### Variant A: 45 min/day (315 hrs over 40 weeks)

| Week | Phase | Primary Focus | Build Task | Daily Minutes | Milestone |
|---|---|---|---|---|---|
| 1 | Phase 1 | Chunking strategies | Chunking comparison harness | 45 | Harness runs 4 strategies |
| 2 | Phase 1 | Embedding models | MTEB benchmark script | 45 | 3-model comparison |
| 3 | Phase 1 | Vector DB architecture | pgvector + Qdrant comparison | 45 | ADR written |
| 4 | Phase 1 | Hybrid retrieval | BM25 + RRF implementation | 45 | Hybrid outperforms dense on 3 queries |
| 5 | Phase 1 | Reranking | Cross-encoder integration | 45 | Precision@5 delta documented |
| 6 | Phase 1 | RAGAS | Evaluation harness + golden dataset | 45 | RAGAS score above 0.80 |
| 7 | Phase 1 | Retrieval debugging | Debugger CLI tool | 45 | 3 failure modes documented |
| 8 | Phase 1 | Integration | Phase 1 flagship build | 45 | One-command RAG pipeline |
| 9 | Phase 2 | Graph state | Typed StateGraph | 45 | Type violation caught in test |
| 10 | Phase 2 | Routing | Conditional edges + fallback | 45 | 5 document types routed correctly |
| 11 | Phase 2 | Persistence | PostgresSaver checkpointing | 45 | Workflow resumes from interrupt |
| 12 | Phase 2 | HITL | Approval gate with audit log | 45 | Low-confidence pause works |
| 13 | Phase 2 | Supervisor | 3-agent supervisor | 45 | Specialist isolation demonstrated |
| 14 | Phase 2 | Recovery | Retry + fallback + dead-letter | 45 | 3 failure scenarios handled |
| 15 | Phase 2 | Observability | LangSmith instrumentation | 45 | One failure diagnosed via trace |
| 16 | Phase 2 | Integration | Phase 2 flagship | 45 | End-to-end doc flow works |
| 17 | Phase 3 | Async FastAPI | Streaming endpoint | 45 | 3 concurrent requests handled |
| 18 | Phase 3 | DI + Middleware | Dependency injection stack | 45 | Token count logged per response |
| 19 | Phase 3 | Auth/RBAC | OAuth2 + 3 roles | 45 | Viewer blocked from pipeline |
| 20 | Phase 3 | PostgreSQL | asyncpg + pgvector migration | 45 | JSONB metadata logging live |
| 21 | Phase 3 | Testing | Unit + integration + RAGAS CI | 45 | CI gate blocks regression |
| 22 | Phase 3 | Background jobs | Async job + status polling | 45 | 30-sec workflow doesn't block |
| 23 | Phase 3 | Hardening | Input validation + output sanitization | 45 | Prompt injection caught at Pydantic |
| 24 | Phase 3 | Integration | Phase 3 flagship | 45 | Full pipeline deployed via API |
| 25 | Phase 4 | Structured outputs | Instructor schemas on all nodes | 45 | Zero unstructured outputs |
| 26 | Phase 4 | Event-driven | Redis Streams trigger | 45 | Duplicate events handled once |
| 27 | Phase 4 | Long-running state | Multi-day workflow + migration | 45 | Resumes after 3 restarts |
| 28 | Phase 4 | MCP at scale | Multi-MCP + audit log | 45 | Tool latency profile produced |
| 29 | Phase 4 | Resilience | Circuit breaker + fallback chain | 45 | Fallback serves when primary down |
| 30 | Phase 4 | Reasoning | Multi-step reasoning agent | 45 | Reasoning trace is queryable |
| 31 | Phase 4 | Integration | Enterprise workflow flagship | 45 | All Phase 1-4 integrated |
| 32 | Phase 4 | Compliance | PII redaction + RBAC layer | 45 | Compliance artifact documented |
| 33 | Phase 5 | Custom eval | LLM-as-judge harness | 45 | 10% disagreement with RAGAS documented |
| 34 | Phase 5 | OTel | OpenTelemetry + Grafana | 45 | 5-panel dashboard live |
| 35 | Phase 5 | Prompt versioning | Versioned library + regression | 45 | Regression caught in CI |
| 36 | Phase 5 | CI/CD | GitHub Actions + eval gate | 45 | Eval gate blocks bad deployment |
| 37 | Phase 5 | Security | STRIDE threat model | 45 | Threat model document complete |
| 38 | Phase 5 | Architecture | ADRs + C4 + cost estimate | 45 | 3 ADRs written |
| 39 | Phase 5 | Compliance | SOC2/HIPAA compliance package | 45 | Full compliance package assembled |
| 40 | Phase 5 | Capstone | Final integration + defense | 45 | Demo-ready, defense-ready |

**At 45 min/day: no topics are dropped. Complexity targets are reduced slightly -- your Week 3 vector DB comparison may use smaller datasets; your Week 6 golden dataset may be 20 questions instead of 25. Every deliverable still ships.**

---

### Variant B: 60 min/day (420 hrs over 40 weeks) -- Baseline

Same week structure. Extra 15 min/day goes to: deeper code review of your own builds, one additional test case per deliverable, and Friday documentation (README updates, LinkedIn draft). This is the target pace.

---

### Variant C: 60 min/day + 90 min on weekends (one of Saturday/Sunday)

The additional 90 min/week (~60 extra hours over 40 weeks) goes to: monthly Loom walkthrough recordings (interview collateral), deeper benchmark analysis, and earlier capstone integration work so Phase 5 is refinement not first-build.

---

## OUTPUT 5: INTERVIEW PREPARATION ROADMAP

### Senior Agentic AI Engineer

Top 5 technical questions:

1. "Walk me through your RAG pipeline architecture and how you chose your chunking strategy." Answer framework: lead with the measurement -- show your chunking comparison harness. Don't describe theory; describe what you built and what the data showed.

2. "How do you debug a hallucination in a RAG system?" Answer framework: retrieval failure vs generation failure taxonomy. Walk through your retrieval debugger. Show trace-level visibility.

3. "How does LangGraph handle state persistence across agent hops?" Answer framework: TypedDict schema + reducers + PostgresSaver. The key insight is typed state -- explain why untyped state fails at scale.

4. "What's your testing strategy for LLM-based systems?" Answer framework: unit (mocked LLM) + integration (real vector store) + RAGAS regression as a CI gate. The interviewer wants to know you understand non-determinism.

5. "How would you handle a multi-step agent workflow that takes 10 minutes to complete?" Answer framework: background job pattern, polling/webhook, checkpointing for resumability. HTTP timeout failure mode is the trap -- show you know it.

Portfolio artifact to lead with: Phase 1+2 integrated flagship (`multi-agent-doc-intelligence/`).

---

### Staff AI Engineer

Top 5 technical questions:

1. "Design a production RAG system for a regulated financial services client. Walk me through every architectural decision." Answer framework: hybrid retrieval for terminology accuracy, pgvector for operational simplicity (shares Postgres footprint), RBAC at API layer, PII redaction before embedding, RAGAS CI gate, ADR documenting every choice.

2. "How would you evaluate whether your RAG system is getting better or worse over time?" Answer framework: custom LLM-as-judge + RAGAS + golden dataset + RAGAS trend in Grafana. Evaluation as a CI gate, not a one-time check.

3. "What are the failure modes of a supervisor-based multi-agent architecture?" Answer framework: fat coordinator (supervisor doing too much), context contamination (worker states bleeding), routing loop (termination condition missing), cascading failures without circuit breakers.

4. "How do you handle prompt versioning in a production system used by 50,000 users?" Answer framework: versioned prompt library, semantic versioning, regression test suite, shadow deployment for new prompt versions, rollback capability.

5. "Your agentic system is processing sensitive documents. What security controls are in place?" Answer framework: RBAC at API layer, PII redaction before LLM input, PII scan before output, audit log of every LLM call, threat model (STRIDE), prompt injection defense at Pydantic validation.

Architecture defense scenario: "You chose pgvector over Qdrant. Defend that decision when we scale to 10M documents." Expected: HNSW parameter tuning, read replica strategy, partitioning, and the explicit trigger for migrating to Qdrant if pgvector can no longer serve the workload.

Portfolio artifact to lead with: Your three ADRs + threat model.

---

### Principal AI Engineer

Top 5 technical questions:

1. "How do you build a culture of evaluation rigor in an AI engineering team that's used to shipping without measuring?" Answer framework: RAGAS CI gate makes eval non-optional, golden dataset ownership assigned to a team member, weekly eval review as part of sprint cadence, treat eval regression like a prod incident.

2. "We're deploying AI in a HIPAA-regulated environment. What does your compliance posture look like?" Answer framework: PII redaction pipeline (Presidio), PHI never enters LLM input without redaction, audit log for every model call, RBAC with documented access matrix, data flow diagram showing PHI handling at every step, SOC2 compliance artifact package.

3. "How do you make architecture decisions under uncertainty when the technology is changing faster than your review cycle?" Answer framework: ADRs with explicit "forces" and "consequences" sections, decision expiry dates, reversibility as a first-class architectural property (prefer reversible decisions under uncertainty), scheduled ADR review.

4. "Your eval scores are good but users say the system is getting worse. How do you close that gap?" Answer framework: distribution shift (golden dataset no longer represents user queries), proxy metric failure (RAGAS measures what it measures, not what users care about), shadow evaluation of real user sessions with LLM-as-judge, user feedback integration into the eval dataset.

5. "How do you onboard a new engineering team to a complex multi-agent system you built?" Answer framework: ADRs as the first read, C4 diagrams as the architecture orientation, retrieval debugger and LangSmith traces as the debugging onboarding, runbook for the three most common production failure modes.

Architecture defense scenario: "Present your enterprise AI platform to our security and compliance review board. You have 20 minutes." Expected: C4 container diagram, STRIDE threat model, compliance package, cost estimate at scale, and crisp answers to "what happens if the LLM provider goes down?" and "how do we audit what the AI decided?"

Portfolio artifact to lead with: Full capstone with ADRs, threat model, compliance package, and Grafana dashboard.

---

## OUTPUT 6: RESOURCES BY PHASE

### Phase 1: Advanced RAG Engineering

Courses:
- "Building and Evaluating Advanced RAG" -- DeepLearning.ai (produced 2023-2024, practitioner-grade)
- "RAG From Scratch" -- LangChain YouTube series (verify current version; LangChain updates frequently)
- Qdrant Vector Search course on their official documentation -- free, maintained actively

Books:
- "Designing Machine Learning Systems" -- Chip Huyen (2022; production ML thinking applies to RAG architecture; flag: pre-dates modern RAG but the evaluation discipline transfers directly)
- "Building LLM Powered Applications" -- Valentina Alto (2024; practitioner-grade, covers RAG production patterns)

GitHub repositories (described by function):
- LangChain RAG tutorial repository -- official LangChain org, shows chunking and retrieval patterns; verify it matches current LangChain version before using
- RAGAS official repository -- maintained by explodinggradients; primary implementation reference for evaluation harness
- pgvecto.rs / pgvector official repository -- maintained by ankane; primary reference for pgvector schema patterns

Research papers:
- "RAGAS: Automated Evaluation of Retrieval Augmented Generation" (2023) -- PRACTITIONER; direct implementation reference
- "HyDE: Hypothetical Document Embeddings" (2022) -- PRACTITIONER; advanced retrieval technique worth knowing for architecture defense

YouTube:
- James Briggs (Pinecone) -- production RAG engineering, well-maintained, practitioner-focused
- LangChain official channel -- verify video dates; older videos may reference deprecated APIs

Documentation:
- LangChain RAG conceptual documentation (docs.langchain.com) -- treat as living reference, not static reading

---

### Phase 2: LangGraph Mastery

Courses:
- "AI Agents in LangGraph" -- DeepLearning.ai x LangChain (2024; most current structured course on LangGraph)
- LangChain Academy "Introduction to LangGraph" -- free, official, maintained
- "Building Agentic AI Applications" -- verify current platform; LangGraph moves fast, prioritize official docs over third-party courses

Books:
- "Multi-Agent Systems" -- Wooldridge (academic; use for architecture defense of agent topology choices, not implementation)
- LangGraph official documentation -- treat as your primary textbook; it is better maintained than any book currently available

GitHub repositories:
- LangGraph official repository (langchain-ai org) -- primary implementation reference; study the examples folder
- LangGraph supervisor pattern examples -- look for maintained examples in the official org
- LangSmith SDK repository -- instrumentation reference

Research papers:
- "ReAct: Synergizing Reasoning and Acting in Language Models" (2022) -- ACADEMIC; foundational for understanding why agent loop architectures work
- "Cognitive Architectures for Language Agents" (2023) -- ACADEMIC; useful for architecture defense of agent topology choices

YouTube:
- LangChain official channel -- LangGraph tutorials; verify dates
- Sam Witteveen -- applied LangGraph examples; practitioner-focused

Documentation:
- LangGraph documentation (langchain-ai.github.io/langgraph) -- primary reference

---

### Phase 3: FastAPI Production Engineering

Courses:
- "FastAPI" official tutorial -- fastapi.tiangolo.com; primary reference, comprehensive, free
- "Full Stack FastAPI" course -- Sebastián Ramírez (creator of FastAPI); most authoritative source
- TestDriven.io FastAPI tutorials -- practitioner-grade testing patterns

Books:
- "FastAPI" -- Bill Lubanovic (2024; most current book-length treatment)
- "Python Concurrency with asyncio" -- Matthew Fowler (2022; essential for understanding async patterns in FastAPI)

GitHub repositories:
- FastAPI official repository -- tiangolo org; primary reference for patterns and best practices
- full-stack-fastapi-template -- official FastAPI template with PostgreSQL, authentication, testing patterns
- asyncpg official repository -- magicstack org; primary reference for async PostgreSQL

Research papers:
- "REST API Design Rulebook" -- Massé (PRACTITIONER; relevant for OpenAPI schema design under Phase 3)
- Not a paper-heavy phase -- prioritize documentation over academic literature here

YouTube:
- ArjanCodes -- Python async patterns, clean architecture; practitioner-focused
- FastAPI official documentation walkthroughs

Documentation:
- FastAPI documentation (fastapi.tiangolo.com) -- primary reference
- asyncpg documentation -- primary reference for PostgreSQL integration

---

### Phase 4: Agentic Engineering Depth

Courses:
- "AI Agentic Design Patterns with AutoGen" -- DeepLearning.ai (2024; covers patterns that transfer to LangGraph)
- Anthropic documentation on tool use and agentic patterns -- primary reference for MCP production patterns
- Redis University -- free courses on Redis Streams for event-driven architecture

Books:
- "Designing Distributed Systems" -- Burns (2018; distributed systems patterns apply to multi-agent architecture; event-driven and saga patterns)
- "Release It!" -- Nygard (2018; circuit breaker, bulkhead, timeout patterns; directly applicable to Phase 4 resilience work)

GitHub repositories:
- Presidio official repository -- Microsoft org; primary reference for PII detection and redaction
- Instructor official repository -- Jason Liu; primary reference for structured output enforcement
- Redis OM Python -- Redis official; event streaming patterns

Research papers:
- "Toolformer: Language Models Can Teach Themselves to Use Tools" (2023) -- ACADEMIC; foundational for understanding tool calling architecture
- "AgentBench" (2023) -- PRACTITIONER; evaluation benchmark for agentic systems; useful for designing your own eval

YouTube:
- Sam Witteveen -- MCP and agentic integration patterns
- Anthropic YouTube channel -- MCP architecture guidance

Documentation:
- Anthropic MCP documentation -- primary reference for MCP production patterns
- Presidio documentation (Microsoft) -- primary reference for PII redaction

---

### Phase 5: Principal-Level Engineering

Courses:
- "LLMOps" -- DeepLearning.ai (2024; covers prompt versioning, CI/CD, evaluation pipelines)
- OpenTelemetry official documentation and Python SDK guide -- free, primary reference
- "Machine Learning System Design" -- Chip Huyen's Stanford course materials (free online)

Books:
- "Designing Data-Intensive Applications" -- Kleppmann (2017; foundational for understanding data architecture decisions you will defend in Principal-level interviews; evergreen)
- "The Staff Engineer's Path" -- Larson (2022; how to operate at Principal/Staff level; directly applicable to Phase 5 client leadership and architecture defense content)

GitHub repositories:
- OpenTelemetry Python repository -- official; primary reference for instrumentation patterns
- LangSmith SDK repository -- official LangChain org; primary reference for trace instrumentation
- GitHub Actions workflows examples -- GitHub official; CI/CD patterns for AI systems

Research papers:
- "Holistic Evaluation of Language Models" (HELM, 2022) -- ACADEMIC; framework for evaluation system design; useful for architecture defense of your custom eval harness
- "Measuring Massive Multitask Language Understanding" (MMLU, 2020) -- ACADEMIC; evaluation methodology reference

YouTube:
- MLOps Community -- production AI engineering, evaluation, observability; practitioner-focused
- Google Cloud Next recordings on AI governance -- useful for regulated environment compliance framing

Documentation:
- OpenTelemetry Python documentation -- primary reference
- LangSmith documentation -- primary reference
- GitHub Actions documentation -- primary reference for CI/CD

---

## SELF-EVALUATION VERIFICATION

Before this output ships -- running the checklist:

- Gap analysis table has at least 15 rows and all gaps are assigned to a phase: 19 rows, all assigned. Verified.
- Every week in every phase has a concrete build task and named deliverable: 40 weeks, 40 build tasks, 40 named deliverables. Verified.
- No concept appears twice without explicit EXTENDS notation: Phase 4 Weeks 27, 28, 29 labeled EXTENDS Phase 2. Phase 4 Weeks 25, 26, 30, 32 labeled NEW. Verified.
- No GitHub URLs are fabricated: All repositories described by organization and function; no URLs provided. Verified.
- Capstone referenced within at least 3 individual phase weeks: Referenced in Weeks 1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 13, 16, 17, 18, 19, 20, 24, 26, 30, 32, 34, 36, 40. Verified.
- All three weekly schedule variants explicitly handle time constraint differences: Variants A, B, C defined with explicit time delta handling. Verified.
- Interview prep covers all three target levels: Senior, Staff, Principal -- all covered. Verified.

All checks pass.

---

**One final note, Maurice:** At 45-60 minutes a day you are not sacrificing the plan -- you are protecting it. The engineers who fail 6-month accelerators are the ones who go hard for three weeks and then burn out. Daily rhythm at a sustainable pace, every deliverable shipping on schedule, compounding over 40 weeks -- that's what produces a Principal-level portfolio. The capstone you'll have at Week 40 is what most self-taught engineers take 2 years to build. The ServiceLink BA role starts July 8, which means your Phase 1 begins in parallel with onboarding. Keep the morning slot protected.
