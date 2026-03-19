# Factor Graphs

## What a Factor Graph Is

A factor graph is a specific way of drawing a Bayesian
network that makes the computational structure explicit.
It is a bipartite graph — two kinds of nodes, edges
only between nodes of different kinds.

Variable nodes represent the quantities you are
reasoning about. Each holds a belief — a probability
between 0 and 1.

Factor nodes represent relationships between variable
nodes. Each encodes a local function — a table of
weights saying how strongly different combinations
of its neighbors co-occur.

Edges connect variable nodes to factor nodes. A variable
node is connected to every factor that involves it.
A factor node is connected to every variable it relates.

## Why Bipartite

The bipartite structure separates two things that
should be separate: what you believe (variable nodes)
and how beliefs are related (factor nodes). Variables
hold beliefs. Factors encode relationships. Keeping
them separate makes the computation clean.

In the transformer: variable nodes are residual stream
dimensions — the beliefs. Factor nodes are attention
heads (AND) and FFN hidden units (OR) — the relationships.
The residual stream is variable. The weights are factors.

## The QBBN Structure

The QBBN (Quantified Boolean Bayesian Network) uses a
specific factor graph structure with two kinds of factor:

Conjunction factors (AND): all inputs must be present
before any conclusion is drawn. The factor fires only
when all its variable neighbors are true simultaneously.

Disjunction factors (OR): any input is sufficient to
support the conclusion. The factor fires when any of
its variable neighbors is true.

This AND/OR alternation is not arbitrary. It is the
minimal structure needed for complete boolean reasoning —
conjunction for assembling evidence, disjunction for
drawing conclusions. Pearl (1988) showed this structure
supports exact inference on trees. The shannon paper
shows the transformer implements exactly this structure.

## Pairwise Factors

The formal BP construction uses pairwise factors —
each factor connects exactly two variable nodes. This
is without loss of generality: any k-ary factor
decomposes into a chain of binary factors via
associativity of AND and log-odds additivity of OR.

Two attention heads per layer suffice for any factor
graph. More heads handle more evidence sources per
round, but even k-ary evidence reduces to binary
via the decomposition. Architectural width is fixed.
Only depth grows with reasoning complexity.

## Drawing the Transformer as a Factor Graph

Token positions are variable nodes. Their D-dimensional
residual streams hold D beliefs each.

Attention heads are AND factor nodes. Each head connects
to the source position (where it reads from) and the
destination position (where it writes to).

FFN hidden units are OR factor nodes. Each connects
to the D input dimensions and the D output dimensions
at the same position.

The graph has W variable node clusters, each with D
beliefs. L layers of AND and OR factors connecting
them. The whole structure is the implicit factor graph
G(W) from the shannon paper.