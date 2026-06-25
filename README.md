# Framework Architect Course
### Build Proprietary AI Diagnostic Frameworks That Compound in Value

![Version](https://img.shields.io/badge/version-0.1.0-8E44AD)
![Status](https://img.shields.io/badge/status-in_progress-9B59B6)
![License](https://img.shields.io/badge/license-DACR_v2.6-6C3483)
![Format](https://img.shields.io/badge/format-self_paced-A569BD)
![Audience](https://img.shields.io/badge/audience-jobseekers_%7C_fractional_execs-7D3C98)
![Modules](https://img.shields.io/badge/modules-1%20of%206-B266FF)

---

## Overview

This course teaches the difference between using AI tools and building AI-powered diagnostic systems. Where most courses stop at "how to prompt ChatGPT," this course goes further: how to design, structure, govern, version, and package a proprietary framework that becomes a durable professional asset.

Built for two audiences:

- **Seasoned job seekers** who need proof-of-work that demonstrates strategic systems thinking, not just tool fluency
- **Fractional executives and consultants** who need repeatable, licensable diagnostic frameworks that scale across engagements

---

## Principal Agentic AI Engineer Accelerator (6-Month Technical Track)

Running alongside the Framework Architect Course is a personalized 6-month engineering depth roadmap targeting Lead / Principal Agentic AI Engineer readiness.

This track was designed for a specific profile: 25+ years of enterprise technology experience, active AI framework and MCP builder, strong architecture and stakeholder leadership background -- with targeted gaps in production RAG engineering, LangGraph orchestration, FastAPI serving, and enterprise-grade observability and compliance.

**Daily time commitment:** 45-60 minutes, 7 days a week. No multi-hour sessions required. Sustainable cadence built for a full-time professional carrying a day job.

### What This Track Builds

The roadmap produces five flagship builds that assemble phase by phase into a single capstone:

| Phase | Weeks | Focus | Flagship Build |
|---|---|---|---|
| 1 | 1-8 | Advanced RAG Engineering | Production RAG platform with hybrid retrieval, reranking, and RAGAS evaluation harness |
| 2 | 9-16 | LangGraph Mastery | Multi-agent document intelligence platform with supervisor pattern, HITL gates, and LangSmith tracing |
| 3 | 17-24 | FastAPI Production Engineering | Enterprise API serving layer with async architecture, RBAC auth, PostgreSQL + pgvector, and CI test suite |
| 4 | 25-32 | Agentic Engineering Depth | Enterprise workflow automation platform with event-driven triggers, structured outputs, circuit breaker resilience, and PII redaction |
| 5 | 33-40 | Principal-Level Engineering | Full observability stack (OpenTelemetry + Grafana), CI/CD with eval gates, STRIDE threat model, ADRs, and SOC2-aligned compliance package |

### Capstone: Enterprise Document Intelligence Platform

The five phases converge into a single production-grade system:

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

**What the capstone proves:** You can design, build, deploy, test, observe, and defend a production-grade agentic AI system in a regulated enterprise context. Every component is portfolio-ready, interview-demonstrable, and client-pitch-capable.

**Resume impact statement:**
> "Designed and built an enterprise document intelligence platform featuring LangGraph multi-agent orchestration with supervisor pattern, production RAG with hybrid retrieval and reranking (RAGAS faithfulness 0.83+), FastAPI async serving layer with RBAC and PostgreSQL persistence, custom LLM evaluation harness, OpenTelemetry observability stack, and a SOC2-aligned compliance documentation package. Full CI/CD with eval gates."

### Technical Gap Analysis

The roadmap was calibrated to a 19-competency gap analysis. The five highest-priority gaps driving the phase sequence:

| Priority | Competency | Gap Level | Phase |
|---|---|---|---|
| 1 | Production RAG (chunking, hybrid retrieval, reranking) | HIGH | Phase 1 |
| 2 | RAGAS evaluation framework implementation | HIGH | Phase 1 |
| 3 | Vector DB architecture tuning (pgvector, Qdrant HNSW) | HIGH | Phase 1 |
| 4 | LangGraph state machines, checkpointing, conditional routing | HIGH | Phase 2 |
| 5 | LangGraph supervisor patterns, swarm architectures | HIGH | Phase 2 |

Full gap table available in `accelerator/gap-analysis.md`.

### Daily Study Rhythm

**Weekday (45 min):**
- 20 min: concept study -- read, annotate, summarize in your own words
- 25 min: code -- always building, never just reading

**Weekend:**
- Saturday (60 min): flagship build sprint -- major feature addition or integration
- Sunday (60 min): documentation -- LinkedIn post, README update, or Loom walkthrough note

**Monthly:** Record a 10-minute Loom walkthrough of the current flagship build. This becomes interview collateral and proof-of-work before you finish the roadmap.

### Interview Readiness Targets

The roadmap prepares for three interview levels:

- **Senior Agentic AI Engineer** -- RAG architecture defense, LangGraph state management, async job patterns, LLM testing strategy
- **Staff AI Engineer** -- Evaluation rigor, production failure mode taxonomy, supervisor architecture defense, prompt versioning at scale
- **Principal AI Engineer** -- Compliance posture under HIPAA/SOC2, architecture decisions under uncertainty, eval culture building, full capstone defense to a security and compliance review board

---

## Framework Architect Course Modules

### How This Course Works

Each module builds on the last. Start with Module 1, complete the deliverable at the end of each module, and move on. By the end of the course, you will have a versioned, documented framework with at least one worked example, ready to use as a portfolio artifact or a client-facing asset.

Templates are fillable scaffolds. Copy them, do not edit the originals, so you always have a clean version to return to.

---

## Repository Structure

```
framework-architect-course/
│
├── README.md                                   # This file
│
├── accelerator/                                # Principal Agentic AI Engineer Track
│   ├── gap-analysis.md                         # 19-competency technical gap table
│   ├── weekly-schedule.md                      # 40-week schedule (45/60/60+ min variants)
│   ├── capstone-spec.md                        # Enterprise Document Intelligence Platform spec
│   ├── interview-prep.md                       # Senior / Staff / Principal interview frameworks
│   └── resources-by-phase.md                  # Courses, books, repos, papers by phase
│
├── modules/
│   ├── module-1-framework-mindset.md           # From prompt user to framework architect
│   ├── module-2-diagnostic-architecture.md
│   ├── module-3-skill-layer.md
│   ├── module-4-proof-of-work.md
│   ├── module-5-licensing-packaging.md
│   └── module-6-multi-framework-orchestration.md
│
├── templates/
│   ├── framework-build-template.md             # Core scaffold, used starting Module 2
│   ├── skill-file-template.md                  # Module 3 deliverable
│   ├── proof-of-work-template.md               # Module 4 deliverable
│   └── licensing-attribution-template.md       # Module 5 deliverable
│
├── examples/
│   └── worked-framework-examples.md            # Anonymized real-world framework evolutions
│
└── resources/
    └── dashboard-visual-references.md          # Visual/dashboard reference patterns
```

---

## Module Status

| Module | Title | Status |
|--------|-------|--------|
| 1 | The Framework Mindset | ![Done](https://img.shields.io/badge/-complete-9370DB) |
| 2 | Diagnostic Architecture | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |
| 3 | Building the Skill Layer | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |
| 4 | Proof-of-Work Production | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |
| 5 | Licensing and Packaging | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |
| 6 | Multi-Framework Orchestration | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |

---

## Accelerator Track Status

| Phase | Focus | Weeks | Status |
|---|---|---|---|
| 1 | Advanced RAG Engineering | 1-8 | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |
| 2 | LangGraph Mastery | 9-16 | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |
| 3 | FastAPI Production Engineering | 17-24 | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |
| 4 | Agentic Engineering Depth | 25-32 | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |
| 5 | Principal-Level Engineering | 33-40 | ![Pending](https://img.shields.io/badge/-pending-D8BFD8) |

---

## Attribution

Course materials and frameworks referenced herein follow DACR License v2.6 (McDonald 2026) attribution standards where applicable. See `templates/licensing-attribution-template.md` (Module 5) for how to apply attribution to frameworks you build using this course.

The Principal Agentic AI Engineer Accelerator roadmap was produced using the Skill Enhancer v2.0 prompt framework (McDonald 2026), the Agentic AI Engineer Accelerator Skill (Epoch Frameworks LLC), and the AI Arbitrage methodology. All framework content, gap analysis structure, and evaluation harness design are original IP under DACR License v2.6.
