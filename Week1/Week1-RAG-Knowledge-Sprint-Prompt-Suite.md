# Week 1 RAG Knowledge Sprint — Prompt Suite
## Agentic AI Engineer Accelerator | ServiceLink Client Integrations Context
### Erwin Maurice McDonald | Epoch Frameworks LLC | DACR License v2.7

---

> **How to use this document:** Each session below is a self-contained prompt. Paste it
> directly into Claude at the start of that session. Each is calibrated for 45-60 minutes
> of focused work. Do not read ahead. Complete the session, then close it. The week
> compounds forward.

---

## Week 1 Architecture

The chunking harness you already built (semantic wins at 0.74 Precision@5) is your
**proof artifact** for Week 1. This week's learning is about understanding WHY that
number is true at a level that lets you defend it in a ServiceLink technical interview
and extend it into a production system. Five sessions. One decision per session.

```
SESSION 1  |  MON  45-60 min  |  Why Chunking Is the Whole Game
SESSION 2  |  TUE  45-60 min  |  Embedding Model Decision Framework
SESSION 3  |  WED  45-60 min  |  Hybrid Retrieval — BM25 + Vector Fusion
SESSION 4  |  THU  45-60 min  |  Re-ranking: From Retrieval Rank to Relevance Rank
SESSION 5  |  FRI  45-60 min  |  Evaluation as Engineering — RAGAS + Golden Dataset
```

---

---

## SESSION 1 — Monday
### "Why Chunking Is the Whole Game"
**Time block:** 45-60 minutes | **Decision at the end:** Which chunking strategy do I
recommend for ServiceLink's document corpus, and can I defend it to an engineering panel?

---

Paste this entire block into Claude to begin Session 1:

```
CONTEXT: I am Erwin Maurice McDonald, a Principal-level AI Architect preparing for a
Lead Agentic AI Engineer role at ServiceLink (title and mortgage document processing
company). I have already built a chunking comparison harness that produced the
following results on a corpus of 12 mortgage and title contract documents (~340 pages):

  Semantic chunking:       Precision@5 = 0.74  [WINNER]
  Hierarchical (1024/256): Precision@5 = 0.68
  Late chunking:           Precision@5 = 0.61
  Fixed 512 tokens:        Precision@5 = 0.42

I have 45-60 minutes this session. I need to move from "I ran the harness and semantic
won" to "I understand exactly WHY semantic won on this specific document type and can
predict when it would NOT win."

TEACH ME: Run me through a rigorous technical session on chunking strategy that covers:

1. THE MECHANICS: Explain precisely how each of the four strategies works at the
   token/embedding level. Don't summarize. Explain the actual computational difference.
   Walk through what happens to a sentence like "The mortgagor shall deliver title
   insurance no later than 30 days after the closing date" under each strategy.

2. THE FAILURE MODE ANALYSIS: For each strategy, give me the specific failure mode that
   produces bad retrieval. What does a bad chunk actually look like? Show me a concrete
   example from a mortgage/title document domain.

3. THE WHY SEMANTIC WON: Given that my corpus is dense legal contracts with long clause
   structures, build the technical argument for WHY semantic chunking outperforms fixed
   by 32 percentage points. What specifically is fixed-size chunking destroying?

4. THE WHEN SEMANTIC LOSES: Give me 3 concrete scenarios where fixed or hierarchical
   would beat semantic. I need to understand the decision boundaries, not just the
   winning case.

5. THE SERVICELINK EXTENSION: ServiceLink processes title commitments, closing
   disclosure forms, deed of trust documents, and lender instructions. Given those
   document types, what chunking strategy would I recommend at production scale and why?
   What factors would change my recommendation?

6. PRINCIPAL-LEVEL INTERVIEW SIMULATION: Give me the 3 hardest chunking questions an
   interviewer would ask someone claiming to be a Principal AI Engineer, and walk me
   through the model answer for each.

After you teach me, I want you to quiz me with 2 questions. I'll answer them. You
grade my answers and tell me what a Principal-level answer would add.

Format: Teach conversationally, not in bullet lists. Use code examples where they make
the concept clearer. ServiceLink mortgage/title domain examples throughout.
```

---

**Session 1 exit check — before you close this tab, answer these yourself:**
- Can I explain in one sentence why semantic chunking respects clause boundaries?
- Can I name the exact failure mode that causes fixed-512 to drop to 0.42 on legal docs?
- Do I know when I would NOT recommend semantic chunking?

