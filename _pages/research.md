---
permalink: /research/
title: "Research"
layout: single
author_profile: true
---

### Research Focus
I study how language models make decisions and where in the network those decisions form, using interpretability tools such as *attribution patching*, *logit lens*, and *causal ablation*.

## Projects

### **Mechanistic Interpretability of Ethical Reasoning in Pre-Trained Language Models: A Multi-Tier Analysis of Compliance Under Psychological Pressure**
May 2025 - Feb 2026

Three-tier evaluation framework (psychometric profiling, severity-weighted compliance evaluation, mechanistic interpretability) applied to 10 base LLMs across 680 ethical dilemmas and 8 pressure types. Introduced two metrics, CRS and MRE, that expose behavior patterns invisible to binary accuracy. Localized decision-relevant computation to late-layer MLPs using attribution patching, direct logit attribution, logit lens, and layer-wise entropy. Causal MLP ablation flips the model's choice in up to 70% of prompts.

### **lawMCS AI**
May 2026 - Present

Multimodal case assistant for McGowan & Cecil, LLC. Cited Q&A over case evidence, demand letter generation, and multi-agent autofill of the firm's case management system with human approval. See [Work](/work/).

### **KnowledgeX - AI-Powered Skill Exchange Platform**
NexHacks 2026, CMU. [Code](https://github.com/kallemanoop/knowledge_debt_exchange)

Peer-to-peer skill trading platform. Conversational LLM agents handle onboarding and semantic search matches users with complementary learning needs. Agentic RAG with LangGraph for extraction, retrieval, and verification. Input token compression reduced inference token usage by 66%.

### **DefinitelyNotGPT - Small Language Model from Scratch**
June - Sept 2025. [Code](https://github.com/kallemanoop/DefinitelyNotGPT)

Transformer-decoder SLM (6 layers, 256 hidden size, 4 attention heads, ~7M parameters) with a custom byte-level BPE tokenizer, RMSNorm, rotary positional embeddings, KV cache, and mixed-precision training.
