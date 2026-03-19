# SAE Features in BP Terms

## What a Feature Is

An SAE feature is a direction in the residual stream
that fires on a specific concept and is silent otherwise.
It is discovered empirically by training a sparse
autoencoder on the residual stream activations of a
trained transformer.

In BP terms a feature is a candidate concept node in
the implicit factor graph. When feature i fires at
token position t with activation strength h, it means
concept i is active at position t with belief h.

## The Dictionary as Factor Graph Nodes

The SAE decoder matrix has F columns, one per feature.
Each column is a D-dimensional direction in residual
stream space.

These F directions are the empirical concept inventory —
the set of nodes in the implicit factor graph as
recovered from the weights. The SAE is not computing
new structure. It is making visible structure that
was always implicit in the residual stream.

In the grounded case, the factor graph nodes are
declared explicitly — each token has a routing key
(nodeType, ownIndex, neighborIndex) and the concept
count is exactly 2n². In the ungrounded case, the
SAE is doing empirically what grounding does formally —
establishing a finite set of named concepts that the
model's computation can be described in terms of.

## The Activation Pattern as Belief State

At each token position t and each layer l, the SAE
produces an F-dimensional sparse activation vector.
Entry i of that vector says how strongly feature i
is present in the residual stream at (t, l).

This is the belief state at node t in the implicit
factor graph, expressed in the SAE's feature basis
rather than the raw residual stream basis.

In the grounded BP construction, the belief state
at a node is a D-dimensional vector where each
dimension is one proposition. In the SAE basis, the
belief state is an F-dimensional sparse vector where
each non-zero entry is one active concept. The SAE
basis is sparser and more interpretable but
mathematically equivalent — both are representations
of the same residual stream vector.

## The Clamping Experiment as BP Manipulation

The Golden Gate Claude experiment clamped one SAE
feature to maximum activation throughout the forward
pass. In BP terms this is forcing one concept node's
belief to 1.0 and holding it there.

The result — every response routes through the Golden
Gate Bridge — is exactly what BP predicts. A node
with belief clamped to 1.0 sends maximum-confidence
messages to all its neighbors in the factor graph.
Every downstream computation receives overwhelming
evidence that the Golden Gate Bridge concept is active,
regardless of any other evidence in the context.

This is the most direct empirical demonstration that
SAE features behave like BP factor graph nodes. The
clamping is a surgical intervention on the belief
state. The result is the propagation of that belief
through the factor graph exactly as the BP equations
predict.

## Attribution Graphs as Factor Graph Edges

Attribution graphs (Anthropic 2025) trace how features
activate other features across layers. Feature A at
layer l activates feature B at layer l+1 with some
weight.

These cross-layer feature activations are the edges
in the implicit factor graph. If feature A at layer l
consistently causes feature B at layer l+1 to activate,
there is a directed edge from A to B with that weight
as the factor potential.

The attribution graph is the implicit factor graph
made visible. Nodes are SAE features. Edges are
attribution weights. The whole structure is the
G(W) from the shannon paper — recovered empirically
rather than declared.

## The 2n² Connection

In the grounded case the concept count is 2n² where
n is the number of declared factor graph nodes.
The routing key (nodeType, ownIndex, neighborIndex)
identifies each concept exactly.

In the ungrounded case the SAE dictionary size F is
the empirical proxy for 2n². The SAE is finding n
empirically — recovering the implicit node set from
the trained weights. Once you have F features you
can ask: what is the routing structure over them?
How many distinct (source feature, target feature)
pairs are there? That count is the empirical 2n².

The SAE does not give you 2n² directly. It gives you
n — the feature inventory. The routing structure over
those features is what the attribution graphs add.
Together: SAE features are the nodes, attribution
graphs are the edges, and the whole thing is the
empirically recovered implicit factor graph.

## Reconstruction Error and BP Approximation

The SAE does not perfectly reconstruct the residual
stream. There is always some reconstruction error —
the sparse feature representation misses some of the
variance in the original vector.

In BP terms this reconstruction error is a bound on
how well the SAE has recovered the implicit factor
graph. Perfect reconstruction would mean the F
features perfectly explain all the residual stream
variance — all the belief dynamics are captured
by the feature set. Reconstruction error means some
belief dynamics are not captured — there are concepts
or relationships the SAE has not found.

This connects the SAE engineering question (how to
minimize reconstruction error) to the BP theoretical
question (how complete is our recovery of the implicit
factor graph). Improving SAE reconstruction is the
same as improving our understanding of the factor graph.