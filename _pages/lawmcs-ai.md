---
permalink: /work/lawmcs-ai/
title: "lawMCS AI"
excerpt: "An LLM system that reads a whole personal-injury case file, drafts, answers with citations, and proposes values for a person to approve."
layout: single
author_profile: true
toc: true
toc_label: "On this page"
toc_sticky: true
---

<p class="lede">An LLM system that reads an entire personal-injury case file and proposes. It never decides.</p>

<p class="role__meta">Lead engineer · 2026 · solo from June to August, then with one teammate</p>

## The problem

Before a demand goes to an insurer, a paralegal reads everything in the case: police reports, medical records and bills, photos, wage documents, insurer letters. Then they retype the facts into the firm's case-management system, field by field. At a 25-person firm, that is where the hours go.

## Why it's harder than it sounds

- **Input can be anything.** Text PDFs, scanned PDFs, broken PDFs, PDFs that are half of each, phone photos, spreadsheets, and emails with attachments that have attachments. Some records run to hundreds of pages.
- **Output has to be complete.** A demand letter that silently drops a bill is worse than no automation at all.
- **Some output is off limits.** Fault, liability and settlement figures are attorney judgments. The system describes evidence and never draws legal conclusions.
- **The target is a live system of record.** 29 sections and 920 fields: dropdowns with allow-lists, dates, currency, links to people, repeating rows.
- **No labels, and the data can't leave.** Nothing can be sent to annotators or used for training.
- **A budget.** A small, fast hosted model and no GPU. Quality had to come from structure, not parameter count.

## The shape of it

```text
upload or webhook
    │
    ▼
router: sniff the real file type (magic bytes, not extensions)
    ├─ documents ──▶ per page: text layer, or render it for vision
    ├─ images ─────▶ vision model
    └─ zip / email ▶ unpack, then back through the router
    │
    ▼
extracted content (every line remembers its file and page)
    │
    ├─▶ DRAFT     map-reduce over every document ─▶ typed sections
    │             ─▶ Python assembles the letter
    ├─▶ ASK       agent picks: answer / one search / read documents
    │             ─▶ cited answer
    └─▶ AUTOFILL  brief ─▶ route to section agents ─▶ extract + quote
                  ─▶ ground ─▶ merge ─▶ match rows ─▶ diff vs live record
                  ─▶ person approves ─▶ re-check ─▶ write ─▶ read back
```

Built with Python, FastAPI, Postgres with pgvector, LangGraph, hosted Qwen models for text, vision and embeddings, Langfuse tracing with PHI masked by default, and a React + TypeScript front end.

## Seven ideas that did the heavy lifting

### 1. Fix the shape, free the content

The model decides what to say. Typed schemas and deterministic gates decide what is allowed to exist. The model never computes a total, picks an attachment or invents a billing code. Codes are format-checked (a bad one is dropped, its description kept and flagged), totals are summed in Python and labeled as sums of recorded amounts, and the liability and demand sections stay as placeholders for an attorney.

### 2. Quote or it didn't happen

Every extracted value has to come with a verbatim quote from the document. If the quote isn't in the text, the value is dropped, and it's dropped *before* merging across chunks. Otherwise one hallucination can manufacture a fake disagreement with a correct value and knock it out of the running.

### 3. Never silently resolve a disagreement

When two documents (or two parts of one) disagree, the engine doesn't vote. The field goes to a person with a note saying the sources disagree. Majority vote, recency and confidence scores all hide uncertainty in exactly the cases where it matters.

### 4. Propose, approve, re-check, write, read back

The engine never writes on its own authority. An existing value that differs becomes a conflict, never an overwrite. On approval it re-reads the live record, and if anything changed since the paralegal looked, it doesn't write. After writing, it reads the value back and compares. Dropdowns accept only allow-listed values, and a link to a person only resolves to a contact that already exists.

### 5. Entity resolution with keys and vetoes

