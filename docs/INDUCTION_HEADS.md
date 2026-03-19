# Induction Heads

## What the Literature Found

Olsson et al. (2022) identified the induction circuit — the
most studied named circuit in transformer interpretability.
It is a two-layer circuit:

Layer 1: a previous token head. Attends to the token
immediately before the current position and copies it into
the current position's residual stream.

Layer 2: an induction head. Takes the current token as query,
looks for a position where the previous-token head wrote
something matching, and copies the token that followed it.

Together they implement: if you have seen [A][B] before and
now see [A], predict [B]. This is the primitive of in-context
learning — pattern matching and copying.

The circuit is sharp. The attention is concentrated on one
specific position, not diffuse across many. The previous
token head attends to exactly position t-1. The induction
head attends to exactly the position after the previous
occurrence of the current token.

Induction heads emerge in a sharp phase transition during
training, coinciding with a sudden improvement in in-context
learning ability.

## What This Means in BP Terms

This is the clearest empirical confirmation of the BP gather
step in a trained model.

The previous token head is performing: find position t-1,
copy its value into my residual stream. This is exactly
the projectDim/crossProject construction from the shannon
paper — a focused lookup that retrieves one specific
neighbor's belief and writes it into a designated dimension
of the residual stream.

The induction head is performing: find the position whose
content matches my query, copy the value at the next
position. This is content-based index matching — the
trained model has learned to route by content rather than
by explicit index, but the routing structure is the same.
One specific source, one specific destination, one value
copied.

The two-layer structure is significant. It takes two rounds
of BP to implement this: one round to write the shifted
token identity into the residual stream, one round to match
against it and copy the following value. This is a two-hop
inference chain on a chain-structured factor graph, exactly
matching the Peirce lower bound — two hops require two
rounds.

## What It Confirms

That trained transformers learn sharp, interpretable routing
circuits. That the gather step is not diffuse averaging but
focused lookup. That multi-layer circuits correspond to
multi-round BP inference chains. That the routing structure
emerges from training rather than being constructed by hand,
confirming the bayes-learner result at the circuit level.

## What It Leaves Open

Induction heads implement one specific routing pattern —
copy the token that followed a previous occurrence of the
current token. The BP framework is more general: any
local factor graph, any routing structure. The question
is whether the full diversity of routing behavior in trained
models can be characterized as BP on some implicit factor
graph, or whether induction heads are a special case that
happens to fit cleanly.

At larger scales, function vector heads become more important
than induction heads for in-context learning (see
FUNCTION_VECTORS.md). The sharp routing picture may be
a small-scale phenomenon that the more complex soft routing
of larger models supersedes.

## Key References

Olsson et al. 2022 — In-context learning and induction heads
Elhage et al. 2021 — A mathematical framework for transformer circuits