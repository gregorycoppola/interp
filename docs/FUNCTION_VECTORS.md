# Function Vector Heads

## What the Literature Found

Todd et al. (2024) identified a second class of attention
heads important for in-context learning: function vector
(FV) heads. Unlike induction heads which copy specific
tokens, FV heads encode a latent representation of the
entire task being demonstrated.

When you show a model several examples of a task — English
to French translation, capitalization, antonyms — certain
attention heads compute a vector that represents the task
itself, independent of the specific examples. This vector
can be extracted and injected into other contexts to cause
the model to perform the task without examples.

FV heads have diffuse attention patterns. They attend across
many positions rather than concentrating on one. They are
reading something global about the context, not retrieving
one specific value.

At larger model scales, FV heads drive in-context learning
more than induction heads. Many FV heads start as induction
heads during training and transition to the FV mechanism —
suggesting sharp routing is a precursor to abstract routing.

## What This Means in BP Terms

This is the open question in the BP framework.

Induction heads map cleanly onto the BP gather step: one
neighbor, one value, sharp routing. FV heads do not map
cleanly. They are computing something aggregate — a summary
of the whole context — rather than retrieving one specific
neighbor's belief.

In BP terms, a diffuse gather over W positions with weights
given by the attention distribution is a weighted mixture
of W neighbors' beliefs. This is not standard BP, which
passes messages from specific neighbors. It is something
closer to a mean-field approximation — averaging over all
neighbors rather than passing messages from each.

Whether there is a clean BP interpretation of FV heads is
an open question. Some possibilities:

One: FV heads are doing BP on a different graph structure —
one where the current position is connected to all W
positions with weights given by the attention distribution.
This is loopy BP on a fully connected graph, which is
what the general theorem predicts for soft attention.

Two: FV heads are doing something outside the BP framework —
a form of approximate inference that the BP account does
not directly cover. This would be a limitation of the BP
interpretation.

Three: FV heads are implementing a higher-level routing —
not retrieving one specific belief but retrieving a summary
statistic of the belief distribution across many positions.
This might correspond to a marginalization operation in BP
rather than a message pass.

## What It Confirms

That trained transformers develop routing behaviors beyond
simple one-neighbor lookup. That the gather step at scale
is richer than the formal BP construction assumes. That
sharp routing (induction) and abstract routing (FV) are
related — one transitions into the other during training.

## What It Leaves Open

The BP interpretation of diffuse attention. Whether mean-
field approximation is the right framing. Whether there
is a class of factor graphs for which soft attention
implements exact BP. Whether the transition from induction
to FV heads during training corresponds to a change in
the implicit factor graph structure.

## Key References

Todd et al. 2024 — Function vectors in large language models
Hendel et al. 2023 — In-context learning creates task vectors
Yin et al. 2025 — Further analysis of FV and induction heads