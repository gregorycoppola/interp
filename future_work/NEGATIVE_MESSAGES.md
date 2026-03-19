# Open Problem: Formal Construction for Inhibition

## Priority: 2 of 4

## What Is Missing

The formal BP construction in the shannon paper handles
positive evidence: two incoming beliefs combine via
updateBelief to produce a higher belief in the conclusion.
The log-odds algebra handles negative evidence in
principle — logit(p) + logit(1-q) reduces the combined
belief when q is high — but the formal weight construction
for inhibitory messages is not written down.

S-inhibition heads suppress a candidate answer. They
write a signal that reduces belief in the subject name
as the output. This is negative message passing —
evidence against a conclusion rather than for it.

## Why It Matters

Inhibition is a general phenomenon. Any reasoning system
that handles competing hypotheses needs to suppress
incorrect candidates. S-inhibition heads are the named
instance but the general class is likely large. Without
a formal account of negative messages, the theory cannot
cover this whole class of behavior.

## What a Solution Would Look Like

The log-odds algebra already handles it. For two evidence
sources where one is positive and one is negative:

    logit(combined) = logit(m0) + logit(1 - m1)

where m1 is the belief in the suppressing signal. This
reduces the combined belief when m1 is high. The formal
weight construction needs to show: what specific W_Q,
W_K, W_V, W1, W2 matrices implement this negative
message pass in the transformer?

The attention gather step is the same — find the
inhibiting node, copy its value. The FFN update step
needs modification: instead of updateBelief(m0, m1)
it needs updateBelief(m0, 1 - m1) or the equivalent
weight modification.

This is likely a small extension of the existing
construction. The theoretical machinery exists. The
specific formal derivation is missing.

## What Would Need to Be Proved

That there exist explicit weight matrices W such that
the transformer with those weights implements one round
of inhibitory BP — gathering a suppressing neighbor's
belief and reducing the current belief accordingly.
Ideally verified in Lean as an extension of
transformer_implements_bp in transformer-bp-lean.