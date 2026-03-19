# Trees vs Loops

## The Fundamental Divide

BP is exact on trees. BP is approximate on loops.
This is the single most important fact about belief
propagation.

A tree is a graph with no cycles — there is exactly
one path between any two nodes. A loop is a graph
where at least one pair of nodes has two or more
paths between them.

## Why Trees Are Special

On a tree, when node A receives a message from node B,
that message contains information from B's subtree —
a completely separate part of the graph from everything
else A knows about. The message is independent of
everything else A is computing.

This independence is what makes BP exact. Pearl's
sum-product algorithm exploits it: messages from
different subtrees are independent by construction,
so you can combine them without worrying about
double-counting evidence.

After diameter(T) rounds on a tree, every node has
heard from every other node exactly once, through
the unique path connecting them. The beliefs are
exact marginal posteriors.

## Why Loops Break Exactness

On a loopy graph, there are multiple paths between
some pairs of nodes. A message from A to B via one
path carries some information about node C. Another
message from A to B via a different path also carries
information about C. When B combines these messages,
it double-counts C's evidence.

Double-counting correlates messages that should be
independent. The result is overconfident posteriors —
the model is more certain than the evidence warrants
because it counted the same evidence twice.

## Loopy BP in Practice

Despite the lack of exactness guarantees, loopy BP
works well empirically on many practical graphs.
When loops are long or the factor potentials are weak,
the double-counting error is small and the approximation
is tight.

The loopy repo ran 500 trials of loopy BP on QBBN-
structured graphs: triangle, square, dating graph,
two-loop, QBBN chain. All 500 converged. Worst mean
KL divergence was 0.000102 — near-exact in practice.

## The No-Hallucination Corollary

The no-hallucination corollary follows directly from
the tree/loop divide:

A transformer with BP weights on a tree-structured
factor graph computes exact marginal posteriors.
Exact posteriors cannot hallucinate — the model
cannot assign high confidence to a claim that is
not supported by the evidence, because its confidence
is calibrated to the actual probability.

On a loopy graph, BP is approximate and the guarantee
breaks down. The model can assign overconfident beliefs
due to double-counting. This is one structural source
of hallucination.

The other source is wrong weights — weights that do
not implement BP correctly. The no-hallucination
corollary requires both tree structure AND BP weights.
Current LLMs have neither guaranteed: their factor
graphs are loopy and their weights are not constrained
to be BP weights.