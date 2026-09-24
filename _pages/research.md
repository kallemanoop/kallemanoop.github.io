---
permalink: /research/
title: "Research"
excerpt: "Mechanistic Interpretability of Ethical Reasoning in Pre-Trained Language Models: A Multi-Tier Analysis of Compliance Under Psychological Pressure."
layout: single
author_profile: true
---

I like language models the way some people like old clocks: I want to open them up and find out which gear is doing what.

## Mechanistic Interpretability of Ethical Reasoning in Pre-Trained Language Models: A Multi-Tier Analysis of Compliance Under Psychological Pressure

<p class="role__meta">Manuscript · May 2025 – Feb 2026</p>

**The question:** what ethical priors do language models pick up in pre-training, before any alignment touches them, and where in the network do those priors live?

Most work answers half of that. Behavioral evaluations tell you *what* a model does without explaining how. Mechanistic work finds active circuits without showing that they drive behavior. I built a framework that does both and ran it on 10 base models, from 124M to 14B parameters across five families.

1. **Psychometric profiling.** Big Five and Short Dark Triad inventories, answered through constrained, logit-masked generation: one forward pass per item, with every token except the five valid Likert options masked out. That gives 100% valid responses and the full distribution over the scale instead of a parsed guess.
2. **Compliance under pressure.** 680 forced-choice ethical dilemmas across 8 pressure types (authority, loyalty, self-interest, survival, empathy, fairness, duty conflict and social), scored with two metrics I introduced:
   - **Choice Rationality Score (CRS)** weights failures by severity, so obeying an order to cause harm counts for more than bending a privacy rule.
   - **Moral Risk Exposure (MRE)** measures how often a model complies when serious harm is on the table.
3. **Looking inside.** Attribution patching, per-head direct logit attribution, logit lens and layer-wise entropy to locate the computation, then causal MLP ablation to check that it actually matters.

### What turned up

- **Accuracy was the wrong lens.** Eight of the ten models were statistically indistinguishable from a coin flip. CRS still pulled them apart: 8 of 10 lean toward harmful compliance, and the two largest (Phi-4 and Qwen3-14B) comply with high-severity violations more than half the time.
- **Authority and loyalty are the pressures nobody resists.** They're the only two where every single model's CRS is negative (Kruskal–Wallis H = 37.46, p < 10⁻⁵). My guess is the training data, where authority reads as a legitimate source of instructions and loyalty reads as a virtue.
- **Ethics lives late, and in the MLPs.** The attention-to-MLP attribution ratio is below 1.0 in all 10 models, and 9 of 10 concentrate attribution in the final third of the network. Zeroing those late MLPs flips the ethical choice in up to 70% of prompts (23.6% on average).
- **Recognition is not action.** AFM had the strongest late-layer signal, the sharpest entropy "crystallisation" and the most agreeable personality profile in the set, and it behaved at exactly chance. It seems to know which option is right and doesn't reliably say so. That gap is the thing I keep coming back to, in research and in production.

### What I'd do next

- Run the same framework on instruction-tuned models, to see what alignment changes inside and not just in the outputs.
- Replace whole-layer ablations with feature-level interventions, to test whether ethical priors can be edited the way factual associations can.
- Add test–retest reliability to the psychometric tier, which I currently treat as exploratory rather than as a trait measurement.