---

---

## SESSION 2 — Tuesday
### "Embedding Model Decision Framework"
**Time block:** 45-60 minutes | **Decision at the end:** Which embedding model would I
select for a ServiceLink production RAG system, and how would I justify it to the CTO?

---

Paste this entire block into Claude to begin Session 2:

```
CONTEXT: I am Erwin Maurice McDonald, Lead/Principal Agentic AI Engineer candidate at
ServiceLink. Yesterday I completed a deep dive on chunking strategy and can defend why
semantic chunking won at Precision@5 = 0.74 on mortgage/title contract documents.

Today I need to understand embedding model selection at a level where I can make and
defend the production model choice for ServiceLink's RAG system. ServiceLink processes
regulated financial documents: title commitments, closing disclosures, deed of trust
instruments, lender instructions, ALTA policies. Precision is not optional. Errors in
document retrieval in this domain have legal and financial consequences.

I have 45-60 minutes. Run this session:

1. THE MTEB LEADERBOARD BRIEFING: Walk me through how to use the MTEB leaderboard
   (https://huggingface.co/spaces/mteb/leaderboard) to make a production decision.
   What filters matter? What does Retrieval task score actually mean vs overall score?
   Why should I NOT just pick the #1 model?

2. THE MODEL LANDSCAPE: Walk me through the models I need to know by name and what
   each one is actually good for:
   - text-embedding-3-large (OpenAI)
   - voyage-3 (Voyage AI)
   - bge-large-en-v1.5 (BAAI)
   - gte-Qwen2-7B-instruct
   - jina-embeddings-v3
   For each: what is it optimized for, what are the latency/cost tradeoffs, and when
   would a Principal engineer select or reject it?

3. THE SERVICELINK DECISION FRAMEWORK: Walk me through the 5 selection criteria I
   need to apply to ServiceLink's context specifically:
   a. Domain fit (legal/financial docs — does general-purpose embedding hold up?)
   b. Latency requirements (what is acceptable for a title officer workflow?)
   c. On-prem / air-gapped requirement (regulated financial data — does ServiceLink
      need self-hosted inference?)
   d. Cost at scale (ServiceLink processes millions of documents — per-token cost math)
   e. Context window alignment (does the embedding model window match my chunk size?)

4. THE EVALUATION BUILD: Teach me how to build a golden dataset to evaluate two
   candidate embedding models against my actual corpus. Walk me through:
   - How to create 50 query/relevant-doc pairs from mortgage docs
   - What Recall@5 and MRR@10 measure and why they matter
   - How to run a fair head-to-head evaluation
   Show me the Python code to run this comparison.

5. THE REGULATED ENVIRONMENT ANGLE: ServiceLink operates in a regulated financial
   environment. Walk me through the compliance considerations that affect embedding
   model selection — what does it mean if PII (SSNs, borrower names, property addresses)
   gets sent to a third-party embedding API? What is the architecture pattern to handle
   this?

6. INTERVIEW SIMULATION: Give me the 2 embedding model questions a Principal-level
   interviewer asks that separate people who read about embeddings from people who have
   made the production selection. Walk me through what the wrong answer sounds like and
   what the right answer adds.

Close the session with a quiz: 2 questions. I answer. You grade.

Format: Conversational deep dive. Code where it clarifies. ServiceLink domain examples.
No generic content — everything tied to regulated financial document retrieval.
```

---

**Session 2 exit check:**
- Can I explain why MTEB rank 1 is not automatically the production choice?
- Do I know what to do when ServiceLink's compliance team says "no third-party API for PII"?
- Can I sketch the Python code structure for a head-to-head embedding eval?

---

---

## SESSION 3 — Wednesday
### "Hybrid Retrieval — BM25 + Vector Fusion"
**Time block:** 45-60 minutes | **Decision at the end:** When I upgrade the ServiceLink
RAG system from pure vector to hybrid retrieval, can I implement and explain RRF fusion?

---

Paste this entire block into Claude to begin Session 3:

