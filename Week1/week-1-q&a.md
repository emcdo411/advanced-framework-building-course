# Two Registers. One Answer. Let's Run Both.

Here's how this works: for each question I'll give you the **Principal-level technical answer** first, then immediately translate it into **plain language** — the version you'd use with a ServiceLink VP, a title operations manager, or a lending executive who has never heard the word "embedding" and doesn't need to.

The translation is not dumbed down. It's re-anchored to what the business actually cares about: accuracy, speed, cost, and risk.

---

## Question 1

*A ServiceLink engineer tells you the team wants to add 50,000 historical HUD-1 settlement statements to the RAG corpus. These are standardized 2-page forms with fixed field layouts — borrower name, lender name, line-item settlement charges, totals. Which chunking strategy do you recommend and why? What's the one failure mode you'd specifically test for with this document type?*

---

### Technical Answer (Principal Level)

HUD-1 settlement statements are structurally homogeneous, field-delimited, and short. The document type has essentially no complex clause structure — it's a ledger, not a legal instrument. Semantic chunking's advantage is specifically about preserving multi-sentence legal clause dependencies. That advantage disappears entirely when the document has no clause dependencies to preserve.

For 50,000 HUD-1s I'd recommend fixed-size chunking at 256 tokens with zero overlap, treating each logical section of the form as a natural boundary — Line 100-199 (Gross Amount Due from Borrower), Line 200-299 (Amounts Paid by Borrower), Line 700 (Total Real Estate Broker Fees), and so on. The HUD-1's own structure is already the right retrieval unit. You don't need a similarity algorithm to find the boundary because the form defines it.

The one failure mode I'd test for specifically is **numeric identifier collision**. HUD-1s contain loan numbers, parcel numbers, MLS IDs, and tax IDs that are structurally identical across thousands of documents — eight to twelve digit strings that look alike but refer to completely different transactions. Pure vector search on these identifiers will produce catastrophic false positives, because the embedding model has no way to distinguish loan number 4471823901 from loan number 4471823902 — they produce nearly identical vectors.

The test is simple: build 20 golden dataset queries of the form "what were the total settlement charges on loan 4471823901?" and measure how often the retriever returns the correct HUD-1 versus a different transaction with a similar loan number. If Precision@5 is below 0.90 on this test, the answer is not to change the chunking strategy — it's to add a BM25 layer that handles exact-match numeric retrieval, which is Wednesday's architectural upgrade.

---

### Plain Language Version

Think about what a HUD-1 actually is. It's a receipt — a standardized government form that lists every dollar that changed hands at a closing. Borrower paid this. Lender credited that. Title company charged this fee. It has the same layout every single time. Two pages. Fixed sections. Numbers in rows.

Now compare that to a deed of trust, which is a 40-page legal document written in dense paragraph form where one sentence refers back to a definition three pages earlier, and a clause in Section 12 is conditional on language buried in Section 4. Those are two completely different document types, and they need different handling.

For the HUD-1s, I'd use the simplest approach because the simplest approach is the right one. The form already tells you where the sections are. You don't need a computer to figure out that "Line 100 through 199" is about what the borrower owes and "Line 700" is about broker fees — the government printed that on the form. So I'd use that structure as the filing system and skip the expensive similarity analysis that semantic chunking runs. Faster to build, cheaper to run, and no quality loss because there was nothing to lose.

The one thing I'd specifically watch out for is this: HUD-1s have loan numbers, parcel numbers, and tax IDs that look almost identical across thousands of transactions. A number like 4471823901 and a number like 4471823902 are completely different loans — different borrowers, different properties, different transactions — but to the AI, they look almost the same because they're just long strings of digits.

If a title officer asks "pull up the settlement statement for loan 4471823901," the AI needs to return exactly that loan, not a similar-looking one. The way I'd test for this is to run 20 queries using specific loan numbers and measure how often the system gets the right document back versus a lookalike. If it's getting confused, the fix is to add a keyword matching layer alongside the AI search — a hybrid approach where exact numbers get matched exactly, and conceptual questions get handled by the AI. That's a known engineering pattern and it's what I'd build.

---

## Question 2

*Your Precision@5 for semantic chunking is 0.74. A principal engineer at ServiceLink asks you: "What would it take to get to 0.85?" Walk me through the specific architectural changes you would make and in what order.*

---

### Technical Answer (Principal Level)

Going from 0.74 to 0.85 is an 11-point lift. That's not achievable through any single change — it requires stacking three independent improvements, each of which addresses a different failure mode in the current pipeline. Here's the order of operations, ranked by expected lift per unit of engineering effort.

