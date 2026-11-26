
# FinanceBench Evaluation Set — Selection Methodology

## 1. Purpose

This document describes how we selected the evaluation questions for our CMPT 983 project, "Improving Financial Document Question Answering Through Retrieval-Augmented Generation (RAG)."

Our goal is to create a representative and reproducible evaluation set for comparing four experimental configurations:

- System A: No-RAG LLM baseline
- System B: Standard Vector RAG
- System C1: RAG with metadata filtering
- System C2: RAG with metadata filtering and cross-encoder reranking

All configurations will use the same evaluation questions.

## 2. Dataset

We use the publicly available FinanceBench dataset (PatronusAI/financebench), containing 150 annotated financial questions and associated SEC financial filings.

The dataset includes questions, reference answers, company names, document identifiers, reporting periods, and supporting evidence.

## 3. Sample Size

We selected 80 questions from the 150 publicly available questions.

This sample size is consistent with our original project proposal, which specified 60–80 questions to manage free-tier LLM API usage and computational requirements.

## 4. Selection Criteria

We used the following criteria:

1. Question-type coverage: Include all three original FinanceBench question categories.
2. Company diversity: Include questions covering different companies.
3. Reporting-period diversity: Include questions from different fiscal years.
4. Reasoning diversity: Include both factual lookup and numerical reasoning questions.
5. Document availability: Verify that source PDFs are available.
6. Reproducibility: Use a fixed random seed and record selected question IDs.

Questions were not selected based on the performance of any RAG system.

## 5. Sampling Procedure

We used stratified random sampling based on FinanceBench's original `question_type` column.

A fixed random seed of 42 was used.

| Original FinanceBench category | Selected questions |
|---|---:|
| Domain-relevant | 27 |
| Metrics-generated | 27 |
| Novel-generated | 26 |
| **Total** | **80** |

Within each category, questions were randomly sampled without replacement using Pandas.

The selected questions were then shuffled using the same random seed.

Company, fiscal-year and reasoning diversity were assessed after sampling rather than enforced as separate sampling quotas.

## 6. Dataset Coverage

The final selection contains:

| Characteristic | Result |
|---|---:|
| Selected questions | 80 |
| Unique companies | 31 |
| Unique reporting years | 10 |
| Unique financial documents | 58 |
| Duplicate question IDs | 0 |
| Missing required values | 0 |

## 7. Research Evaluation Categories

In addition to the original FinanceBench categories, we introduced two research-specific classifications:

- **Lookup:** Questions that can be answered by directly extracting or interpreting reported information without performing new arithmetic.
- **Numerical:** Questions requiring arithmetic, ratios, percentages, or other numerical calculations.

The preliminary classification resulted in:

| Evaluation category | Questions |
|---|---:|
| Numerical | 45 |
| Lookup | 35 |
| **Total** | **80** |

Classification was based on the provided reasoning annotations and manual review of questions with missing or ambiguous reasoning labels.

Some borderline cases require additional evidence verification, so these labels should be treated as provisional until reviewed.

## 8. Source Document Availability

The 80 selected questions reference 58 unique financial documents.

All 58 document filenames were matched against the official FinanceBench GitHub PDF collection.

- Required documents: 58
- Matched PDF filenames: 58
- Missing PDF filenames: 0

The initial automated test of original source URLs successfully identified 42 PDFs. The remaining 16 original links returned access restrictions, HTML responses, or connection errors.

The official FinanceBench PDF collection provides matching filenames for all 58 required reports.

## 9. Evaluation Data Format

The evaluation file is stored as:

`data/eval_set.jsonl`

Each JSONL record includes:

- `question_id`
- `question`
- `company`
- `doc_name`
- `doc_period`
- `question_type`
- `question_category`
- `question_reasoning`
- `answer`
- `evidence`

Supporting evidence is stored as a list containing the document name and zero-based evidence page number.

## 10. Data Leakage Prevention

The reference answers and ground-truth evidence pages are retained strictly for evaluation purposes.

During answer generation:

- The LLM receives the question.
- RAG systems receive retrieved document passages.
- Gold reference answers are not supplied to the LLM.
- Gold evidence pages are not used to select retrieved passages.

## 11. Reproducibility and Future Expansion

The selection uses a fixed random seed of 42 and a documented allocation across the original FinanceBench categories.

The selected 80 question IDs are preserved in the evaluation JSONL file.

If the project is later expanded to all 150 public questions, the updated dataset will be versioned and every system will be evaluated on the same expanded question set.

## 12. Related Issues

- Issue #2: Define the shared experiment contract
- Issue #3: Freeze the stratified evaluation set
- Issue #4: Extract text and write metadata chunks
