---
permalink: /work/
title: "Work"
excerpt: "Founding AI automation engineer at a personal injury law firm, plus earlier research, engineering and community work."
layout: single
author_profile: true
---

One big thing, a few earlier chapters, and some opinions about how to work.

## Founding AI Automation Engineer

<p class="role__meta">A 25-person personal injury law firm · May 2026 – present</p>

Paralegals were spending their days reading hundreds of pages per case and retyping facts into the firm's case-management system, one field at a time. I designed and built the system that does the reading: alone for the first three months, then with a teammate.

It does three jobs:

- **Draft** the firm's demand letter from every document in a case. The model writes the prose; Python does the totals, the medical codes and the exclusions, because arithmetic should not be a vibe.
- **Answer** questions about the evidence with citations, through an agent that decides per question whether to answer directly, run one search, or go read whole documents.
- **Autofill** the case-management system (29 sections, 920 fields) with propose-then-approve: every value arrives with a verbatim quote, and nothing is written until a person approves it and the live record has been re-checked.

It never states fault or liability and never proposes a demand figure. Those are attorney calls, and the guardrails that enforce this are code, not just a sentence in a prompt.

<div class="metrics">
  <div class="metric"><span class="metric__value">≥5×</span><span class="metric__label">faster ingestion</span></div>
  <div class="metric"><span class="metric__value">25 → 1–3</span><span class="metric__label">confident-wrong values per scan, on our test case</span></div>
  <div class="metric"><span class="metric__value">38.9 s</span><span class="metric__label">vs 40.0 s after merging a teammate’s engine</span></div>
  <div class="metric"><span class="metric__value">260+</span><span class="metric__label">tests, all offline</span></div>
</div>

[Read the lawMCS AI case study →](/work/lawmcs-ai/){: .btn .btn--primary}

## Before that

### Research Intern, GITAM

<p class="role__meta">Gandhi Institute of Technology and Management, India</p>

Ran a national-scale analysis of retracted research papers, mentored by [Dr. Prem Kumar Singh](https://scholar.google.com/citations?user=FFmAj_MAAAAJ&hl=en).

### Salesforce Apprentice, PwC Acceleration Centers

<p class="role__meta">India</p>

Worked on Salesforce data models, Apex, Java and a lot of SQL. Learned early that the schema is the product and everything else is a view of it.

### Vice President, ACM GITAM Student Chapter

Mentored 100+ students in ML, MLOps and NLP, and ran events including ACM Career Compass, Parichay, Introduction to LLMs, Decoding Emotions (an NLP workshop) and Xplore IoT.

### Content Lead, Meta Developer Circles GITAM

Wrote technical content and workshops on AR/VR, Python and full-stack systems.

## How I like to work

- **I'd rather own a problem than a layer of it.** The messy input, the model, the UI, and the number that says whether any of it works.
- **I measure before I guess.** Most of my best fixes started with "wait, why is that distribution flat?"
- **I cut things.** A task queue, containers for the app and fine-tuning all got cut from the case system once the numbers said they weren't earning their keep.
- **Users are the spec.** At the firm, paralegals' approve and reject clicks became the answer key.
- **I write things down.** Handoff docs, invariants, the reason a decision went the way it did. Future me is a stakeholder too.

## Toolbox

Mostly Python and PyTorch. C++ when it has to be fast, SQL when it has to be true, TypeScript when someone has to click it. Postgres (with pgvector) is my default answer to storage questions, LangGraph comes out when something genuinely needs to be a state machine, and Langfuse is how I find out what the models actually did.