**First: Add a re-ranker.** This is the highest-leverage single change available. Right now the pipeline takes semantic chunking output and returns the top-5 chunks by cosine similarity. Cosine similarity is an approximation. The top-5 by similarity is not the same as the top-5 by relevance to the specific query. A cross-encoder re-ranker — I'd start with `BAAI/bge-reranker-v2-m3` or Cohere Rerank — takes the top-50 retrieved chunks and scores each one against the query using full bidirectional attention. It's slower than ANN search, but it runs on only 50 candidates, so the latency impact is manageable. I'd expect 4-6 points of Precision@5 lift from this change alone on a legal document corpus.

**Second: Add BM25 hybrid retrieval.** My current pipeline is pure vector search. It finds semantically similar chunks but struggles with exact matches — regulatory citations, defined terms, parcel numbers, numeric identifiers. A hybrid system runs BM25 keyword search in parallel with vector search and fuses the two ranked lists using Reciprocal Rank Fusion. For a ServiceLink corpus, which is saturated with regulatory references and exact legal terminology, I'd expect 3-5 points of lift on queries involving specific citations or defined terms. This also directly addresses the HUD-1 loan number collision problem from Q1.

**Third: Tune the semantic chunking threshold on this specific domain.** My harness used the 95th percentile breakpoint threshold. That's a conservative default. For ServiceLink's document types, I'd build a golden dataset of 50 queries with human-verified ground truth, then sweep the threshold parameter from the 85th to 99th percentile and measure Precision@5 at each setting. Domain-specific threshold tuning on a legal document corpus typically yields 2-3 points of lift because the default threshold was calibrated on general text — not deed of trust language.

Stacked in order: re-ranker gets me to approximately 0.79-0.80, hybrid retrieval to 0.82-0.83, threshold tuning to 0.85+. Each step is independently measurable, which matters because if the re-ranker doesn't deliver the expected lift, I know to investigate before spending engineering time on the next layer.

One honest caveat: 0.85 on 10 test queries is a fragile number. I'd want to expand the golden dataset to 50-100 queries before declaring the target achieved. Ten queries is enough to make architectural decisions but not enough to publish a production quality claim.

---

### Plain Language Version

Think of it this way. Right now the system works like a library assistant who reads every book in the library, memorizes what each chapter is "about," and when you ask a question, pulls the five chapters that feel most related to your question. That's what we have at 0.74. It's genuinely good. But "feels most related" is not the same as "is most relevant."

Getting to 0.85 means stacking three improvements, in this order:

**Improvement one: Add a quality reviewer.** Right now the system pulls the top 50 chapters that feel related, then hands you the first five. The upgrade is to add a reviewer — a second pass that actually reads all 50 candidates against your specific question and picks the five that are most directly responsive, not just the five that feel most similar. This is the single biggest improvement available. Think of it as the difference between a librarian who gives you the five books most likely to be about your topic versus a librarian who skims all 50 candidates and hands you the five that specifically answer your question. Expected improvement: 4-6 points.

**Improvement two: Add a keyword matching lane.** The AI is excellent at finding meaning. But it's not always excellent at finding exact strings — a specific regulation number, a loan number, a parcel ID. "Section 1024.8 of Regulation X" is a phrase that means something very specific in the mortgage world, but to the AI it looks similar to other regulatory references even though it's a completely different rule. The fix is to run two searches in parallel: the AI search that finds meaning, and a traditional keyword search that finds exact matches. You then blend the two results. This means a title officer searching for a specific regulation gets it back every time, not just when it happens to sit in a semantically convenient chunk. Expected improvement: 3-5 points.

**Improvement three: Tune the settings for this specific type of document.** The system currently uses default settings that were designed for general text — news articles, websites, general prose. Mortgage and title documents have their own rhythm: longer clauses, more formal sentence structure, defined terms that repeat across sections. Running a calibration pass on our specific document types — testing different split settings against real queries that title officers actually ask — would squeeze out another 2-3 points.

Those three improvements, stacked in that order, get us from 0.74 to roughly 0.85. Each one is independently testable, which means if the first upgrade doesn't deliver what we expected, we know to investigate before investing in the next one. We're not guessing; we're measuring at each step.

One honest note I'd add in the room: 0.74 was measured on 10 test questions. 0.85 as a production claim needs to be validated against at least 50 real queries that title officers actually use, including the hard ones — exact regulation lookups, cross-document comparisons, edge cases. The number I'd stake a production release on comes from that larger test, not from ten questions over twelve documents.

---

## Grading Your Responses

You haven't answered yet — these are my model answers for you to study. When you're ready, answer both questions in your own words — either in technical mode, plain-language mode, or both — and I'll grade you against Principal standard and tell you exactly what each answer is missing and what would move it up.

What the grading will look for specifically:

For Q1, a Senior answer names the right strategy. A Principal answer names the strategy, explains why this document type makes the strategy choice obvious, and proactively identifies the failure mode that would still exist after making the right choice.

For Q2, a Senior answer lists the three improvements. A Principal answer sequences them by expected lift per engineering effort, gives honest caveats about sample size, and demonstrates that each improvement addresses a specific, named failure mode rather than just "making the system better."

Take your shot.
