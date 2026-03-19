# Superposition

## What the Literature Found

Elhage et al. (2022) showed that neural networks represent
more features than they have neurons by encoding features
as non-orthogonal directions in activation space. This is
superposition.

A D-dimensional residual stream can represent far more than
D features. The number of possible nearly-orthogonal
directions in D dimensions grows roughly as exp(D) — an
astronomically large number. In practice only a sparse
subset of directions are used, but the number is still
much larger than D.

The consequence is polysemanticity: individual neurons
respond to multiple unrelated concepts, because multiple
features are superposed in the same dimensions. A single
FFN hidden unit might fire on "banana," "yellow things,"
and "tropical locations" — three different features sharing
the same neuron.

SAEs decompose the superposed activations into the
underlying monosemantic features by finding sparse
overcomplete dictionaries — more directions than dimensions.

## What This Means in BP Terms

Superposition is the key challenge for the BP interpretation
of real transformers.

The formal BP construction assumes clean separation: each
residual stream dimension corresponds to one belief, each
attention head reads one specific dimension, each FFN
hidden unit updates one specific belief. This works at
D=1 and generalizes cleanly to D independent scalar BP
computations if the dimensions are independent.

Superposition breaks the independence assumption. The D
dimensions are not tracking D independent propositions —
they are tracking a much larger number of propositions
in superposition. The FFN weight matrices are not updating
D independent beliefs — they are computing something
in a superposed space where the features are entangled.

This means the implicit factor graph is much larger and
denser than the D * W node count suggests. The true node
count is the number of superposed features — potentially
millions per layer — and the edges between them are
encoded in the weight matrices in a way that is not
directly readable.

The 2n² routing class theorem applies to the grounded case
where features are explicit and orthogonal. In the
superposed case, n is not D but the number of superposed
features, and the routing structure is implicit in the
weights rather than explicit in the token encoding.

## What It Confirms

That the BP interpretation of real transformers requires
accounting for superposition. That the true concept count
is larger than D. That the implicit factor graph has more
nodes than the residual stream has dimensions. That SAEs
are needed to recover the underlying factor graph structure.

## What It Leaves Open

Whether there is a clean BP account of superposition —
a version of BP that works on overcomplete non-orthogonal
feature sets. Whether the SAE decomposition recovers the
true implicit factor graph or an approximation. Whether
superposition is compatible with exact BP or only with
approximate inference. Whether the loopy BP approximation
is tight on the superposed factor graph.

## Key References

Elhage et al. 2022 — Toy models of superposition
Bricken et al. 2023 — Towards monosemanticity
Anthropic 2024 — Scaling monosemanticity