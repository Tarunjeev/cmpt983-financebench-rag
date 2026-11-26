
# CMPT 983 — FinanceBench RAG Experiment Contract

## 1. Purpose

This document defines the shared data formats and experimental
settings for our FinanceBench RAG project.

All team members must follow these specifications to ensure fair
and reproducible comparisons.

We will evaluate four configurations:
- A: No-RAG LLM baseline
- B: Standard vector RAG
- C1: RAG with company/year metadata filtering
- C2: RAG with metadata filtering and cross-encoder reranking

## 2. Evaluation Dataset

File: `data/eval_set.jsonl`

We will use one fixed evaluation set of 60–80 FinanceBench questions,
covering factual lookup and numerical reasoning questions.

Each JSONL row must contain:

| Field | Description |
|---|---|
| question_id | Unique question identifier |
| question | Financial question text |
| company | Company associated with the question |
| answer | Gold reference answer |
| question_type | `lookup` or `numerical` |
| evidence | List of gold supporting document pages |

Each evidence entry contains:
- `doc_name`: SEC document identifier
- `page_number`: Supporting PDF page number

Gold answers and evidence must only be accessed during evaluation.

## 3. Document Chunks

File: `data/chunks.jsonl`

Each extracted document chunk must contain:

| Field | Description |
|---|---|
| chunk_id | Unique chunk identifier |
| text | Extracted financial document text |
| company | Company associated with the document |
| doc_name | Source SEC document identifier |
| fiscal_year | Fiscal year of the report |
| page_number | Original PDF page number |

PDF extraction will use PyMuPDF.

Chunks will contain approximately 256–512 tokens with overlap.

The chunking process must preserve document and page metadata.

## 4. Retrieval Output

File: `results/retrieval_{system}.jsonl`

Applicable systems: B, C1, C2.

Each row contains:
- `question_id`
- `system`: B, C1, or C2
- `top_chunks`: Ranked list of the top five retrieved chunks

Each retrieved chunk entry contains:
- `chunk_id`
- `rank`
- `score`

Ranks start at 1, with the most relevant chunk first.
Scores are system-specific and should not be directly compared
between vector search and cross-encoder reranking.

## 5. Generated Answer Output

File: `results/answers_{system}.jsonl`

Applicable systems: A, B, C1, C2.

Each row contains:
- `question_id`
- `system`
- `answer`: Generated answer text
- `chunk_ids`: IDs of chunks provided to the LLM

For System A, `chunk_ids` must be an empty list.

For Systems B, C1, and C2, `chunk_ids` records the
retrieved context used to generate the answer.

## 6. Experimental Configurations

### System A — No RAG

Pass the question and standardized instruction prompt
directly to the generator LLM.

No financial document context is supplied.

### System B — Standard Vector RAG

Use dense embeddings stored in a shared FAISS index.

Retrieve the top 5 chunks using cosine similarity.

No company/year metadata filtering or reranking is applied.

### System C1 — Metadata Filtering

Use company and fiscal-year metadata to restrict
the document search.

Retrieve the top 5 relevant chunks from the filtered candidates.

### System C2 — Metadata Filtering + Reranking

Apply the same metadata filtering as C1.

Retrieve 15–20 candidate chunks.

Use the cross-encoder reranker to reorder candidates.

Pass the 5 highest-ranked chunks to the generator.

## 7. Fixed Model Settings

| Setting | Value |
|---|---|
| Embedding model | `BAAI/bge-small-en-v1.5` |
| Reranker | `BAAI/bge-reranker-base` |
| Vector index | FAISS |
| Retrieval top-k | 5 |
| C2 candidate count | 15–20 |
| Generator LLM | One shared model, to be selected by the team |
| Temperature | 0.0 |
| Evaluation set | Same frozen 60–80 questions |
| Prompt | One standardized instruction prompt |

The selected generator model, version, and prompt text
must be recorded before running the experiments.

## 8. Experimental Fairness Rules

1. All systems use the same generator LLM.
2. All systems use the same frozen evaluation questions.
3. Temperature remains 0.0.
4. The same instruction prompt is used for every system.
5. Gold reference answers are evaluation-only.
6. Gold evidence pages are evaluation-only.
7. Retrieval and generation must not access gold evidence pages
   or gold answers.
8. Only the retrieval strategy changes between B, C1, and C2.
9. All system outputs use the shared JSONL schemas.
10. Do not modify the evaluation question set after experiments begin.

## 9. Evaluation Metrics

### Retrieval Quality

- Page-level Recall@5
- Page-level Precision@5

Compare retrieved document/page metadata against the
FinanceBench gold supporting pages.

### Answer Quality

- End-to-end answer accuracy
- Lookup question accuracy
- Numerical question accuracy

Numerical comparisons will use a ±1% tolerance
where appropriate, including correct units and interpretation.

### Failure Analysis

Incorrect RAG answers will be classified as:

- Retriever failure: Required supporting evidence is not retrieved.
- Generator failure: Relevant evidence is retrieved, but the
  generated answer is incorrect.

## 10. Team Responsibilities

- Tarunjeev Juneja: Shared contract, evaluation dataset,
  document extraction and chunking, System A.
- Sabah Shaikh: Metadata filtering and cross-encoder reranking.
- Kevin Gonzalez Castro: Shared generator and evaluation,
  answer generation and error analysis.

All members must use the schemas and experiment settings
defined in this document.

## 11. Contract Status

Status: Proposed — pending team review.

This contract must be reviewed and agreed upon before
implementing dependent GitHub issues.

Related issue: #2
