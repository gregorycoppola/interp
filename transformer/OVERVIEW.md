# The Transformer: A BP Perspective

A transformer is a machine that runs belief propagation
on an implicit factor graph. This directory explains each
component of the transformer architecture in terms of
what it does in that factor graph.

## The One-Sentence Version

Tokens are nodes. The residual stream is the belief state.
Attention is the gather step. The FFN is the update step.
One layer is one round of BP. L layers are L rounds.

## The Architecture

A transformer takes a sequence of W tokens as input.
Each token becomes a D-dimensional vector — its belief
state. The sequence is processed by L identical layers,
each consisting of an attention block followed by an FFN
block. The output is a new D-dimensional vector at each
position.

## The BP Reading

At initialization, each token holds a prior belief — its
embedding. At each layer, attention gathers evidence from
neighboring tokens and writes it into the residual stream.
The FFN reads the gathered evidence and computes an updated
belief. This is one round of BP. After L layers the beliefs
have been updated L times — L rounds of inference.

The weights define the implicit factor graph. The
attention weights define the edges — which tokens
influence which. The FFN weights define the factor
potentials — how evidence combines into updated beliefs.

## The Files

    TOKENS.md           what a token is and what W means
    RESIDUAL_STREAM.md  the D-dimensional belief state
    ATTENTION.md        the gather step
    FFN.md              the update step
    LAYERS.md           one layer = one round of BP
    DIMENSIONS.md       W, D, H, D_ff, L explained
    WEIGHT_SHARING.md   why patterns differ from evaluations
    SOFTMAX.md          the three softmaxes
    SCALING.md          how the factor graph grows