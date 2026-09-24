---
layout: single
title: "Hi, I'm Anoop."
excerpt: "I build LLM systems for places where being confidently wrong is expensive, and I take language models apart to see how they make up their minds."
author_profile: true
---

<p class="lede">I build LLM systems for places where being confidently wrong is expensive, and I take language models apart to see how they make up their minds.</p>

Right now I'm the founding AI automation engineer at a personal injury law firm. I built the system that reads an entire case file (scans, phone photos, emails with zips inside, PDFs held together by hope) and turns it into cited drafts and field-by-field proposals that a paralegal approves with a click. I'm also finishing an MS in Data Science at the University of Maryland, which explains why my sleep schedule shows volatility clustering.

Before that, I wrote a paper on where ethical decisions live inside base language models (short answer: late-layer MLPs, and the models don't always act on what they seem to know). I also trained a tiny GPT from scratch because I wanted to see where the magic was. Mostly in the tokenizer, which is also where most of the out-of-memory errors were.

## The short version

<div class="grid">
  <div class="card">
    <span class="kicker">01</span>
    <h3>What I think about</h3>
    <ul>
      <li><strong>The gap between what a model knows and what it does.</strong> In my research, the model with the strongest internal ethical signal still chose at chance. At work I see the production version: the right value is sitting in the context, and the model confidently writes a different one.</li>
      <li><strong>Evaluation when nobody hands you labels.</strong> Real problems rarely arrive with a benchmark. They arrive with a pile of past human decisions, some of which were wrong.</li>
      <li><strong>Where rigor should live.</strong> In the prompt, in the weights, or in forty lines of boring Python. (Usually the Python.)</li>
      <li><strong>How small a model can be</strong> before it stops being useful, if the scaffolding around it is honest.</li>
    </ul>
  </div>
  <div class="card">
    <span class="kicker">02</span>
    <h3>What I try to solve</h3>
    <ul>
      <li><strong>Mess in, structure out.</strong> Scans, photos, broken PDFs and emails-inside-zips turned into something a model can reason over, with every fact still traceable to a page.</li>
      <li><strong>Agents that touch real systems without breaking them.</strong> Propose, ground it in a quote, re-check the live record, then write. In that order, every time.</li>
      <li><strong>“It feels better” into a number with an error bar</strong>, plus a gate that fails the build when the number gets worse.</li>
      <li><strong>The last mile.</strong> The part after the demo works, where the tail latency and the weird edge cases live.</li>
    </ul>
  </div>
  <div class="card">
    <span class="kicker">03</span>
    <h3>What I’m solving right now</h3>
    <ul>
      <li><strong>Driving down “confident-wrong”:</strong> wrong values that look ready to approve, without quietly trading away recall. The current knob is how much to trust a document depending on who wrote it.</li>
      <li><strong>A verifier for extracted rows</strong>, so that “a patient is not their own doctor” is enforced by code rather than optimism.</li>
      <li><strong>Growing an answer key past its own blind spots.</strong> An eval seeded from a system’s past outputs can’t see what the system never found.</li>
      <li><strong>The research sequel:</strong> taking the ethics work from base models to instruction-tuned ones, to see what alignment actually rewires on the inside.</li>
    </ul>
  </div>
  <div class="card">
    <span class="kicker">04</span>
    <h3>What interests me</h3>
    <ul>
      <li><strong>Mechanistic interpretability:</strong> residual streams, attribution, and why so much decision-making piles up in late-layer MLPs.</li>
      <li><strong>LLMs as (strange) cognitive subjects:</strong> personality, conformity, and what “pressure” does to a next-token distribution.</li>
      <li><strong>Causal inference.</strong> I’d rather know <em>why</em> than <em>that</em>.</li>
      <li><strong>Data where the noise has structure:</strong> volatility, heteroskedasticity, and labels that are really just past decisions.</li>
      <li><strong>Post-training</strong>, and what it changes inside a model versus what it changes on the surface.</li>
      <li><strong>Building things from scratch</strong> to find out which parts are load-bearing.</li>
    </ul>
  </div>
</div>

## Lately

<div class="updates">
<ul>
  <li><b>Sep 2026</b> Shipped per-section extraction agents with real-time sync, then merged a teammate’s parallel scan engine into them without giving back any speed (38.9 s vs 40.0 s median).</li>
  <li><b>May 2026</b> Joined a personal injury law firm as its founding AI automation engineer.</li>
  <li><b>Feb 2026</b> Wrapped the paper on mechanistic interpretability of ethical reasoning across 10 base models.</li>
  <li><b>Jan 2026</b> Built KnowledgeX at NexHacks ’26 (CMU): a marketplace where the currency is what you know.</li>
  <li><b>Fall 2025</b> Started the MS in Data Science at UMD. Go Terps.</li>
  <li><b>Summer 2025</b> Graduated in Computer Science and Engineering from GITAM and trained DefinitelyNotGPT on one very patient GPU.</li>
</ul>
</div>

Want the long version? There's a [case study of lawMCS AI](/work/lawmcs-ai/), the law firm system, a write-up of [the research](/research/), and a page of [bugs that taught me something](/notes/).
