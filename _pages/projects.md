---
permalink: /projects/
title: "Projects"
excerpt: "lawMCS AI, a mechanistic interpretability study of ethical reasoning in language models, KnowledgeX, and DefinitelyNotGPT."
layout: single
author_profile: true
---

Four things: one running in production, one research paper, one hackathon build, and one language model trained from scratch.

### lawMCS AI

<p class="role__meta">Production · 2026</p>

An LLM system that reads an entire personal-injury case file, drafts the demand letter, answers questions with citations, and proposes values for a person to approve. [Full case study →](/work/lawmcs-ai/)

### Mechanistic Interpretability of Ethical Reasoning in Pre-Trained Language Models: A Multi-Tier Analysis of Compliance Under Psychological Pressure

<p class="role__meta">Research · May 2025 – Feb 2026</p>

Where do ethical priors live inside base language models, and do the models act on them? A three-tier framework (psychometric profiling, severity-weighted compliance under 8 kinds of pressure, and mechanistic interpretability) run across 10 base models. The short answer: late-layer MLPs, and not reliably. [The full write-up →](/research/)

### KnowledgeX: trade skills, not money

<p class="role__meta">Hackathon · NexHacks ’26 at CMU · Jan 2026 · <a href="https://github.com/kallemanoop/knowledge_debt_exchange">code</a></p>

A peer-to-peer marketplace where you teach what you know to learn what you don't. Onboarding is a conversation with an LLM agent instead of a form, and matching is semantic search over skill embeddings that looks for people with *complementary* needs. Behind it, a LangGraph agentic-RAG flow runs extraction, retrieval and verification against the database (so it can't match you with someone who doesn't exist), and input-token compression cut inference token usage by 66%.

### DefinitelyNotGPT: a small language model from scratch

<p class="role__meta">June – Sept 2025 · <a href="https://github.com/kallemanoop/DefinitelyNotGPT">code</a></p>

A decoder-only transformer written end to end: my own byte-level BPE tokenizer, RMSNorm, rotary position embeddings, multi-head attention with a KV cache, and mixed-precision training on a cosine schedule. Six layers, four heads, a 256-wide hidden state, about 7M parameters. It took over 12 hours to train on a Turing-generation GPU, and it talks with the confidence of a much larger model and the knowledge of a much smaller one.

What it taught me: the model is the easy part. Tokenizing a big corpus without running out of memory is where the character building happens.
