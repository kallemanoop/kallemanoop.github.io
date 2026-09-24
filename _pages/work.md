---
permalink: /work/
title: "Work"
layout: single
author_profile: true
---

### Professional Experience

### **Founding AI Engineer - McGowan & Cecil, LLC**
May 2026 - Present, Laurel, MD

Built lawMCS AI, a case assistant for a personal injury law firm. It reads every document in a case (medical records, scans, photos, emails) and works like a paralegal.

- Multimodal ingestion with per-page text and vision routing, chunking with file and page provenance, and pgvector retrieval.
- LangGraph RAG agent for cited Q&A over case evidence.
- Pydantic-validated demand letter generator. The model writes prose; Python computes totals and validates medical codes.
- Multi-agent autofill for the firm's case management system: a master agent classifies documents and routes them to nine section agents (Meds, Liens, Lost Wages), each with its own schema and write path. Every value is quote-grounded and approved by a human before it is written.
- Offline evaluation harness with answer keys built from paralegal approve/reject decisions.
- Traced in Langfuse with PHI masked for HIPAA.

---
## Skills

**Languages:** Python, C++, R, SQL, TypeScript  
**ML & DL:** PyTorch, TensorFlow, JAX, Hugging Face, LangChain, LangGraph, vLLM  
**Backend & Infra:** FastAPI, PostgreSQL, pgvector, Redis, Docker, AWS (S3, EC2, Lambda, Bedrock), Airflow  
**Evaluation & Observability:** Langfuse, LangSmith, Grafana
