# SAE Features: The Empirical Concept Inventory

## What the Literature Found

Sparse autoencoders (SAEs) are trained to decompose
residual stream activations into sparse combinations of
learned directions. Each direction is a feature — something
that activates on some inputs and not others.

Anthropic's scaling monosemanticity paper (2024) trained
SAEs on Claude Sonnet and found millions of interpretable
features. The features are specific: one fires on the
concept "banana," another on "the Golden Gate Bridge,"
another on "base64 encoding," another on "the role of
assistant in a conversation."

The Golden Gate Claude experiment took one specific feature
and clamped it to maximum activation throughout the forward
pass. The model became obsessed with the Golden Gate Bridge,
routing every response back to it regardless of the query.

Anthropic's attribution graph work (2025) traces how
features influence each other through the computation —
which features activate which other features across layers.

## What This Means in BP Terms

SAE features are the empirical concept inventory — the
closest thing to a direct observation of the implicit
routing classes.

Each SAE feature is a direction in the residual stream
that corresponds to one concept. The feature fires when
the model is computing something related to that concept
at that position. In BP terms, this is one node in the
implicit factor graph being active — one belief being
high.

The clamping experiment is a direct manipulation of a
factor graph node. Forcing a feature to maximum activation
is equivalent to setting one node's belief to 1.0 and
holding it there. The downstream effect — all inference
routes through that concept — is exactly what BP predicts
when one node's belief is forced: it sends maximum-
confidence messages to all its neighbors, overwhelming
all other evidence.

The attribution graphs are factor graph edges made visible.
When feature A activates feature B across layers, that is
a directed edge in the implicit factor graph — a factor
potential connecting A to B. The attribution graph is the
empirical reconstruction of the implicit factor graph
structure.

## The Concept Count Question

SAEs consistently find more features than there are
residual stream dimensions. For D=4096, SAEs find tens
of thousands to millions of features depending on the
dictionary size. This is superposition — the model encodes
more concepts than it has dimensions by using non-orthogonal
directions.

This is the empirical approach to answering the concept
count question that the 2n² theorem answers in the grounded
case. For an ungrounded model, the SAE dictionary size is
a proxy for n — the number of implicit nodes in the factor
graph. The 2n² routing class count then depends on this
empirical n rather than a declared one.

## What It Confirms

That production transformers have discrete internal concepts
with specific semantic identities. That those concepts
behave like nodes in a factor graph — they activate on
specific inputs and influence downstream computation in
structured ways. That clamping a concept node corrupts
inference exactly as BP predicts. That the implicit factor
graph has recoverable structure via attribution graphs.

## What It Leaves Open

The precise correspondence between SAE features and BP
routing classes. Whether the SAE dictionary gives the
right n for the 2n² count. Whether attribution graphs
correspond exactly to factor potentials or to something
more complex. Whether the full implicit factor graph can
be recovered from SAE features and attribution graphs
combined.

## Key References

Bricken et al. 2023 — Towards monosemanticity (SAEs on one-layer MLP)
Anthropic 2024 — Scaling monosemanticity (SAEs on Claude Sonnet)
Anthropic 2024 — Golden Gate Claude
Anthropic 2025 — Circuit tracing and attribution graphs