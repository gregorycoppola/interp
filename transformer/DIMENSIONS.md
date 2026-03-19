# The Five Dimensions

A transformer has five independent architectural
dimensions. Each has a precise meaning in BP terms.

## W — Sequence Length

What it is: the number of token positions in the input.

BP meaning: the number of nodes in the implicit factor
graph for one forward pass.

Typical values: 8,192 (Llama 3 pretraining), 131,072
(extended context).

Cost: attention scales as W². Doubling W quadruples
attention cost. Long context is expensive because the
factor graph grows and every node attends to every other.

Independence: W is fully independent of D, H, L, D_ff.
You can have any context length with any model size.

## D — Model Dimension

What it is: the width of the residual stream at each
token position.

BP meaning: the number of beliefs tracked simultaneously
per node. Each dimension is one proposition being
believed.

Typical values: 4,096 (8B), 8,192 (70B), 16,384 (405B).

Bottleneck: D is the bandwidth of the reasoning channel.
All information between layers must pass through D
dimensions. H and D_ff operate within layers but D
is what persists.

Independence: D determines capacity. Larger D means
more propositions tracked per position. Scales
parameter count quadratically via attention.

## H — Number of Attention Heads

What it is: the number of independent attention
operations per token position per layer.

BP meaning: the number of distinct neighbor lookups
per round. H different questions asked simultaneously
at each position.

Typical values: 32 (8B), 64 (70B), 128 (405B).

Head dimension: each head operates on D/H dimensions.
H heads * (D/H) = D — together they fill the stream.

Distinct patterns: L * H = total AND weight patterns.
For 405B: 126 * 128 = 16,128. Surprisingly small.
The routing vocabulary is constrained.

## D_ff — FFN Hidden Dimension

What it is: the number of hidden units in the FFN,
applied independently to each token position.

BP meaning: the number of OR nodes per token position
per layer. The number of distinct concept detectors
per round.

Typical values: 14,336 (8B, ≈ 3.5*D), 53,248 (405B,
≈ 3.3*D).

Distinct patterns: L * D_ff = total OR weight patterns.
For 405B: 126 * 53,248 = 6.7M.

Ratio: D_ff / H = 416 for 405B. There are 416 times
as many OR patterns as AND patterns. The architecture
is heavily weighted toward inference over routing.

## L — Number of Layers

What it is: the number of transformer blocks stacked.

BP meaning: the number of rounds of BP per forward
pass. The reasoning depth.

Typical values: 32 (8B), 80 (70B), 126 (405B).

Lower bound: L must be at least as large as the longest
reasoning chain in the tasks the model needs to solve.
The Peirce lower bound says this is a hard constraint —
no local algorithm can do N-hop reasoning in fewer
than N rounds.

## The Interaction

    W    nodes in the factor graph
    D    beliefs per node
    H    gathers per node per round
    D_ff OR updates per node per round
    L    rounds of BP

Total AND evaluations per pass: L * H * W
Total OR evaluations per pass:  L * D_ff * W
Total proposition slots:        L * W * D

For Llama 3 405B:
    AND evaluations: 132M
    OR evaluations:  55B
    Propositions:    16.9B