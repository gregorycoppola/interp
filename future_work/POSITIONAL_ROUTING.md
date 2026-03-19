# Open Problem: Positional Heads as Routing Classes

## Priority: 3 of 4

## What Is Missing

The formal BP construction routes by content — the Q·K
dot product peaks when the token's stored neighbor index
matches the query. Positional heads route by position —
they attend to position zero, or the previous position,
or a fixed offset, regardless of content.

The theory accommodates positional routing in principle:
position can be encoded in the token embedding and used
as a routing key just like a semantic index. But this
is not formally developed. The routing class structure
for positional heads — what (nodeType, ownIndex,
neighborIndex) triple they correspond to — is not
characterized.

## Why It Matters

Position-zero heads are common across many models and
appear to implement a form of global context storage —
a mechanism for broadcasting information from the first
token to all positions. This is a structurally important
behavior that the theory should account for.

Positional routing is also the bridge between the formal
BP construction (which uses explicit position indices)
and content-based routing (which uses semantic similarity).
Characterizing it formally would clarify the relationship
between the two.

## What a Solution Would Look Like

Show that RoPE (rotary position embedding) or learned
position embeddings implement a routing key that the
Q·K dot product can use to route by position. Characterize
the resulting routing class as a special case of the
general (nodeType, ownIndex, neighborIndex) structure
where neighborIndex is determined by position rather
than semantic content.

For position-zero heads specifically: show that attending
to position zero is equivalent to routing to a global
context node — a node connected to all positions in the
factor graph, broadcasting a global belief.

## What Would Need to Be Proved

That positional routing is a valid instance of the
routing class structure from the 2n² theorem. That
position-zero attending corresponds to a specific
factor graph topology — the global context node — and
that this topology has a clean BP interpretation.