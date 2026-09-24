---
permalink: /notes/
title: "Field Notes"
excerpt: "Short write-ups of bugs and measurements that taught me something."
layout: single
author_profile: true
toc: true
toc_label: "Notes"
toc_sticky: true
---

Short write-ups of problems that were more interesting than they had any right to be. Most come from building [lawMCS AI](/work/lawmcs-ai/). Each one follows the same shape: what I saw, what I measured, what fixed it, and what I'd tell past me. Details are generalized, and no case data appears here.

### The embeddings that arrived out of order

- **Saw:** Retrieval felt random. Tiny OCR fragments outranked a 162-chunk letter, and every cosine score sat in a flat 0.49–0.55 band.
- **First guess:** The Postgres migration I had just finished broke something.
- **Measured:** Dumped top-k with scores and snippets, then checked the embedding model on its own: identical text scored 1.0, related text 0.45, unrelated text 0.21. The model was fine.
- **Cause:** The provider returned multi-item embedding batches out of order, and my code read them in arrival order, so every chunk got someone else's vector. It only happened with multiple batches, which is why small cases looked fine.
- **Fix:** Sort by the returned index, add a regression test, re-embed everything.
- **Lesson:** A flat score distribution means your text and your vectors have stopped talking to each other. Measure before blaming the database.

### 90 of 104 scanned pages never became text

- **Saw:** The assistant couldn't "see" most of a case that was heavy on scans.
- **Cause:** The image-description pass ran one image at a time and hit its time budget after about 14.
- **Fix:** Concurrent vision calls, deduplicated by image bytes. 86 newly described images later, a question about vehicle damage pulled from the police report and the photos together.
- **Lesson:** A timeout that fails quietly looks exactly like a model that isn't smart enough.

### The gibberish filter that ate a bill

- **Tried:** Dropping OCR garbage with alphanumeric ratios, vowel ratios and function-word heuristics.
- **Result:** Every version flagged real evidence, including the one line on a medical bill that held the charges.
- **Fix:** Reverted. Keep every piece of evidence and fix extraction instead, at the page level: detect a corrupt text layer and send that page to the vision model.
- **Lesson:** When completeness matters, a filter's false positives are the expensive kind.

### Paying for thoughts, receiving nothing

- **Saw:** With hidden reasoning on, a 400-token budget spent 400 of 402 tokens thinking and returned an empty answer in 3.0 s. With reasoning off: 0.9 s, 38 tokens, valid output. The hidden thinking tokens were still billed.
- **Then measured:** "Low" reasoning on the eval gave the same recall (~45%) at 9–10 minutes per run, versus under a minute without it.
- **Decision:** Off by default. A model or mode change is now a config edit with a measured baseline to beat.
- **Lesson:** Thinking is a line item. Price it like one.

### The agent that couldn't stop planning

- **Saw:** On confusing meta-questions, the agent leaked its chain of thought and repeated the same planning paragraph until it hit a limit: 47 s and 23,000 characters.
- **Cause:** The agent framework's model client bypassed my client's reasoning handling.
- **Fix:** Exclude reasoning in the request, cap tokens, add a light frequency penalty (0.3), and strip think-tags from the stream.
- **After:** 5 s and 274 characters.

### "Not stated," 908 times

- **Saw:** After merging two engines, the first live benchmark read 43 / 63 / 66 s against a 40 s target.
- **Measured:** The logs showed two things. Contact lookups were running one after another (a 16–18 s tail), and the model was dutifully writing "Not stated" or "N/A" for every absent field: 385 to 908 placeholder values per scan, which doubled output tokens.
- **Fix:** Ask the model to omit absent fields, drop placeholders in the reader anyway, and run four lookups concurrently.
- **After:** A median of 38.9 s against the 40 s target, and model time per scan fell from about 5.5 minutes to about 3.
- **Lesson:** Models are very polite about things that aren't there, and politeness costs tokens.

### The API that said 200 and meant nothing

- **Situation:** Parts of the case-management API were undocumented, including the exact shape for linking a field to a person.
- **Measured:** A bare integer worked. One plausible object shape returned a 400. Other shapes returned a 200 and silently did nothing.
- **Rule:** Every unknown got settled with a reversible probe on a test case, never guessed on production data.
- **Lesson:** A 200 is a claim, not a receipt. Read the value back.

### The column that wouldn't leave

- **Saw:** A paralegal's case list broke.
- **Cause:** I had removed a required column from the data model, but the auto-migrator only ever adds columns. The column stayed in the live database and every insert failed. The test suite builds fresh tables, so it couldn't have caught this.
- **Fix:** An explicit list of retired columns that get dropped on migration.
- **Lesson:** Always ask what the live table actually looks like.

### Uploads that silently didn't sync

- **Cause:** The job runner coalesced autofill jobs per case, so a webhook that arrived mid-scan was simply dropped.
- **Fix:** A trailing re-run flag: anything that arrives during a scan triggers exactly one more.
- **Lesson:** Deduplication is a correctness decision, not only a performance one.
