# Open Questions: SAEs and BP

What the SAE literature has not yet answered,
framed in BP terms.

## 1. Does the SAE dictionary correspond to routing classes?

The 2n² theorem counts routing classes — distinct
(nodeType, ownIndex, neighborIndex) triples. SAE
features are directions in residual stream space.
These are not obviously the same thing.

A routing class is defined by how the attention
mechanism addresses a concept. An SAE feature is
defined by what pattern of residual stream activation
it fires on. Are these the same? Does every routing
class correspond to one SAE feature and vice versa?

This is not established. The routing class count
is determined by the attention weights. The SAE
feature count is determined by the residual stream
activations. They could agree or disagree. If they
agree, the SAE dictionary size gives you n directly.
If they disagree, something more subtle is happening.

## 2. Does reconstruction error bound BP approximation?

SAE reconstruction error measures how well the
sparse feature representation captures the residual
stream. BP approximation error measures how far
the transformer's beliefs are from the true posteriors
on its implicit factor graph.

Is there a formal relationship between these two?
If the SAE perfectly reconstructed the residual
stream, would that guarantee the BP approximation
is exact? Or are they measuring different things?

Establishing this relationship would connect the
SAE engineering objective (minimize reconstruction
error) to the BP theoretical objective (achieve
exact inference). It would give a principled reason
to care about SAE reconstruction quality beyond
interpretability.

## 3. What is the right dictionary size?

SAEs with larger dictionaries find more features.
There is no principled stopping criterion — you
can always find more features by making the
dictionary larger. In the BP framework, the right
dictionary size is 2n² where n is the number of
implicit factor graph nodes. But n is unknown for
ungrounded models.

Is there a way to determine the right F from the
model's weights, without running SAEs? Can you
predict F from D, L, H, D_ff and the training
data statistics? Or is F fundamentally empirical —
something you can only measure, not predict?

## 4. Can you recover the full factor graph from SAEs and attribution graphs?

SAE features are nodes. Attribution graph edges
are edges. Together they should give you the
implicit factor graph G(W).

But the attribution graph is computed per-prompt —
it shows the active subgraph for one specific
inference. The full factor graph is the union of
all active subgraphs across all possible inputs.
Can you recover the full graph by aggregating
attribution graphs across many prompts? How many
prompts would you need? Is the full graph finite
and recoverable in principle?

## 5. Do SAE features at different layers correspond to different rounds?

In the BP framework, layer l features are the
beliefs after l rounds of BP. Earlier layers should
have more concrete, surface-level concepts. Later
layers should have more abstract, high-level concepts.

The scaling monosemanticity paper found features
at multiple abstraction levels. But is the layering
clean — do early-layer SAE features correspond
to early rounds of inference, and late-layer SAE
features to late rounds? Or do abstract features
appear at all layers?

If the layering is clean, it would confirm the
L-round BP interpretation at the feature level.
If it is not clean, something more complex is
happening across rounds.

## 6. What is the SAE interpretation of function vector heads?

Function vector heads are the main open problem
in the COVER_CHART — diffuse attention, no clean
BP interpretation. SAEs might help here.

If you run an SAE on the residual stream at a
position where a function vector head has just
written its output, what features are active?
Is it one specific feature (the task concept)
or many features (a diffuse mixture)? Does the
SAE decomposition of the function vector output
have a clean BP interpretation even if the raw
attention pattern does not?

This could be the path to resolving the function
vector head problem — not by characterizing the
attention weights directly but by characterizing
what the attention writes into the residual stream
in SAE feature terms.

## 7. Can SAEs be used to ground an ungrounded model?

Grounding requires connecting tokens to declared
concept nodes. SAE features are empirical concept
nodes — they are the implicit routing classes made
visible. Can you use SAE features as a grounding
mechanism?

Concretely: run an SAE on a trained model, find
the F features, declare those features as the
concept vocabulary, map each token position to
its active SAE features, and run grounded BP on
the resulting explicit factor graph.

This would be a form of post-hoc grounding —
making an ungrounded model grounded by recovering
its implicit concept structure and making it
explicit. The no-hallucination corollary would
then apply to the grounded version. Whether this
is practically feasible and whether it would
reduce hallucination is entirely open.