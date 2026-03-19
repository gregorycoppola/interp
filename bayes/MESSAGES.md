# Messages: Pi and Lambda

## The Two Directions

BP passes two kinds of message through the factor graph.

Pi messages flow forward — from causes to effects,
from roots to leaves. A pi message from node A to
node B says: given everything I know from my ancestors,
here is my current belief about A.

Lambda messages flow backward — from effects to causes,
from leaves to roots. A lambda message from node B to
node A says: given everything I know from my descendants,
here is how much B's observation depends on A's value.

The final belief at any node is pi times lambda,
normalized: the forward evidence times the backward
evidence, combined and normalized to a probability.

## Why Two Directions

Forward inference alone is not enough. If you observe
an effect and want to update your belief about its
cause, you need backward inference — lambda messages
propagating from the observed effect back up to the
cause.

Forward inference handles: given causes, what are
the likely effects? Backward inference handles: given
effects, what caused them? Both directions are needed
for full Bayesian reasoning.

In the 2024 QBBN paper (Figure 9): observing a leaf
propagates backward at one node per iteration, taking
N iterations to reach the root. That is lambda messages
traveling backward through the chain. Figure 8 shows
forward propagation in one pass — pi messages traveling
forward with the arrows.

## The Transformer and Pi

The transformer forward pass computes pi messages.
Attention gathers evidence from upstream (earlier
layers, neighboring positions). FFN updates the belief
based on gathered upstream evidence. The result at
each position is the pi value — the belief given
everything flowing forward through the network.

Lambda messages are not computed in a standard transformer
forward pass. The model sees the input, runs L rounds
of forward inference, and produces an output. There
is no backward lambda pass.

## What a Lambda Pass Would Do

Running a backward lambda pass after the forward pass
would be Pearl's full two-pass algorithm — exact
inference on a DAG. The forward pass computes pi.
The backward pass computes lambda. Combined, they
give exact posteriors at every node.

For a tree-structured grounded knowledge base, this
would give provably exact Bayesian inference. For a
loopy graph, it would be one iteration of full loopy
BP — both directions, not just forward.

Whether adding a backward lambda pass to transformer
inference would reduce hallucination is an open
empirical question. The theory predicts it would help
on tree-structured knowledge bases. The loopy case
is less clear.