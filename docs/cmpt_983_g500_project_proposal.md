# **Project Outline: Improving Financial Document Question Answering Through Retrieval-Augmented Generation (RAG)**

## **1\. Who Is in Your Group?** Kevin, Sabah, & Tarun

## **2\. What Is the Topic / Task You Would Like to Work On?** 

### **2.1 Problem Overview**

Financial analysts regularly extract metrics, compare performance across periods, and verify disclosures from corporate filings such as SEC Form 10-K and 10-Q reports (often 80 to 200+ pages per document). Answering questions over these filings requires locating exact figures buried in dense text or financial tables, respecting reporting periods, and performing arithmetic.

General-purpose Large Language Models (LLMs) frequently hallucinate or refuse to answer when the underlying filing is absent from their pre-training data. **Retrieval-Augmented Generation (RAG)** addresses this by retrieving relevant passages from an external document store and passing them to the LLM as context. However, on the **FinanceBench** benchmark, standard RAG connected to a shared vector store fails **81% of the time** (achieving only 19% accuracy) because conventional vector search retrieves passages from the wrong company or fiscal year, separates table headers from rows, and struggles to rank the most relevant evidence.

### **2.2 Research Questions**

1. **RQ1:** How much does standard RAG improve financial question-answering accuracy compared with an LLM operating without document retrieval?  
2. **RQ2:** Does an enhanced retrieval strategy, specifically **metadata filtering** and **cross-encoder reranking**, outperform a standard dense vector-search RAG pipeline?  
3. **RQ3:** How directly does retrieval quality (finding the right page/passage) translate into factual reliability in the generated answer?  
4. **RQ4:** Which types of financial questions (e.g., multi-step numerical calculations or table lookups) remain difficult even when the right document passage is retrieved?

### **2.3 Research Hypotheses**

* **H1:** A RAG-based system will achieve higher factual accuracy on corporate filing questions than the same LLM without external document retrieval.  
* **H2:** Enhancing retrieval via company/year metadata filtering and cross-encoder reranking will significantly increase both retrieval recall and end-to-end answer accuracy compared with standard vector similarity search alone.  
* **H3:** Questions requiring numerical calculations or table interpretation will exhibit higher error rates than direct factual lookup questions, even when the correct supporting page is retrieved.

### **2.4 Scope & Feasibility**

We will evaluate our pipelines on FinanceBench using free-tier LLM APIs and local CPU embeddings, with minimal cost and an estimated workload of 8–12 hours per person.

## **3\. How Do You Plan to Approach Your Topic / Task?** 

## **3.1 Step-by-Step Execution Plan** 

#### **Phase 1: Dataset & PDF Processing** 

1. **Dataset Setup:** Download the public FinanceBench dataset (PatronusAI/financebench) and its associated SEC PDF filings. To respect free-tier API rate limits and iteration time, we will establish a core stratified evaluation set of **60 to 80 questions** covering both factual lookups and numerical questions.  
2. **PDF Text Extraction & Chunking:** Extract machine-readable text from the source PDFs using PyMuPDF (fitz), dividing documents into overlapping text chunks (e.g., 256–512 tokens) while tagging every chunk with a metadata dictionary: {company, doc\_name, fiscal\_year, page\_number}.

#### **Phase 2: Building the Experimental Configurations**

We will implement and compare three core systems using Python in a reproducible Jupyter/Colab notebook environment:

* **System A \- No-RAG LLM Baseline:**  
  Pass only the question and a standardized instruction prompt to our generator LLM without any retrieved filing text. This measures what the model already knows from pre-training versus where it hallucinates or abstains.  
* **System B \- Standard Vector RAG (Vanilla Baseline):**  
  Convert all document chunks into dense vector embeddings using a lightweight local embedding model (BAAI/bge-small-en-v1.5 or all-MiniLM-L6-v2) stored in a single shared **FAISS** vector index. For each question, retrieve the top\-$k$ chunks ($k=5$) via cosine similarity and feed them with the question to the generator LLM.  
