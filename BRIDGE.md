# The Bridge: Transformer Components and BP

A one-page correspondence table. Every transformer
component on the left. Its BP counterpart on the right.

## The Correspondence

    TRANSFORMER                     BELIEF PROPAGATION

    token position                  variable node in factor graph
    W token positions               n nodes in the factor graph
    D-dimensional residual stream   D beliefs at one node
    token embedding                 prior belief (initialized to 0.5)

    attention head                  gather step
    Q/K weights                     routing key — which neighbor to look at
    V weights                       value to copy from the neighbor
    softmax over W positions        content-based neighbor selection
    sharp attention                 exact routing to one specific neighbor
    diffuse attention               soft routing — open BP question
    H heads per layer               H independent gathers per round
    H * W AND evaluations / layer   H gathers per node per round

    residual stream after attention scratch space — gathered evidence waiting
    all H heads written before FFN  AND gate — simultaneity enforced

    FFN hidden layer                OR nodes
    FFN hidden unit                 one OR node — one concept detector
    sigmoid activation              updateBelief — exact Bayesian update
    D_ff hidden units               D_ff OR gates per node per round
    W1 weight matrix                keys — patterns to match against
    W2 weight matrix                values — belief updates to apply

    one transformer layer           one round of belief propagation
    L layers                        L rounds of BP
    layer depth L                   reasoning depth — max inference chain length

    attention weights A[t,t']       edge weights in the implicit factor graph
    FFN weights W1, W2              factor potentials
    all weights W                   the implicit factor graph G(W)

    forward pass                    pi messages — forward inference
    (no backward pass)              lambda messages not computed
    L forward passes (chain of thought)  L * depth rounds total

## The Key Numbers (Llama 3 405B, W=8192)

    distinct AND patterns:   L * H       = 16,128
    distinct OR patterns:    L * D_ff    = 6,709,248  (6.7M)
    AND evaluations / pass:  L * H * W   = 132M
    OR evaluations / pass:   L * D_ff * W = 55B
    OR / AND ratio:          D_ff / H    = 416x

## The Key Theorems

    every sigmoid transformer is a Bayesian network
        — sigmoid-transformer-lean, formally verified

    transformer implements exact BP with constructed weights
        — transformer-bp-lean, formally verified

    exact posteriors force BP weights uniquely
        — sigmoid-transformer-lean, formally verified

    BP is exact on trees
        — hard-bp-lean, formally verified

    N rounds are necessary for N-hop reasoning
        — pearl repo (Peirce lower bound), formally verified

## What Is Covered and What Is Open

    COVERED: sharp attention heads — previous token, induction,
             name mover, retrieval, duplicate token
             All are exact implementations of the BP gather step.

    PARTIAL: S-inhibition (negative messages not formally developed)
             positional heads (positional routing not characterized)
             redundant heads (k-ary OR connection not stated)

    OPEN:    function vector heads — diffuse attention at scale
             No clean BP interpretation. Main open problem.

## The One-Sentence Summary

A transformer is a factor graph. Attention is AND.
FFN is OR. One layer is one round of BP. The weights
are the graph. The forward pass is the inference.