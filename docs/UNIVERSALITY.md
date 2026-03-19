# Universality

## What the Literature Found

The universality hypothesis (Olah et al. 2020) proposes
that the same features and circuits emerge independently
across different models trained on different data with
different architectures.

Induction heads appear in every transformer language model
studied, regardless of size or training data. Curve
detectors appear in every vision model. The same small
set of named circuits — previous token heads, induction
heads, name mover heads — appear across GPT-2, LLaMA,
Pythia, and other models.

This is surprising if you think of neural networks as
arbitrary function approximators. It is less surprising
if the circuits are structural necessities — the only
way to implement certain computations locally given the
transformer architecture.

## What This Means in BP Terms

Universality is strong evidence that routing patterns
are structural necessities, not learned accidents.

In the BP framework, the routing structure of the implicit
factor graph is determined by what inference problems the
model needs to solve. If certain inference patterns —
find the previous occurrence of the current token, copy
the following value — are universal across models, it is
because those inference patterns are required by the
data distribution, not because they are arbitrary solutions
that happened to be found.

The BP account predicts universality: if the data
distribution requires certain inference chains, the optimal
local algorithm for those chains has a specific routing
structure, and gradient descent will find that structure
regardless of initialization. The circuits are universal
because the inference problems are universal.

This connects to the bayes-learner result: gradient
descent finds BP weights from scratch with no hints.
Universality is the same phenomenon at the circuit level —
gradient descent finds the same routing circuits from
scratch across many different training runs.

## What It Confirms

That routing structure is not arbitrary. That the same
inference problems require the same routing patterns.
That gradient descent reliably finds the correct routing
structure for a given inference problem. That the BP
account of what transformers are doing predicts rather
than merely describes the universality phenomenon.

## What It Leaves Open

Whether all circuits are universal or only a subset.
Whether universality holds at larger scales where function
vector heads dominate over induction heads. Whether the
factor graph structure of different models converges to
the same implicit graph or to different graphs that share
only their routing primitives.

## Key References

Olah et al. 2020 — Zoom in: An introduction to circuits
Olsson et al. 2022 — In-context learning and induction heads