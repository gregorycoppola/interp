# Scaling: How the Factor Graph Grows

## What Scales With What

Each of the five dimensions contributes differently
to the size of the implicit factor graph and the
cost of the forward pass.

## Nodes in the Factor Graph

    proposition nodes per layer = W * D
    AND nodes per layer         = H * W
    OR nodes per layer          = D_ff * W

All three scale linearly with W — the context length.
All three also scale with their respective model
dimension (D, H, D_ff).

The OR nodes dominate. For Llama 3 405B at W=8192:

    propositions per layer:  134M
    AND nodes per layer:     1.05M
    OR nodes per layer:      436M

OR nodes outnumber AND nodes by D_ff / H = 416x.
OR nodes outnumber proposition nodes by D_ff / D = 3.3x.

## Distinct Weight Patterns

Weight patterns are shared across W positions:

    distinct AND patterns = L * H       = 16,128  (405B)
    distinct OR patterns  = L * D_ff    = 6.7M    (405B)

These are the learned routing vocabulary and concept
inventory respectively. 16K routing questions and
6.7M concept detectors, applied densely across all
positions and rounds.

## Parameter Count

Parameters scale with D quadratically through attention:

    attention params = 4 * D² * L
    FFN params       = 3 * D * D_ff * L  (SwiGLU uses 3 matrices)
    embedding params = vocab * D

For 405B: 135B attention + 330B FFN + 2B embeddings
= 467B total. FFN dominates — 70% of parameters.

## Compute Cost

    attention FLOPs = 2 * W² * D * L    (quadratic in W)
    FFN FLOPs       = 2 * W * D * D_ff * L  (linear in W)

Attention cost grows as W² — the W² dot products for
each head. FFN cost grows as W — it is applied
independently to each position. For short sequences
FFN dominates. For long sequences attention dominates.

## What Scaling Buys

More D: more beliefs tracked per position, richer
residual stream, more propositions per node. More
capacity for superposed concepts.

More H: more routing questions per round, more
diverse gather operations, richer AND structure.
But H saturates quickly — 16K distinct AND patterns
is already a constrained vocabulary.

More D_ff: more concept detectors per round, richer
OR structure, more conceptual capacity. This scales
the most usefully — 6.7M OR patterns vs 16K AND
patterns suggests OR is the binding constraint.

More L: more rounds of BP, deeper reasoning chains,
longer inference paths. Direct increase in reasoning
depth up to the Peirce lower bound.

More W: larger factor graph, more entities in scope,
richer context. But W² attention cost makes this
the most expensive dimension to scale.

## The Llama 3 Family

    model   D      H    L    D_ff    W       OR patterns   AND patterns
    8B      4096   32   32   14336   8192    459K          1024
    70B     8192   64   80   28672   8192    2.3M          5120
    405B    16384  128  126  53248   8192    6.7M          16128

The ratio D_ff / H stays roughly constant at ~416.
The ratio D_ff / D stays roughly constant at ~3.3.
Scaling increases both OR and AND patterns but keeps
their ratio stable — the architecture maintains the
same balance between routing and inference at every
scale.