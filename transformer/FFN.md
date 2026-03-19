# The FFN: The Update Step

## What the FFN Does

The FFN is the update step of BP. After attention has
gathered evidence from neighboring positions into the
residual stream, the FFN reads that evidence and computes
a new belief.

For each token position t independently:

    h = activation(W1 * x_t + b1)     expand to D_ff dimensions
    output = W2 * h + b2               project back to D dimensions

The FFN has no interaction between token positions —
it operates on each position's residual stream vector
in isolation. All the cross-position communication
happens in attention. The FFN is purely local.

## D_ff: The Hidden Dimension

D_ff is the number of hidden units in the FFN. It is
the number of OR nodes per token position per layer.

    Llama 3 8B:   D_ff = 14,336  (≈ 3.5 * D)
    Llama 3 405B: D_ff = 53,248  (≈ 3.3 * D)

Each of the D_ff hidden units computes one weighted
combination of the D input dimensions, applies a
nonlinearity, and contributes to the D-dimensional
output. In BP terms: each hidden unit is one OR node,
asking "does this particular combination of gathered
evidence support updating this belief?"

## The OR Gate

With sigmoid activation the FFN computes:

    output = sigma(W2 * sigma(W1 * x + b1) + b2)

The inner sigma(W1 * x + b1) computes D_ff soft
threshold operations — D_ff OR evaluations. The outer
operation combines them back into D belief updates.

In the formal BP construction with equal weights:

    updateBelief(m0, m1) = sigma(logit(m0) + logit(m1))

This is the exact Bayesian update for two independent
binary evidence sources. The FFN with BP weights
computes exactly this for each belief dimension.

## Weight Sharing Across Positions

The same W1 and W2 matrices apply to every token
position. The FFN does not know which position it
is operating on — it sees only the D-dimensional
residual stream vector and transforms it.

This means the D_ff hidden units are the same D_ff
OR detectors applied at every position. The number
of distinct learned OR patterns is:

    L * D_ff = distinct OR weight patterns

For Llama 3 405B: 126 * 53,248 = 6,709,248 ≈ 6.7M.
The same 6.7M concept detectors fire at every one of
the W = 8,192 token positions, giving L * D_ff * W
= 55B OR evaluations per forward pass.

## FFN as Key-Value Memory

Geva et al. (2021) showed that FFN hidden units function
as key-value memories. The first layer W1 is the key
matrix — each row is a pattern to match against.
The second layer W2 is the value matrix — each column
is what to write if the key fires.

In BP terms: the key is the routing pattern (which
combination of gathered evidence triggers this OR node)
and the value is the factor potential (what belief
update this evidence combination produces). The FFN
is a learned table of (pattern, update) pairs.

This is the factor potential in the implicit factor
graph. The FFN weights are the edge weights between
the gathered evidence and the updated belief. Editing
FFN weights (as in ROME) directly edits the factor
potential — changing the model's belief about a
specific conceptual association.

## SwiGLU vs Sigmoid

The formal BP proof requires sigmoid activation.
Production models including Llama use SwiGLU:

    SwiGLU(x) = SiLU(W1 * x) * (W3 * x)

The sigmoid is present inside SiLU but the gating
multiplication introduces a second linear projection.
The qualitative OR structure holds — SwiGLU still
computes soft thresholding — but the exact BP weight
construction does not directly apply.

The empirical result (val MAE 0.000752 in bayes-learner)
used ReLU and found near-exact BP weights anyway.
The exact proof requires sigmoid. The empirical result
suggests the structure is findable with compatible
activations.