# Belief Propagation

## What It Is

Belief propagation is the algorithm for doing inference
on a factor graph. It is not an approximation of the
correct answer — on trees it gives the exact correct
answer. On graphs with loops it gives an approximation
that is often very good.

The algorithm is simple: nodes pass messages to their
neighbors. Each message summarizes what a node believes
about itself, given everything it has heard so far.
After enough rounds of message passing, every node has
heard from all the evidence in the graph and its belief
is the correct posterior.

## The Two Steps

Each round of BP has two steps.

Gather: each variable node collects messages from its
factor neighbors. It writes these messages into a scratch
space — a temporary holding area — before doing any
computation. This is the AND step: all required inputs
must be simultaneously present before any conclusion
is drawn.

Update: each variable node reads its scratch space and
computes a new belief by combining the gathered messages.
This is the OR step: the new belief combines all the
incoming evidence into a single updated probability.

In the transformer: gather is attention, update is FFN,
scratch space is the residual stream.

## Rounds

One round of BP is one synchronous update: every node
gathers and updates simultaneously. After k rounds,
a node has incorporated evidence from up to k hops
away in the factor graph.

On a chain of N nodes, you need at least N rounds to
propagate evidence from one end to the other. This is
the Peirce lower bound — proved formally in the pearl
repo. It is a hard lower bound on any local algorithm,
not just BP.

In the transformer: one layer is one round. L layers
are L rounds. A transformer with L layers can reason
about evidence chains up to L hops long.

## Convergence

On a tree-structured factor graph, BP converges in
exactly diameter(T) rounds to the exact marginal
posteriors. This is Pearl's 1988 result, formalized
in hard-bp-lean.

On a loopy graph, BP may not converge. When it does
converge, the fixed point minimizes the Bethe free
energy — an approximation to the true posterior.
Empirically, loopy BP converges and gives good
approximations on many practical graphs.

The transformer runs a fixed number of rounds (L)
rather than running until convergence. On a tree-
structured factor graph with L >= diameter, this
gives exact posteriors. On a loopy graph it gives
L rounds of approximation.

## Why This Matters for Transformers

The transformer does not know it is running BP. The
weights were learned by gradient descent. But the
shannon paper proves that whatever weights it has
learned, the computation it performs is BP on the
implicit factor graph those weights define.

This means every transformer forward pass is inference
on some probabilistic model. The model may not be the
intended one — if the weights are wrong or the graph
is loopy, the inference may be approximate or incorrect.
But the computational structure is always BP.