```
CONTEXT: I am Erwin Maurice McDonald, Lead/Principal Agentic AI Engineer candidate at
ServiceLink. I have working knowledge of chunking strategy and embedding model selection
from earlier this week. My chunking harness produces Precision@5 = 0.74 with semantic
chunking. I need to understand why that is NOT good enough for production and what
hybrid retrieval adds.

ServiceLink domain reminder: title commitments, deed of trust instruments, closing
disclosures, ALTA policies, lender instructions. These documents contain:
- Exact named entities: "Schedule B-II Exception 7", "CLTA Form 100.29", "Regulation X"
- Legal citations and cross-references
- Numeric identifiers: loan numbers, parcel numbers, APN codes
- Borrower/property PII

This is exactly the failure zone of pure vector search. I have 45-60 minutes.

1. THE FAILURE MODE: Start by showing me exactly what happens when a title officer
   queries "Schedule B-II Exception 7 coverage" against a pure vector index. Walk
   through the failure: why does semantic similarity fail on exact named entities,
   regulatory codes, and numeric identifiers? Make this concrete with a real example.

2. BM25 MECHANICS: Explain BM25 from first principles — not the formula summary, the
   actual mechanics. What is term frequency saturation? What is document length
   normalization? Why does BM25 handle "CLTA Form 100.29" better than a dense vector?
   Show me what the BM25 score calculation looks like for a mortgage document query.

3. RECIPROCAL RANK FUSION: Walk me through RRF step by step. Show me:
   - The formula and why k=60 is the default
   - A worked example: two ranked lists, the fusion math, the output
   - Show me the Python implementation I built in the harness and explain each line:

   def rrf_score(rank, k=60):
       return 1 / (k + rank)

   def reciprocal_rank_fusion(bm25_results, vector_results, k=60):
       scores = {}
       for rank, doc_id in enumerate(bm25_results):
           scores[doc_id] = scores.get(doc_id, 0) + rrf_score(rank, k)
       for rank, doc_id in enumerate(vector_results):
           scores[doc_id] = scores.get(doc_id, 0) + rrf_score(rank, k)
       return sorted(scores.items(), key=lambda x: x[1], reverse=True)

   Why does this formula specifically work better than a simple score average?

4. IMPLEMENTATION OPTIONS: Walk me through the three implementation paths for hybrid
   retrieval and when I'd choose each for ServiceLink:
   a. Elasticsearch/OpenSearch: built-in BM25 + kNN hybrid
   b. Qdrant: sparse + dense native
   c. pgvector + PostgreSQL tsvector: dense + full-text in one query
   Given ServiceLink likely runs on PostgreSQL (title/mortgage ops are Postgres-heavy),
   walk me through the pgvector + tsvector hybrid query pattern.

5. QUERY EXPANSION BONUS: Explain HyDE (Hypothetical Document Embeddings) and
   multi-query retrieval. Show me when these are worth the added LLM call cost and
   when they are overkill for a production system.

6. THE UPGRADE PATH: I need to upgrade my existing chunking harness to add hybrid
   retrieval. Walk me through what I add to chunker.py and evaluate.py. What does the
   new evaluate.py look like when it measures hybrid vs pure vector precision@5?

7. INTERVIEW SIMULATION: The "gotcha" question on hybrid retrieval at Principal level
   is not "what is RRF" — it's "when does hybrid make retrieval WORSE?" Give me that
   answer and walk me through it.

Quiz me at the end: 2 questions. Grade my answers.

Format: Conversational. Code examples. ServiceLink document domain throughout.
```

---

**Session 3 exit check:**
- Can I explain why "CLTA Form 100.29" is a vector search failure case?
- Can I implement RRF fusion from memory and explain the k=60 parameter?
- Do I know the PostgreSQL tsvector + pgvector hybrid query pattern?

---

---

## SESSION 4 — Thursday
### "Re-ranking: From Retrieval Rank to Relevance Rank"
**Time block:** 45-60 minutes | **Decision at the end:** Do I add a re-ranker to the
ServiceLink pipeline, which one, and how do I justify the latency/quality tradeoff?

---

Paste this entire block into Claude to begin Session 4:

