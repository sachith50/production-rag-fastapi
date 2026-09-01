# Production RAG System Over Technical Documents

A production-oriented **Retrieval-Augmented Generation (RAG)** system for querying technical documents with high-precision retrieval, hybrid search, cross-encoder re-ranking, and hallucination-aware answer generation.

Built with **FastAPI, PostgreSQL, pgvector, LangChain, BM25, vector embeddings, and OpenAI**.

---

## 🚀 Key Features

### 🔍 Hybrid Retrieval

Combines:

- **BM25** for exact keyword and technical-term matching
- **pgvector** for semantic vector search
- Combined candidate retrieval for improved recall

### 🎯 Cross-Encoder Re-ranking

The system retrieves the top candidate chunks using hybrid search and then applies a **cross-encoder** to score query-document relevance.

This improves precision before the retrieved context is passed to the LLM.

### 📊 Quantitative Evaluation

The retrieval pipeline was evaluated using a **30-question labeled evaluation set**.

| Retrieval Strategy | Top-5 Hit Rate | Top-10 Hit Rate |
| --- | ---: | ---: |
| Naive Vector Search | 61.2% | 72.4% |
| Hybrid + Cross-Encoder | **89.1%** | **94.8%** |

This improved Top-5 retrieval hit rate by **27.9 percentage points** over the baseline.

### 🛡️ Hallucination Guardrails

The generation pipeline is designed to keep answers grounded in retrieved documentation.

- Answers are generated using retrieved document context
- Responses include inline source citations
- Retrieved chunks are mapped to their source documents
- The system can refuse to answer when sufficient context is unavailable

### ⚡ Asynchronous Processing

FastAPI provides the backend API while heavier document-ingestion tasks can be processed asynchronously.

The ingestion pipeline includes:

- PDF/DOCX parsing
- Recursive chunking
- Embedding generation
- Vector database ingestion

---
# 🏗️ System Architecture

## Document Ingestion

```text
PDF / DOCX
    ↓
Document Parsing
    ↓
Recursive Chunking
    ↓
Vector Embeddings
    ↓
PostgreSQL + pgvector
```

## Query & Retrieval

```text
User Query
    ↓
Hybrid Retrieval
(BM25 + pgvector)
    ↓
Top-20 Candidate Chunks
    ↓
Cross-Encoder Re-ranking
    ↓
Ranked Context
    ↓
LLM Generation
    ↓
Answer + Inline Citations
```

---

# 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| Backend | FastAPI |
| Language | Python 3.10+ |
| RAG Framework | LangChain |
| Database | PostgreSQL |
| Vector Search | pgvector |
| Sparse Retrieval | BM25 |
| Dense Retrieval | Vector Embeddings |
| Re-ranking | Cross-Encoder |
| LLM | OpenAI API |
| Dependency Management | Poetry |
| Document Formats | PDF, DOCX |

---

# 📁 Project Structure

```text
project/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   └── ...
│   ├── pyproject.toml
│   └── .env
│
├── evaluation/
│   └── ...
│
└── README.md
```

> The structure above is representative and may vary depending on the implementation.

---

# ⚙️ Installation & Setup

## Prerequisites

Make sure you have:

- Python 3.10+
- Poetry
- PostgreSQL
- `pgvector` extension
- OpenAI API key

## 1. Clone the Repository

```bash
git clone <repository-url>
cd <repository-name>
```

## 2. Install Dependencies

```bash
cd backend
poetry install
```

## 3. Configure Environment Variables

Create a `.env` file:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/rag_db
OPENAI_API_KEY=your_api_key_here
```

Update the database credentials according to your PostgreSQL setup.

---

# ▶️ Running the Application

Start the FastAPI server:

```bash
poetry run uvicorn app.main:app --reload
```

The API will then be available locally.

FastAPI's automatically generated API documentation can be used to explore and test the available endpoints.

---

# 🔄 How the RAG Pipeline Works

## Step 1 — Document Ingestion

Technical documents are parsed and divided into smaller chunks using recursive chunking.

Each chunk is converted into a vector embedding and stored in PostgreSQL using pgvector.

## Step 2 — User Query

When a user submits a question, the system performs two types of retrieval:

- **BM25 keyword search**
- **Dense vector similarity search**

## Step 3 — Candidate Generation

The results from both retrieval methods are combined to produce a candidate set.

The system considers the top **20 candidate chunks** for the next stage.

## Step 4 — Re-ranking

A cross-encoder evaluates the relevance of each candidate chunk against the user's query.

The candidates are then sorted according to their relevance scores.

## Step 5 — Answer Generation

The highest-ranked chunks are provided to the LLM as context.

The LLM generates an answer based on the retrieved documentation and provides source citations.

## Step 6 — Hallucination Prevention

If the retrieved context does not contain sufficient information to answer the question, the system can return a fallback response instead of generating an unsupported answer.

---

# 📈 Evaluation Results

The system was evaluated against a **30-question labeled dataset**.

| Approach | Top-5 Hit Rate | Top-10 Hit Rate |
|----------|---------------:|----------------:|
| Naive Vector Search | 61.2% | 72.4% |
| BM25 + pgvector + Cross-Encoder | **89.1%** | **94.8%** |

### Improvement

- **Top-5:** +27.9 percentage points
- **Top-10:** +22.4 percentage points

The results demonstrate the improvement obtained by combining sparse retrieval, dense retrieval, and cross-encoder re-ranking.

---

# 🧠 Why Hybrid Search?

Vector search is useful for understanding semantic similarity, but technical documents often contain exact terms such as:

- Function names
- Error messages
- API names
- Identifiers
- Acronyms
- Technical terminology

BM25 is particularly useful for these exact matches.

Combining BM25 with vector search allows the system to capture both:

> **Exact terminology + Semantic meaning**

---

# 🛡️ Grounding Strategy

The system follows a simple principle:

> **If the retrieved documentation does not provide enough evidence, do not invent an answer.**

The generation pipeline therefore uses retrieved document chunks as the primary source of information and associates generated responses with the relevant source chunks.

---

# 🔮 Future Improvements

Potential extensions include:

- Query rewriting
- Metadata-aware retrieval
- Retrieval caching
- Streaming responses
- Document versioning
- Authentication and rate limiting
- Docker-based deployment
- Retrieval observability and tracing
- Larger automated evaluation datasets
- Conversational memory

---

# 👨‍💻 Author

**Sachith Kanwa**

A production-oriented exploration of **RAG, information retrieval, vector databases, LLM grounding, and document question answering**.
