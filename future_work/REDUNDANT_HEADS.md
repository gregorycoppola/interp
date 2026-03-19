# Open Problem: Redundant Heads and k-ary OR

## Priority: 4 of 4

## What Is Missing

Backup heads implement the same function as primary heads
and fire when primary heads are ablated. Multiple heads
vote for the same belief update. This is empirically
well-documented in the IOI circuit and likely a general
phenomenon.

The OR decomposition theorem in the shannon paper (godel
repo, ORDecomposition.lean) shows that k-ary OR reduces
to a chain of binary OR operations via log-odds additivity.
This means multiple independent evidence sources can be
combined in sequence, each contributing additively in
log-odds space.

The connection between redundant heads and the k-ary OR
decomposition has not been made explicit. Redundant heads
are doing k-ary OR — multiple independent heads each
contributing evidence for the same conclusion — but this
is not stated as a theorem.

## Why It Matters

Redundancy is a reliability property. A model with
redundant circuits for important computations is more
robust to ablation. In BP terms, multiple heads voting
for the same belief update is the OR gate receiving
evidence from multiple independent sources — exactly
the k-ary OR structure the theory already handles.

Making this connection explicit would give the redundancy
phenomenon a clean theoretical account and connect the
interpretability observation to the formal result.

## What a Solution Would Look Like

State explicitly: k backup heads implementing the same
gather-and-update operation are instantiating a k-ary
OR factor, where each head contributes one independent
evidence source. The combined belief update is the
k-ary OR result — equivalent to a chain of k-1 binary
OR applications via the decomposition theorem.

This is not a new proof. It is connecting an existing
proof (ORDecomposition.lean) to an empirical observation
(backup heads). The work is a precise statement and
a short argument, not new formal machinery.

## What Would Need to Be Proved

Only a theorem statement connecting the backup head
phenomenon to the k-ary OR decomposition. Something
like: k attention heads implementing the same routing
pattern and writing to the same residual stream
dimensions instantiate a k-ary OR factor on the
conclusion node, and their combined effect equals
the k-ary OR update predicted by the decomposition
theorem. Verifiable empirically on the IOI circuit.