```
CONTEXT: I am Erwin Maurice McDonald, Lead/Principal Agentic AI Engineer candidate at
ServiceLink. I now have solid working knowledge of chunking, embedding selection, and
hybrid BM25+vector retrieval with RRF fusion. My pipeline retrieves well. But retrieval
rank is not the same as relevance rank. I need to understand re-ranking as the final
quality gate before context enters the LLM.

ServiceLink context: A title officer asks "does this property have any outstanding
mechanic's liens?" My hybrid retrieval returns top-50 candidate chunks from deed of
trust instruments, ALTA policies, and Schedule B exception lists. Some are highly
relevant. Some are semantically adjacent but wrong. I am about to put top-5 into an
LLM context window that will generate a legal opinion. I cannot afford wrong chunks.

I have 45-60 minutes.

1. WHY RETRIEVAL RANK FAILS: Walk me through exactly why the top-5 from ANN (approximate
   nearest neighbor) vector search is NOT the same as the top-5 most relevant chunks.
   What is ANN doing that creates this gap? Use the ServiceLink mechanic's lien query
   as the running example.

2. CROSS-ENCODER MECHANICS: Explain how a cross-encoder re-ranker actually works at
   the architecture level. What is the difference between a bi-encoder (used for
   retrieval) and a cross-encoder (used for re-ranking) in terms of what computation
   happens? Why is cross-attention the key? Why can't I use this for retrieval directly?

3. THE THREE RE-RANKER OPTIONS:
   a. Cross-encoder (bge-reranker-v2-m3 or ms-marco-MiniLM):
      - How it works, latency, quality, when to use
      - Show me the Python implementation using sentence-transformers
   b. ColBERT via RAGatouille:
      - Token-level late interaction — what is different about this?
      - When does it outperform cross-encoder?
   c. LLM-as-reranker:
      - What is the prompt pattern?
      - When is the latency cost justified?
      - Why is this the right choice for low-volume, high-stakes legal queries?

4. THE PIPELINE PATTERN: Walk me through the full retrieval-to-rerank pipeline:
   Step 1: Retrieve top-50 via hybrid BM25+vector
   Step 2: Re-rank with cross-encoder -> top-5
   Step 3: Pass top-5 to LLM context window
   Show me the Python code skeleton for this full pipeline.

5. THE LATENCY/QUALITY TRADEOFF: ServiceLink title officers need answers in under
   3 seconds. A cross-encoder re-ranking 50 chunks adds latency. Walk me through:
   - Typical latency numbers for each re-ranker type
   - What latency optimizations are available (smaller model, batch size, caching)
   - How I would make the go/no-go decision for production
   - What a "re-rank only for specific query types" routing pattern looks like

6. THE REGULATED ENVIRONMENT QUESTION: The LLM-as-reranker sends chunks to an LLM
   API. If those chunks contain PII (borrower names, SSNs, property addresses), what
   is the architecture pattern to stay compliant? Walk me through the redact-then-rerank
   pattern.

7. INTERVIEW SIMULATION: Give me the Principal-level re-ranking question that separates
   people who have read about it from people who have shipped it. What does the wrong
   answer look like? What does the production-seasoned answer add?

Quiz: 2 questions at the end. I answer. You grade against Principal-level standard.

Format: Conversational. Code examples for the cross-encoder implementation. ServiceLink
mortgage/title domain throughout. No generic examples.
```

---

**Session 4 exit check:**
- Can I explain the bi-encoder vs cross-encoder architectural difference?
- Can I sketch the 50-to-5 pipeline from retrieval through re-rank?
- Do I know the latency tradeoff argument for the ServiceLink context window?

---

---

## SESSION 5 — Friday
### "Evaluation as Engineering — RAGAS + Golden Dataset"
**Time block:** 45-60 minutes | **Decision at the end:** I can build a production-grade
eval harness that turns my chunking harness into an engineering discipline with tracked
metrics. This is my Week 1 portfolio centerpiece.

---

Paste this entire block into Claude to begin Session 5:

