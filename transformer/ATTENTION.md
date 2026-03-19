# Attention: The Gather Step

## What Attention Does

Attention is the gather step of BP. For each token
position t, it asks: what information from other
positions do I need, and how do I get it?

Each attention head answers this question independently.
Head h at layer l computes:

    q_t = W_Q * x_t          (query: what am I looking for?)
    k_{t'} = W_K * x_{t'}    (key: what does each position have?)
    v_{t'} = W_V * x_{t'}    (value: what to copy if matched)

    score(t, t') = q_t · k_{t'} / sqrt(D/H)
    weights = softmax(scores over all t')
    output_t = sum over t' of weights[t'] * v_{t'}

The output is a weighted mixture of value vectors from
all W positions, with the attention weights determining
how much each position contributes.

## The Q/K/V Split

Q and K are used for routing — deciding where to look.
V is used for retrieval — deciding what to copy once
the right position is found.

In the formal BP construction:
    W_Q = W_K = projectDim(d)    route by stored index
    W_V = crossProject(s, d)     copy dimension s to dimension d

In a trained model the weights are learned rather than
constructed, but the same functional split applies:
Q and K implement content-based routing, V implements
content-based retrieval.

## Sharp vs Diffuse Attention

Sharp attention: the softmax concentrates weight on one
or a few positions. The head is doing a focused lookup —
retrieving one specific neighbor's belief. This is the
direct analog of the formal BP gather step.

Diffuse attention: the softmax spreads weight across many
positions. The head computes a weighted average of many
neighbors' beliefs. This does not map cleanly onto the
formal BP gather step. See FUNCTION_VECTORS.md in the
future_work directory.

Empirically: sharp heads (previous token, induction,
name mover, retrieval) are fully covered by the BP
framework. Diffuse heads (function vectors) are the
main open problem.

## H: The Number of Heads

H attention heads run in parallel at each layer, each
asking a different question. Each head has its own
W_Q, W_K, W_V matrices. The outputs of all H heads
are concatenated and projected back to D dimensions.

    H = 32   for Llama 3 8B
    H = 128  for Llama 3 405B

H is the number of independent gathers per token
per layer. In BP terms: the number of distinct
neighbor lookups the model can perform simultaneously
at each round.

The H outputs together fill the D-dimensional residual
stream: H heads * (D/H) dimensions per head = D.

## Distinct AND Patterns

Because attention weights are shared across token
positions — the same W_Q, W_K, W_V matrices apply
at every position — the number of distinct learned
routing patterns is:

    L * H = distinct AND weight patterns

For Llama 3 405B: 126 * 128 = 16,128 distinct AND
patterns. The same 16,128 routing questions get asked
at every one of the W = 8,192 token positions, giving
L * H * W = 132M AND evaluations per forward pass.

16,128 is surprisingly small. The transformer has a
constrained routing vocabulary — a small set of
distinct ways of asking "where should I look" —
applied densely across all positions and layers.

## The W² Cost

Every head computes a score between every pair of
positions — W² dot products per head per layer. For
W = 8,192 and H = 128, L = 126:

    attention FLOPs ≈ 2 * W² * D * L = 277 trillion

This is the dominant cost of a forward pass. It grows
quadratically with W, which is why long context is
expensive and why efficient attention variants matter.