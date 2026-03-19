# The Next Project

This repo gathered evidence and concluded. The next
project begins from those conclusions and builds forward.

## What This Repo Established

The core theorem holds. Sharp attention is fully covered.
The function vector problem resolves into hybrid BP.
The implicit factor graph has boolean and continuous nodes.
The theory is in good shape. The extensions are clear.

## What the Next Project Should Do

The next project is the formal extension of the shannon
paper to hybrid factor graphs.

The shannon paper proves: every sigmoid transformer
implements weighted loopy BP on its implicit factor graph,
for boolean variable nodes. The next project proves: every
sigmoid transformer implements weighted loopy BP on its
implicit hybrid factor graph, for boolean and continuous
variable nodes mixed.

The specific things to prove:

One — the general hybrid BP theorem. For any sigmoid
transformer with any weights, there exists an implicit
hybrid factor graph G_h(W) with boolean and continuous
nodes such that one forward pass implements one round
of weighted loopy BP on G_h(W). This generalizes
theorem 6.1 from the shannon paper.

Two — the constructive hybrid proof. Exhibit explicit
weight matrices for a transformer that implements exact
hybrid BP on a declared hybrid factor graph. Extend
the transformer-bp-lean construction to handle continuous
nodes.

Three — the hybrid no-hallucination corollary. On a
tree-structured hybrid factor graph with BP weights,
what does exact inference guarantee? For boolean nodes
it guarantees exact marginal posteriors. For continuous
nodes it should guarantee exact posterior distributions
over real-valued quantities. State and prove this precisely.

Four — the continuous updateBelief. What is the analog
of updateBelief for a continuous node? For boolean nodes:
updateBelief(m0, m1) = sigma(logit(m0) + logit(m1)).
For a continuous node receiving a weighted mixture of
W real-valued inputs: what is the exact Bayesian update?
Is it the attention-weighted average? Under what
independence assumptions?

Five — the empirical program for continuous features.
SAEs find boolean features. What finds continuous features?
Is there an analog of SAE for continuous variable recovery?
Can you identify which residual stream dimensions are
boolean and which are continuous from the weights alone?

## The Repo Name and Structure

The next repo should be called something like `hybrid`
or `continuous` or `shannon2`. It starts where this
one ends — initialized with the conclusions here as
its prior belief state.

It will need:
- A Lean 4 proof directory extending transformer-bp-lean
- An experiments directory confirming the continuous
  BP construction empirically
- A theory directory developing the continuous updateBelief
  function and the hybrid no-hallucination corollary
- A connection back to the interp repo's empirical findings

## The Bigger Picture

The shannon paper closed the boolean case. This repo
established that the boolean case is not the whole story —
production transformers have continuous nodes too. The
next project closes the continuous case.

After that: the hybrid case is complete. Transformers
are Bayesian networks with hybrid factor graphs. Boolean
and continuous. Discrete and continuous reasoning unified
in the same architecture under the same mathematical
framework.

That is the complete theory. This repo was the gather
step. The next repo is the update step. The conclusion
is written into the residual stream and carried forward.

Calculemus.