* **System C \- Enhanced RAG (Metadata Filtering \+ Cross-Encoder Reranking):**  
  Upgrade the retrieval stage without changing the generator LLM:

  1. **Step C1 (Metadata Filtering):** Narrow the candidate chunks using company name and reporting year extracted from the query so the retriever does not pull tables from the wrong company or fiscal period.  
  2. **Step C2 (Cross-Encoder Reranking):** Retrieve the top 15–20 candidate chunks after metadata filtering and re-score question-passage relevance using a lightweight pre-trained cross-encoder (BAAI/bge-reranker-base via sentence-transformers), passing the top $k=5$ reranked passages to the LLM. *(We will log results for C1 alone and C1+C2 together to isolate each component's impact.)*

#### **Phase 3: Experimental Controls, Evaluation & Error Analysis**

To ensure fair, scientific comparisons across all systems, we will keep the following controls constant:

* Identical generator LLM, temperature (0.0 for deterministic decoding), and evaluation question set across Systems A, B, and C.  
* No access to gold reference answers or oracle evidence pages during retrieval or generation.

We will evaluate every system across three dimensions:

| Evaluation Dimension | Metrics & Procedure |
| :---- | :---- |
| **1\. Retrieval Quality** | **Page-Level Recall@k & Precision@k:** Compare the {doc\_name, page\_number} metadata of the top\-$k$ retrieved chunks against FinanceBench's ground-truth evidence page annotations. |
| **2\. Answer Quality** | **End-to-End Accuracy:** Compare generated answers against FinanceBench reference answers for factual equivalence and numerical match (within a standard $\pm 1%$ rounding tolerance), separating accuracy on Lookup vs. Numerical questions. |
| **3\. Failure Mode Taxonomy** | Manually categorize incorrect outputs in Systems B and C into **Retriever Failures** (correct evidence page missing from top\-$k$) vs. **Generator Failures** (correct page retrieved, but the LLM misread a table row, unit, or calculation). |

## **4\. Have You Surveyed Course Readings / Material Relevant to Your Project?** .

1. **Course-Provided FinBERT & FinGPT Notebooks:** As discussed in class, FinBERT is a fast encoder model suited for short-text sentiment classification, whereas generative tasks like document QA require autoregressive decoders such as FinGPT or instruction-tuned LLMs. We include a targeted zero-training inference check with the course's FinGPT notebook as an optional baseline comparison while demonstrating why external document retrieval is necessary for specific corporate filings.  
2. **Course Scope & Evaluation Principles:** In accordance with the syllabus, our project focuses strictly on financial text and document analysis and emphasizes controlled ablation methodology and transparent error analysis over cherry-picked results.

## **5\. Have You Surveyed Additional Literature Relevant to Your Project?** 

1\. **Islam et al. (2023) – FinanceBench:** Introduces the FinanceBench dataset and highlights limitations of standard RAG, which achieves only 19% accuracy across multiple financial filings. This motivates our focus on improving document retrieval.

2\. **Lai et al. (2025) – SEC-QA:** Demonstrates that document-level filtering improves financial question-answering accuracy from 30% to 80%. 

3\. **Lithgow-Serrano et al. (2025) – Assessing RAG System Capabilities:** Evaluates different RAG approaches for financial documents and highlights challenges with retrieval and numerical reasoning. This motivates our use of **cross-encoder reranking** and separate evaluation of numerical questions.

4\. **Jimeno-Yepes et al. (2024) – Financial Report Chunking:** Shows that traditional text chunking can break financial tables and reduce answer accuracy. Structure-aware chunking improves performance by preserving tables and headings.

5\. **Chen et al. (2023) – Program of Thoughts Prompting:** Demonstrates that using Python for mathematical calculations can improve numerical reasoning accuracy from 40.4% to 64.5% on FinQA.

