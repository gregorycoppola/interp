# Named Circuits

## What the Literature Found

Several complete circuits have been reverse engineered —
end-to-end accounts of how specific model behaviors are
implemented across multiple attention heads and layers.

### Indirect Object Identification (IOI)

Wang et al. (2022) reverse engineered the full circuit in
GPT-2 Small responsible for completing sentences like
"When Mary and John went to the store, John gave a drink
to ___" with "Mary."

The circuit involves roughly 26 attention heads organized
into functional groups: duplicate token heads, S-inhibition
heads, name mover heads, and backup name mover heads. Each
group plays a specific role in the multi-step inference.

### Greater-Than Circuit

A circuit that determines which of two years is greater.
Involves specific attention heads that identify the decade
and year digits and compare them. Multi-step, involving
heads at different layers passing information forward.

### Modular Arithmetic

Nanda et al. (2023) found that transformers trained on
modular arithmetic (a + b mod p) implement the computation
via discrete Fourier transforms in the embedding space,
using specific attention heads to combine frequency
components.

### Docstring Circuit

A circuit for completing Python docstrings by copying
argument names from the function signature. Sharp attention,
retrieving specific tokens by position.

## What This Means in BP Terms

Named circuits are the empirical face of multi-round BP
inference chains on specific subgraphs of the implicit
factor graph.

The IOI circuit is the most developed example. In BP terms:
the duplicate token heads are performing one gather step —
finding where the subject name appears earlier in the
context. The S-inhibition heads are performing an update
step — suppressing the subject name as a candidate answer.
The name mover heads are performing a final gather step —
retrieving the indirect object name that remains after
inhibition.

This is a three-round inference chain: gather (find
duplicate), update (suppress), gather (retrieve answer).
Three rounds for a three-hop inference. The Peirce lower
bound applies: you cannot do this in fewer rounds with a
local algorithm.

The modular arithmetic circuit is different — it does not
look like standard BP at all. It uses Fourier representations
and interference patterns. This may be a case where the
transformer has found a non-BP algorithm for a specific
computational task, which the general theorem accommodates:
any weights implement weighted BP on some implicit factor
graph, but the factor graph for modular arithmetic may have
a structure that looks nothing like a knowledge base.

## What It Confirms

That multi-layer computations in trained models decompose
into interpretable multi-step inference chains. That the
number of rounds matches the depth of the reasoning required.
That specific functional roles — gather, update, inhibit —
correspond to specific heads at specific layers.

## What It Leaves Open

Whether all named circuits have clean BP interpretations
or whether some (like modular arithmetic) require a
different account. Whether the implicit factor graph can
be recovered from the circuit description. Whether the
Peirce lower bound is tight for named circuits — do they
use the minimum number of layers the reasoning requires?

## Key References

Wang et al. 2022 — Interpretability in the wild: IOI circuit
Nanda et al. 2023 — Progress measures for grokking
Anthropic 2025 — Circuit tracing and attribution graphs