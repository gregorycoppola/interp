# Tokens

## What a Token Is

A token is one position in the input sequence. It is the
atomic unit of the transformer's computation. In text,
tokens are word pieces — subword units from a vocabulary
of roughly 128,000 items. In the BP interpretation, each
token position is one node in the implicit factor graph.

## W: The Number of Tokens

W is the sequence length — how many token positions are
in the input. W is the number of nodes in the factor
graph for one forward pass.

    Llama 3 8B pretraining:  W = 8,192
    Llama 3 extended:        W = 131,072

Every token attends to every other token in the attention
step. The cost of this is W² per head per layer — the
dominant computational cost of the transformer.

## What a Token Knows

Each token position holds a D-dimensional vector in the
residual stream. That vector encodes everything the model
currently believes about that position — the original
token identity plus all updates from previous layers.

At the start of the forward pass, the vector is the
token embedding — a learned D-dimensional representation
of that word piece. By the end of L layers, it has
been updated L times by attention and FFN operations.

## Tokens as Factor Graph Nodes

In the BP interpretation, each token position is one
node. Its D-dimensional residual stream vector is its
belief state — D beliefs tracked simultaneously.

The attention mechanism defines the edges between nodes —
which positions influence which. The FFN defines the
factor potential at each node — how it combines incoming
evidence into updated beliefs.

The W tokens form a fully connected graph in principle —
every token can attend to every other token. In practice
the attention weights concentrate on a small subset of
positions, making the effective graph sparse.

## Tokens vs Concepts

Tokens are not concepts. A token is a surface form —
a word piece at a specific position. A concept is a
routing class — an inferential identity. The same concept
can appear at many token positions. The same token can
instantiate many different concepts depending on context.

The mapping from tokens to concepts is what grounding
accomplishes. A grounded token knows which concept it
instantiates. An ungrounded token holds a belief vector
whose conceptual identity is implicit in the weights.