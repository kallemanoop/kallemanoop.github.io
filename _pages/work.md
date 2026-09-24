---
permalink: /work/
title: "Work"
layout: single
author_profile: true
---

### Professional Experience

### **Founding AI Engineer - McGowan & Cecil, LLC**
May 2026 - Present, Laurel, MD

Built lawMCS AI, a case assistant for a personal injury law firm.

- A case assistant with multi-modal ingestion and processing capability for a personal injury firm that could
read medical scans, photos, records and functions like a paralegal.
- Shipped a LangGraph RAG agent for Q&A over case evidence and generating demand letters using Qwen 3.6 Flash multimodal model paired with HNSW-indexed 1024-dimension embeddings (via Matryoshka truncation) to accelerate semantic search and citation accuracy across text, scans, and photos.
- Designed a microservice-style multi-agent system: a master agent classifies documents and routes them to nine
section agents (Meds, Liens, Lost Wages), each working with one section's schema and write path with a
human-in-loop.
- Cut autofill latency from 25 to 2.5 minutes via parallel windowing and LLM response caching by content hash.


---
## Skills

**Languages:** Python, C++, R, SQL, TypeScript  
**ML & DL:** PyTorch, TensorFlow, JAX, Hugging Face, LangChain, LangGraph, vLLM  
**Backend & Infra:** FastAPI, PostgreSQL, pgvector, Redis, Docker, AWS (S3, EC2, Lambda, Bedrock), Airflow  
**Evaluation & Observability:** Langfuse, LangSmith, Grafana