```
CONTEXT: I am Erwin Maurice McDonald, Lead/Principal Agentic AI Engineer candidate at
ServiceLink. I have spent the week building from chunking mechanics through embedding
selection, hybrid retrieval, and re-ranking. My chunking harness (Precision@5 = 0.74)
is already a portfolio artifact. Today I turn it into a production-grade evaluation
system. This session is the capstone of Week 1.

The difference between "I built RAG" and "I engineered RAG" is evaluation. Anyone can
build a pipeline that returns chunks. A Principal engineer builds a system that PROVES
retrieval quality on their domain with tracked, reproducible metrics.

ServiceLink context: My golden dataset will be built from mortgage and title document
queries. The eval harness produces metrics that I can show to a hiring panel and say:
"Here is my pipeline. Here is the baseline. Here is what each architectural decision
changed. Here is the metric that matters for ServiceLink's domain."

I have 45-60 minutes.

1. THE RAGAS FRAMEWORK — MECHANICS: Walk me through the four RAGAS metrics at the
   implementation level, not the conceptual level:
   - Faithfulness: what is the LLM actually scoring? What prompt does it run?
   - Answer Relevancy: how is this different from faithfulness?
   - Context Precision: what failure does this catch that Precision@5 misses?
   - Context Recall: why is this the hardest metric to score?
   Show me what a failing score looks like for each metric in the ServiceLink domain.

2. THE GOLDEN DATASET BUILD: Walk me through building a 50-question golden dataset
   for ServiceLink mortgage/title documents:
   - What makes a good golden dataset question vs a bad one?
   - How do I generate 50 questions without manually writing them all?
   - What does the human review step actually look like?
   - What is the data schema: {question, ground_truth, contexts, answer}
   Show me the Python code to build and store this dataset.

3. THE EVAL HARNESS CODE: Walk me through what evaluate.py looks like after I upgrade
   it from Precision@5-only to a full RAGAS harness. Show me:
   - How to wire RAGAS into my existing evaluate.py structure
   - How to run evaluation across multiple pipeline configurations
     (semantic vs hierarchical, with vs without re-ranker, etc.)
   - How to store results so I can track changes over time
   - What the output looks like: a comparison table of configs vs metrics

4. THE LLM-AS-JUDGE PATTERN: RAGAS uses an LLM internally to score faithfulness.
   Walk me through what that means in practice:
   - What model does RAGAS use by default?
   - How do I plug in a different judge model?
   - What is the cost of running RAGAS against a 50-question golden dataset?
   - What does the RAGAS score look like for a good pipeline vs a bad one?
     Give me realistic numbers for the ServiceLink domain.

5. THE REGRESSION TESTING PATTERN: Walk me through how a Principal engineer uses
   eval not just as a one-time measurement but as a regression gate:
   - How do I run eval automatically when I change chunking strategy?
   - What is the "metric floor" concept — how do I define when a change is a regression?
   - What does a minimal CI eval pipeline look like?

6. THE PORTFOLIO FRAMING: I need to present this eval harness on GitHub and in a
   ServiceLink technical interview. Help me build the framing:
   - What is the one-sentence description of what I built?
   - What are the before/after RAGAS numbers I should target to demonstrate impact?
   - What does the LinkedIn post structure look like to make this visible?
   - What is the README structure for emcdo411/enterprise-rag-api?

7. WEEK 1 SYNTHESIS: I have now completed the full RAG engineering depth sprint.
   Walk me through a 5-minute verbal summary of everything I learned this week
   as if I am delivering it to the ServiceLink engineering panel. I will say it.
   You grade it against Principal-level standard.

Final quiz: 3 questions — one from each of the top 3 most important concepts of the
week. I answer. You grade. You tell me where my answer is at Senior level and what
moves it to Principal level.

Format: Conversational, rigorous. Code for the harness upgrade. ServiceLink domain
throughout. End with the verbal synthesis drill.
```

---

**Session 5 exit check:**
- Can I explain what RAGAS Faithfulness actually measures under the hood?
- Do I have a golden dataset strategy for ServiceLink's document types?
- Can I give the 5-minute verbal synthesis to the ServiceLink panel right now?

---

---

## Week 1 Synthesis — What You Can Now Do

After completing all five sessions, you can walk into a ServiceLink technical interview
and answer any of the following:

**Chunking:** "Walk me through how you selected chunking strategy for a dense legal
contract corpus and what your evaluation showed."

**Embedding:** "How would you select an embedding model for a regulated financial
document environment where PII cannot leave the perimeter?"

**Hybrid Retrieval:** "Why does pure vector search fail on regulatory codes and named
entities, and how did you address it?"

**Re-ranking:** "What is the latency tradeoff of adding a cross-encoder re-ranker and
how would you make the go/no-go decision for production?"

**Evaluation:** "How do you know your RAG pipeline is actually working, and how do you
prevent regressions when you make changes?"

These are the exact questions that separate Principal engineers from seniors who read
about RAG. You will have built the answers from the ground up.

---

## Week 2 Preview

Next week moves to Multi-Agent Orchestration (LangGraph). The flagship build becomes
a document processing pipeline: Intake Agent -> Classifier Agent -> Extraction Agent ->
Validation Agent -> Output Agent. This maps directly to ServiceLink's document workflow.
The RAG pipeline you built in Week 1 becomes the retrieval layer inside the Extraction
Agent.

---

*Erwin Maurice McDonald | Epoch Frameworks LLC | DACR License v2.7 | McDonald (2026)*
*Agentic AI Engineer Accelerator v1.0 | ServiceLink Client Integrations Track*
