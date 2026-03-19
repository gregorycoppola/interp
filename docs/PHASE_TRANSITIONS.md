# Phase Transitions

## What the Literature Found

Induction heads emerge suddenly during training — not
gradually but in a sharp phase transition. At a specific
point in training, the loss drops discontinuously and
induction heads appear simultaneously across the model.
Before the transition, no induction heads exist. After,
they are fully formed.

This phase transition coincides precisely with a sudden
improvement in in-context learning ability. The two events
— formation of induction heads and improvement in ICL —
happen at the same training step, suggesting a causal link.

Grokking (Nanda et al. 2023) is a related phenomenon:
models suddenly generalize on modular arithmetic tasks
after a long period of memorization, with a sharp
transition. The transition corresponds to the formation
of a specific circuit implementing the Fourier-based
modular arithmetic algorithm.

## What This Means in BP Terms

Phase transitions are the routing structure snapping into
place as a discrete event.

In the BP framework, the implicit factor graph is defined
by the weights. Before the phase transition, the weights
do not encode a coherent routing structure — the attention
heads do not implement clean gather steps, the FFN does
not implement clean belief updates. After the transition,
the weights snap into a configuration that implements
a specific inference algorithm.

This is consistent with the uniqueness theorem in the
shannon paper: exact posteriors force BP weights uniquely.
The training loss is pushing the model toward configurations
that produce correct predictions. At the phase transition,
the model finds the specific weight configuration that
implements the correct inference algorithm — and because
that configuration is unique (by the uniqueness theorem),
the transition is sharp rather than gradual.

The gradual improvement before the transition is the model
finding approximate solutions — weights that partially
implement the inference algorithm. The sharp transition
is the moment when the weights cross into the basin of
the exact solution. This matches the bayes-learner
empirical result: gradient descent finds BP weights
with val MAE 0.000752, not gradually converging but
finding the right structure from a random start.

## What It Confirms

That the routing structure of the implicit factor graph
is not arbitrary — there is a specific correct structure
and training finds it. That finding the correct structure
is a discrete event, consistent with the uniqueness
of the BP weight configuration. That the formation of
interpretable circuits and the improvement in task
performance are causally linked through the BP inference
structure.

## What It Leaves Open

Whether all capabilities emerge through phase transitions
or only those with unique correct implementations. Whether
the phase transition timescale depends on the complexity
of the implicit factor graph. Whether the transition can
be predicted from the factor graph structure before
training begins.

## Key References

Olsson et al. 2022 — In-context learning and induction heads
Nanda et al. 2023 — Progress measures for grokking