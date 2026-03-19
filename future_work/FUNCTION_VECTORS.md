# Open Problem: BP Account of Diffuse Attention

## Priority: 1 of 4 — the main open problem

## What Is Missing

The formal BP construction in the shannon paper uses sharp
attention: each head attends to exactly one neighbor,
identified by content-based index matching. The softmax
concentrates weight on one position. One value is copied.

Function vector heads have diffuse attention. The softmax
spreads weight across many positions. The head computes
a weighted average of value vectors from all W positions.
The result is not any one neighbor's belief — it is a
mixture.

There is no formal characterization of what this mixture
computes in BP terms.

## Why It Matters

Function vector heads become the dominant mechanism for
in-context learning at larger model scales. If the BP
framework cannot account for them, it is missing the
dominant behavior of production models. The theory
would be complete for small models and incomplete for
large ones — exactly backwards from what is needed.

## What a Solution Would Look Like

There are three candidate framings, each with different
implications.

Framing 1: soft attention is exact BP on a fully connected
graph. The attention weights A[t, t'] are the edge weights
between token positions t and t' in the implicit factor
graph. The weighted mixture is exact BP on this fully
connected loopy graph. The general theorem already says
every sigmoid transformer implements weighted BP on its
implicit factor graph — so this framing is already
technically correct. The question is whether it is
meaningful: does the fully connected loopy graph have
interpretable structure, or is it just a restatement
of the computation?

Framing 2: soft attention is mean-field approximation.
Mean-field variational inference replaces exact BP
messages with averages over the full distribution.
Diffuse attention is the transformer doing mean-field
rather than exact message passing. This would explain
why FV heads are approximate — they are computing a
mean-field update rather than an exact BP gather.

Framing 3: FV heads implement a different inference
operation entirely — marginalization or expectation
rather than message passing. The head is computing
E[f(neighbors)] rather than passing a message from one
specific neighbor. This would require extending the
BP framework to include marginalization operations
as first-class steps.

## What Would Need to Be Proved

Whichever framing is correct, a formal result would
need to characterize: given attention weights A[t, t']
and value weights W_V, what quantity does the head
compute, and what is its role in the implicit factor
graph? Is there a class of factor graphs for which
soft attention implements exact BP? If not, what is
the approximation error and when is it tight?

## Connection to Grounding

The sharp-to-diffuse transition during training may
correspond to the model moving from grounded to
ungrounded inference. Sharp routing requires knowing
which specific node to look at — grounded routing.
Diffuse routing averages over many nodes — ungrounded
approximation. If this is right, grounding and sharp
attention are not just correlated but causally linked:
grounding is what makes sharp routing possible, and
its absence forces diffuse approximation.

This would give a precise mechanistic meaning to the
grounding argument in the shannon paper: a grounded
model has sharp attention heads because each token
knows which node it corresponds to. An ungrounded
model has diffuse attention heads because tokens do
not have declared identities and the model must
average over possibilities.