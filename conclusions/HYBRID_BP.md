# The Main Conclusion: Hybrid BP

## The Problem We Started With

Function vector heads — the dominant attention mechanism
at large model scale — have diffuse attention patterns.
They spread weight across many token positions rather
than concentrating on one. They do not map cleanly onto
the sharp one-neighbor gather of the formal BP construction.

This looked like a gap. The COVER_CHART marked it red.
The future_work directory listed it as the main open
problem. The BP framework seemed to cover the small-scale
sharp-attention regime cleanly and struggle with the
large-scale diffuse-attention regime.

## The Resolution

The gap closes once you allow continuous-valued nodes
in the factor graph.

The QBBN boolean framework uses boolean variable nodes —
beliefs in (0,1) representing probabilities over binary
propositions. This is the right framework for discrete
conceptual reasoning: is this entity present, does this
rule apply, is this proposition true.

But production transformers also compute continuous
quantities. Numerical magnitudes. Statistical aggregations
over context. Weighted averages of real-valued properties.
These are not boolean beliefs — they are real-valued
variables. And the factor graph framework already handles
real-valued nodes. Sum-product message passing generalizes
to continuous variables by replacing sums with integrals.
The message passing structure is identical.

A function vector head computing a weighted average over
W positions is doing exactly this: computing a message
for a continuous-valued node by integrating over its
neighbors, weighted by the attention distribution. This
is BP on a continuous node. It is not outside the theory.
It is a natural generalization of it.

## The Positive Statement

Transformers implement BP on hybrid factor graphs.

A hybrid factor graph has two kinds of variable nodes:

Boolean nodes: beliefs in (0,1). Represent discrete
propositional concepts. Updated by updateBelief —
the sigmoid log-odds combination. Gathered by sharp
attention heads. Found empirically by SAEs as
monosemantic features.

Continuous nodes: real-valued quantities. Represent
numerical magnitudes, statistical properties, abstract
task representations. Updated by weighted aggregation —
the attention-weighted sum of value vectors. Gathered
by diffuse attention heads — function vector heads.
Found empirically by... an open question. SAEs find
boolean features. What finds continuous features?

Both node types are handled by the same transformer
architecture. Both are covered by the general BP theorem.
Both contribute to the residual stream at each layer.
The architecture is doing boolean inference and continuous
estimation simultaneously, in the same forward pass,
through the same mechanism.

## Why This Is Not Trivial

The QBBN boolean framework was sufficient for the formal
proofs — the constructive proof, the uniqueness theorem,
the no-hallucination corollary, the Peirce lower bound.
All of these are proved in the boolean case.

Extending to hybrid factor graphs is a genuine theoretical
move. It changes what the implicit factor graph looks like.
It changes what exact inference means. It changes what
the no-hallucination corollary says.

For boolean nodes: exact inference means exact marginal
posteriors over binary propositions. The no-hallucination
corollary is precise — beliefs match true posterior
probabilities.

For continuous nodes: exact inference means exact
posterior distributions over real-valued quantities.
What is the no-hallucination corollary for a continuous
node? What does it mean to hallucinate a continuous
quantity? These are open questions that the next project
should address.

## What This Means for the Literature

The sharp/diffuse attention dichotomy in mechanistic
interpretability — induction heads vs function vector
heads, small models vs large models — corresponds to
the boolean/continuous node dichotomy in the BP framework.

Sharp attention: boolean node routing. One neighbor,
one belief, discrete identity.

Diffuse attention: continuous node aggregation. Many
neighbors, weighted sum, real-valued property.

The transition from sharp to diffuse heads as model
scale increases corresponds to the model learning to
track more continuous quantities alongside its discrete
propositional beliefs. Larger models do more continuous
estimation. This is not a departure from BP — it is
BP on a richer hybrid graph.

## The One-Sentence Version

Transformers are Bayesian networks with hybrid factor
graphs: boolean nodes for discrete propositional
reasoning, continuous nodes for real-valued estimation,
both updated by belief propagation, both in the same
residual stream.