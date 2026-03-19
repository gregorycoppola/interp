# interp

A meta-analysis of the mechanistic interpretability literature
through the lens of the Transformers are Bayesian Networks
framework (Coppola 2026, shannon repo).

The central question: what has mechanistic interpretability
actually found, and what does it mean in BP terms?

We do not run new experiments. We read the literature and
interpret it. Every finding gets two treatments: what the
authors said, and what it means if transformers are Bayesian
networks.

## Structure

    docs/INDUCTION_HEADS.md       sharp gather, BP routing confirmed
    docs/FUNCTION_VECTORS.md      diffuse gather, open BP question
    docs/NAMED_CIRCUITS.md        multi-round BP on subgraphs
    docs/RETRIEVAL_HEADS.md       grounding, hallucination connection
    docs/SAE_FEATURES.md          empirical concept inventory
    docs/SUPERPOSITION.md         why concepts exceed dimensions
    docs/UNIVERSALITY.md          routing patterns as structural necessities
    docs/PHASE_TRANSITIONS.md     routing structure emerging discretely
    docs/OPEN_QUESTIONS.md        what the literature has not answered

## The BP Lens

The shannon paper proves that every sigmoid transformer with
any weights implements weighted loopy belief propagation on
its implicit factor graph. One layer is one round of BP.
Attention is the gather step. FFN is the update step.

Mechanistic interpretability has been reverse engineering
transformers empirically without this framework. The question
this repo asks is: do the empirical findings confirm, extend,
or challenge the BP account? And what does the BP framework
predict that interpretability has not yet looked for?

## Key References

The literature this repo draws on:

Olsson et al. 2022 — In-context learning and induction heads
Elhage et al. 2021 — A mathematical framework for transformer circuits
Wang et al. 2022 — Interpretability in the wild (IOI circuit)
Todd et al. 2024 — Function vectors in large language models
Anthropic 2024 — Scaling monosemanticity (SAE features)
Anthropic 2024 — Golden Gate Claude
Geva et al. 2021 — Transformer FFN as key-value memories
Meng et al. 2022 — ROME (locating factual associations)
Anthropic 2025 — Circuit tracing and attribution graphs