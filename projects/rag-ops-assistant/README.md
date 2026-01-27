# RAG Ops Assistant (Qdrant + llama.cpp)

## Summary
A compact Retrieval-Augmented Generation (RAG) assistant for infrastructure operations, optimized for **local inference** and reduced hallucination on domain topics (Kafka/Loki/Prometheus, etc.).

## Problem
- Small local models can hallucinate or miss niche operational details.
- Operators need fast, grounded answers from curated docs/runbooks.

## Solution Overview (Spec)
- **Ingestion**:
  - markdown/doc chunking by headings
  - deterministic IDs: `{source}:{topic}:{section}:{chunk_index}`
  - store chunks + metadata in Qdrant
- **Retrieval**:
  - query → embedding → top-k chunks
  - context window budget + dedupe
- **Generation**:
  - llama.cpp model answers using retrieved context
  - response format supports references/URLs from ingested docs

## Deliverables
- Ingestion pipeline specification (chunking + IDs + metadata rules)
- Qdrant collection layout and payload schema
- Prompt templates for operator-style answers (steps + verification + caveats)

## Tech
Qdrant, embeddings, llama.cpp, Python

## Next Steps
- Add “diagnostic mode” that requests metrics/log snapshots via MCP
- Evaluate smaller/faster models vs accuracy trade-offs for CPU-only environments