# 🔍 ClaimLens

> **AI-powered RAG system for insurance policy analysis.**

ClaimLens is an advanced Retrieval-Augmented Generation (RAG) system designed to extract and reason over complex insurance policy clauses. It utilizes hybrid retrieval (FAISS + BM25) combined with cross-encoder reranking to accurately answer policy-related queries with high-confidence citations.

## 🌟 Key Features

- **Intelligent Ingestion**: Automatically loads, parses, and chunks insurance policies (PDFs) into logical, clause-level documents.
- **Hybrid Retrieval**: Employs Dense embeddings (FAISS) alongside Sparse term-matching (BM25) for high recall.
- **Cross-Encoder Reranking**: Re-evaluates retrieved clauses to ensure maximum relevance to user queries.
- **Advanced Reasoning Engine**: Leverages LLMs (via Groq/LangChain) to provide precise, structured answers with source citations.
- **Audit-Ready Accuracy**: Provides confidence scores and exact clause IDs (including start pages) for every generated answer.

## 🏗️ Architecture Stack

- **Framework**: [LangChain](https://github.com/langchain-ai/langchain) & [LangGraph](https://github.com/langchain-ai/langgraph)
- **Vector Store**: [FAISS](https://github.com/facebookresearch/faiss) (CPU) & BM25
- **Embeddings**: HuggingFace (`BAAI/bge-base-en-v1.5`)
- **LLM Engine**: Groq (Llama-based inference)

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- A valid [Groq API Key](https://console.groq.com/)

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/ClaimLens.git
   cd ClaimLens
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

## 🛠️ Usage

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

## 📂 Project Structure

```text
ClaimLens/
├── app/
│   ├── config.py             # System configuration
│   ├── ingestion/            # Document loaders and chunking splitters
│   ├── retrieval/            # Hybrid retriever (FAISS + BM25 + Reranker)
│   ├── reasoning/            # Prompts, schemas, and LLM reasoners
│   └── pipeline.py           # Core RAG orchestration pipeline
├── data/                     # Source PDFs (Policy documents)
├── docs/                     # Additional documentation
├── scripts/                  # CLI scripts (run_pipeline, evaluation, etc.)
├── indexes/                  # Generated FAISS/BM25 Vector stores
└── requirements.txt          # Python dependencies
```

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/yourusername/ClaimLens/issues).

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.
