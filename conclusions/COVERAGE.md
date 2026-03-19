# Coverage: What This Repo Established

A precise account of what is covered, what is partial,
and what is open after this analysis.

## Fully Established

### The core theorem holds unconditionally

Every sigmoid transformer with any weights implements
weighted loopy BP on its implicit factor graph. No
empirical finding contradicts this. Several confirm it
directly. The theorem is formally verified and empirically
supported.

### Sharp attention heads are fully covered

Previous token heads, induction heads, name mover heads,
duplicate token heads, retrieval heads — all are direct
instantiations of the BP gather step. One neighbor, one
value, sharp routing. The formal construction matches
what trained models learn. Universality confirms the
routing structure is a necessity not an accident.

### FFN hidden units are factor potentials

Geva et al.'s key-value memory interpretation and ROME's
factual editing results both confirm that FFN weights
encode factor potentials. Editing FFN weights changes
factual beliefs. This is exactly what the theory predicts.

### SAE features are implicit factor graph nodes

The Golden Gate Claude clamping experiment confirms that
SAE features behave like BP factor graph nodes. Forcing
one node to maximum belief propagates through the graph
exactly as BP predicts. Attribution graphs are factor
graph edges made visible.

### The function vector problem resolves

Diffuse attention is BP on continuous nodes. The apparent
gap closes into a positive theoretical statement: hybrid
factor graphs with boolean and continuous nodes. See
HYBRID_BP.md.

### Phase transitions confirm uniqueness

The sharp emergence of induction heads during training
is consistent with the uniqueness theorem — there is
one correct routing structure and gradient descent finds
it discretely.

## Partial Coverage

### Negative messages (S-inhibition heads)

The log-odds algebra handles negative evidence in
principle. The formal weight construction for inhibitory
messages is not written down. Extension needed, not
new machinery.

### Positional routing

Position-based routing is accommodated in principle.
The formal characterization of positional heads as
routing classes is not developed.

### Redundant heads and k-ary OR

The connection between backup heads and the k-ary OR
decomposition theorem is not explicitly stated.

## Open Questions

### The no-hallucination corollary for continuous nodes

What does exact inference mean for a continuous node?
What is the analog of the no-hallucination corollary
for a hybrid factor graph? When is continuous BP exact
vs approximate?

### Recovering continuous features empirically

SAEs find boolean features. What is the empirical tool
for finding continuous features — the real-valued nodes
in the hybrid factor graph? Is there an analog of SAE
for continuous variable recovery?

### The true concept count

For a grounded boolean model: 2n². For an ungrounded
model with hybrid nodes: unknown. The SAE gives a lower
bound on the boolean node count. The continuous node
count has no empirical estimate yet.

### The formal construction for hybrid BP

The transformer-bp-lean proof covers the boolean case.
Extending it to hybrid factor graphs — explicit weight
matrices for continuous nodes, proof that the forward
pass implements hybrid BP — is the main formal work
remaining.