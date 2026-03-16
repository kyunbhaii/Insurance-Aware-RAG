# Insurance Aware RAG

> **AI-powered RAG system for insurance policy analysis.**

This RAG system is an advanced Retrieval-Augmented Generation (RAG) system designed to extract and reason over complex insurance policy clauses. It utilizes a two-stage retrieval pipeline (Dense Retrieval + Cross-Encoder Reranking) combined with strictly enforced structural validation to accurately answer policy-related queries with high-confidence citations.

## Key Features & Architectural Specialties

- **Deterministic Clause Splitting**: Abandons naive text chunking. Policies are parsed into atomic structural clauses with deterministic, stable IDs (e.g., `ICICILombard_p8_Grace_Period_1`) to ensure perfect evaluation traceability and prevent vector overwrites.
- **Two-Stage Retrieval Pipeline**: 
  - *Candidate Generation*: High-recall dense retrieval using FAISS (`BAAI/bge-base-en-v1.5`).
  - *Cross-Encoder Reranking*: Precision reranking of the top candidates using `BAAI/bge-reranker-base` to surface the most relevant evidence.
- **Strictly Validated Reasoning**: Leverages exact Pydantic schemas (`RAGResponse`, `Citation`) to strictly enforce LLM output. The system acts as a structural firewall that rejects ungrounded hallucinated citations and logically contradictory responses.
- **Fail-Fast Engineering**: Designed with strict internal verification. Duplicate clause IDs raise errors during ingestion, and retrieval/reasoning layers fail fast on missing context to avoid silent degradation. 
- **Validation-Driven Evaluation Framework**: Includes a built-in evaluator (`evaluate_stagewise`, `evaluate_multi_clause`) mapping ground-truth clause IDs to measure exact Retrieval Recall@K and MRR.
## Architecture Stack

- **Framework**: [LangChain](https://github.com/langchain-ai/langchain) & [LangGraph](https://github.com/langchain-ai/langgraph), with [Pydantic](https://docs.pydantic.dev/) for strict schema validation
- **Vector Store**: [FAISS](https://github.com/facebookresearch/faiss) (CPU)
- **Embeddings**: `BAAI/bge-base-en-v1.5` (Dense) & `BAAI/bge-reranker-base` (Cross-Encoder)
- **LLM Engine**: Groq (Llama-based inference)

## Getting Started

### Prerequisites

- Python 3.10+
- A valid [Groq API Key](https://console.groq.com/)

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/kyunbhaii/Insurance-Aware-RAG.git
   cd Insurance-Aware-RAG
   ```

2. **Set up a virtual environment** (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Environment Variables**:
   Create a `.env` file in the root directory and add your Groq API key:
   ```env
   GROQ_API_KEY="your_actual_groq_api_key"
   ```

## Usage

### 1. Place Your Data
Place your insurance policy PDFs in the `data/` directory. For example, `data/icici_complete_health.pdf`.

### 2. Run the Full Pipeline
The main script processes the document, generates the embeddings, builds the retriever, and executes a sample query.

```bash
python -m scripts.run_pipeline
```

### 3. Review the Output
The system handles queries (e.g., *"Is maternity covered?"*) and returns a structured response:
```text
Answer: [Detailed reasoning paragraph]
Found: True/False
Citations:
  - Clause 4.2 (page 12)
Confidence: High
```

### Advanced Scripts
- **Testing Retrieval**: `python -m scripts.test_retrieval` to benchmark document retrieval performance.
- **Data Ingestion Preview**: `python -m scripts.main` to preview how your PDF is split into clauses.
- **Evaluation**: Explore `app/evaluation` and `scripts/run_evaluation.py` for comprehensive RAG testing.

## Project Structure

```text
ClaimLens/
├── app/
│   ├── config.py
│   ├── ingestion/
│   ├── retrieval/
│   ├── reasoning/
│   └── pipeline.py
├── data/
├── docs/
├── scripts/
├── indexes/
└── requirements.txt
```

## Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/kyunbhaii/Insurance-Aware-RAG/issues).

## License

This project is licensed under the MIT License - see the LICENSE file for details.