One insurance policy should be one row, however many letters mention it. Each row type gets identity **keys** (share a complete one and you're the same thing) and **vetoes** (differ on one and you never are). A single claim can hold several coverage rows, so coverage type is a veto. No complete key means no merge: a duplicate is cheaper than two records fused into one.

### 6. Agents as specs, not as calls

"One agent per section" exists as specs: a schema, allow-lists, rules, a write shape and a routing entry. All the specs routed to a chunk are combined into one model call, grouped under section headings. That keeps the accuracy of section-scoped instructions at single-call cost. Separate calls would have cost roughly 1.5–2× the tokens and blown the latency budget.

### 7. Routing beats retrieval for broad questions

Top-k retrieval falls apart on "tell me about this case": nothing anchors the similarity search, so it returns email boilerplate at a flat ~0.5 cosine. The assistant is a LangGraph state machine that picks a strategy per question: answer directly, run one search, or list and read whole documents. Specific questions take about 6 s. Broad ones take about 18 s and come back cited, where before they couldn't be answered at all. Conversation state is checkpointed in Postgres.

## Measuring without labels

There's no benchmark for "did we fill this law firm's case file correctly," and the data can't leave the building. So I built an offline eval harness:

- **Answer keys from human decisions.** Values paralegals approved, or that were already in the record, are positives. Values they rejected are negatives, unless the same value was approved elsewhere for the same field (then it was a rejected duplicate, not a wrong answer). It's weak supervision, and it's labeled as such.
- **A headline metric for the failure that hurts:** *confident-wrong*, a wrong value shown as ready to approve. A blank costs a paralegal a few seconds. A confident wrong value costs a correction in a legal record. Precision, recall and deferral rate are reported alongside it.
- **Replayable and cheap.** A recorded, read-only snapshot of the case-management API, plus a model-response cache keyed on a hash of the model and prompt. Unchanged prompts rerun for free, and offline mode refuses any uncached call, so ablations run on *identical* model answers.
- **Variance, not vibes.** Fresh samples per run, always reported as ranges, and a no-regression gate that fails when confident-wrong goes up.
- **A known limit.** The key is seeded from what the engine proposed before, so it can't see fields the engine never found. Recall is relative. The harness catches regressions well; a paralegal review pass will close the gap.

<p class="aside-note"><strong>An experiment I like.</strong> Most of the remaining wrong values came from letters the firm had written itself: summary counts that disagreed with the underlying bills, a job title lifted from a sentence about duties. Those letters are secondary sources about primary evidence, so rows sourced only from firm-authored letters now require review. On identical model answers across three samples, confident-wrong went from 6 / 3 / 2 to 3 / 1 / 2, and recall fell from 44–48% to 29–37%. That’s a trade, and I made it on purpose.</p>

## Numbers

<div class="metrics">
  <div class="metric"><span class="metric__value">≥5×</span><span class="metric__label">faster ingestion (a 100-file case used to take 5–7 min)</span></div>
  <div class="metric"><span class="metric__value">~5.5 → ~3 min</span><span class="metric__label">model time per scan, once it stopped writing “N/A” hundreds of times</span></div>
  <div class="metric"><span class="metric__value">47 s → 5 s</span><span class="metric__label">agent reply on questions that used to send it into a loop</span></div>
  <div class="metric"><span class="metric__value">25 → 1–3</span><span class="metric__label">confident-wrong values per scan, on our test case</span></div>
  <div class="metric"><span class="metric__value">38.9 s</span><span class="metric__label">median reading time after the merge, vs 40.0 s before (cold cache)</span></div>
  <div class="metric"><span class="metric__value">260+</span><span class="metric__label">tests, all offline (fake model, SQLite)</span></div>
</div>

## Working with a teammate

A teammate joined in late August and built a parallel scan engine that took 13 documents from about 25 minutes serial to 2 min 19 s. I merged it with my section-agent design under two rules: their tests pass unchanged, and their speed doesn't regress.

I dry-ran the merge to count real conflicts (3 of 27 files), rebuilt the biggest one on top of *their* version so their structure held, wrote down the eight invariants that made their numbers real, and checked each one. Combining the two turned up six defects, including a cache that re-coerced already-coerced values (so a date with no year came back as a confident date) and a refresh that read repeating rows as if they were forms. Each fix has a test that fails without it. The first live benchmark still missed the target, the logs showed why (it's on the [notes page](/notes/)), and the re-run came in at a median 38.9 s against their 40.0 s.

## Things I cut

- **A task queue and broker.** At most 25 users and network-bound calls. Threads were enough, and a broker was operational weight with no payoff.
- **Fine-tuning.** The limit was the prompt, not the weights. The hosted model can't be fine-tuned anyway, and training on privileged client data is a hard no.
- **Mirroring the case-management system.** The firm holds over a million documents. Instead of a twin that has to stay in sync, the system became an engine that reads from the record and writes back to it.
- **Reasoning mode.** "Low" reasoning gave the same recall at roughly ten times the runtime. It stays off, by measurement.

## What I'd do differently

Build the eval harness in August, not late September. Several weeks of accuracy work were judged by eye before there was a number, and my eye was wrong more often than I'd like.

## What I took away

- Structure beats prompting, and it carries over to the next model for free.
- Measure before guessing. A flat score distribution is a bug, not a property of your data.
- A wrong value that looks ready is worse than a blank.

<p class="aside-note">This write-up stays at the level of architecture, methods and aggregate numbers. The code and all case data belong to the firm, and none of it appears here.</p>
