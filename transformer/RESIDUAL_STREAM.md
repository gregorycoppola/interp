# The Residual Stream

## What It Is

The residual stream is the D-dimensional vector that
persists at each token position throughout all L layers.
It is the belief state of that token — everything the
model currently believes about that position.

Think of it as a whiteboard. Each token position has
its own whiteboard with D slots. Layers read from it
and write back to it. It is never cleared. Everything
accumulates via addition — the residual connection.

## Why Residual

Each layer adds its output to the existing vector rather
than replacing it:

    x^{l} = x^{l-1} + attention_output + ffn_output

The stream accumulates. Old beliefs persist unless
actively overwritten. New evidence adds to existing
beliefs rather than replacing them.

This is the correct behavior for iterative BP: beliefs
update incrementally across rounds, accumulating evidence
from further and further away in the factor graph.

## D: The Width of the Stream

D is the number of dimensions in the residual stream.
Each dimension is one belief being tracked at that
position.

    Llama 3 8B:   D = 4,096
    Llama 3 70B:  D = 8,192
    Llama 3 405B: D = 16,384

D is the bandwidth of the reasoning channel. All
information that passes from one layer to the next
must fit through D dimensions. H and D_ff can be
large within a layer but they cannot change what
gets passed forward. D is the bottleneck.

## D Dimensions vs D Propositions

If the D dimensions are independent, each tracks one
proposition and the residual stream holds D independent
beliefs. The model is doing D parallel BP computations
simultaneously.

In practice the dimensions are not fully independent.
Superposition means the model encodes more than D
propositions by representing them as non-orthogonal
directions across the D dimensions. The true number
of beliefs being tracked may be much larger than D.

Whether the dimensions are independent or superposed
determines whether the BP algebra applies cleanly
(independent case) or requires extension (superposed
case). See SUPERPOSITION.md in the docs directory.

## The Stream as Shared Workspace

Elhage et al. (2021) frame the residual stream as a
shared workspace where attention heads read and write.
Each head reads from the stream at all positions,
computes a result, and writes back to the stream at
the current position.

In BP terms: the residual stream is the message board
where gathered evidence waits before the FFN processes
it. The AND gate is enforced architecturally — the FFN
cannot run until all attention heads have written their
results into the stream. By the time the FFN executes,
all the evidence is simultaneously present.

That simultaneity — all gathered evidence present before
any conclusion is drawn — is the conjunction. The residual
stream is what makes AND structural rather than learned.