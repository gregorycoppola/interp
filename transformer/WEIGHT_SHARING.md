# Weight Sharing

## The Core Fact

FFN weights are shared across all W token positions
within a layer. Attention weights are shared across
all W token positions within a layer. Neither is
shared across layers.

This creates a gap between the number of distinct
learned patterns and the number of evaluations per
forward pass. The ratio between them is exactly W.

## The FFN

For layer l, there is one set of FFN weights W1_l
and W2_l. These same matrices apply to every token
position t:

    FFN_l(x_t) = W2_l * activation(W1_l * x_t)
    for all t in 1..W

The FFN does not know which position it is at. It
sees only a D-dimensional vector and transforms it.

Result:
    distinct OR weight patterns = L * D_ff = 6.7M  (405B)
    OR evaluations per pass     = L * D_ff * W = 55B

The 6.7M patterns fire W = 8,192 times each.

## The Attention

For layer l and head h, there is one set of weights
W_Q^{h,l}, W_K^{h,l}, W_V^{h,l}. These same matrices
apply to every token position.

Result:
    distinct AND weight patterns = L * H = 16,128  (405B)
    AND evaluations per pass     = L * H * W = 132M

The 16,128 routing questions get asked at every position.

## Why Sharing Across Positions

The FFN is position-agnostic by design. Position
information is encoded into the token's residual stream
vector via rotary embeddings before the FFN sees it.
The FFN computes the same transformation everywhere —
the position-specific content of the input supplies
the instance.

In BP terms: weight sharing is the implementation of
universal quantification. The same rule applies to
every entity. The weights are the rule. The token
position is the grounding. Sharing weights across
positions is applying the same rule to every instantiation.

## Why Not Sharing Across Layers

Each layer is one round of BP. Different rounds detect
different things — early layers detect surface patterns,
later layers detect abstract relationships. Sharing
weights across layers would force every round to use
the same detectors, collapsing the hierarchical
reasoning structure.

Recurrent networks share weights across steps because
they assume all steps are homogeneous. Transformers
do not share because the rounds are heterogeneous —
each builds on the previous.

## The Ratio

The ratio between OR evaluations and OR weight patterns
is W = 8,192. Every learned concept detector fires
8,192 times per forward pass — once per token position.

The ratio between AND evaluations and AND weight patterns
is also W = 8,192. Every learned routing question is
asked 8,192 times per forward pass.

The ratio between OR patterns and AND patterns is
D_ff / H = 416. There are 416 distinct concept
detectors per routing pattern. The architecture
invests heavily in concepts, lightly in routing.