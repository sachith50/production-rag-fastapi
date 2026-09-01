# SKILL.md: Production RAG System Over Documents
- Track: Applied AI / Generative AI
- Stack: Python, FastAPI, pgvector, LangChain

## Key Architecture Decisions
- Hybrid Search: Combined BM25 keyword search with dense vector embeddings to capture exact terminology alongside semantic intent.
- Re-ranking Layer: Used cross-encoder re-ranking to score retrieved contexts prior to prompt injection.
- Evaluation: Measured retrieval hit-rate against a 30-question labeled evaluation set.
