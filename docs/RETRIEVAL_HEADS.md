# Retrieval Heads

## What the Literature Found

A class of attention heads specifically responsible for
retrieving factual information across long contexts. These
heads are sharply focused — they locate and copy specific
pieces of information from distant positions.

Pruning retrieval heads causes hallucination. The model
loses the ability to retrieve grounded facts and begins
generating plausible but unsupported content. Pruning
non-retrieval heads does not have this effect.

Meng et al. (2022) in the ROME paper showed that factual
associations — the knowledge that "the Eiffel Tower is in
Paris" — are stored in specific FFN layers at specific
positions, and can be edited by modifying those weights.
The factual knowledge is localized, not distributed across
all layers equally.

## What This Means in BP Terms

Retrieval heads are the grounding mechanism in a trained
ungrounded LLM.

In the BP framework, grounding means having an explicit
connection between a token and a declared entity in the
knowledge base. An ungrounded LLM has no explicit
grounding — but retrieval heads function as implicit
grounding: they locate where in the context a specific
fact was stated and copy it forward to where it is needed.

When retrieval heads work correctly, the model is doing
something like grounded inference — the answer is
supported by a specific piece of evidence in the context.
When retrieval heads are pruned, that implicit grounding
is lost and the model hallucinates — exactly the
structural consequence the shannon paper predicts for
operating without grounding.

The ROME result connects to the factor potential structure.
The FFN layers storing factual associations are the OR
nodes with specific learned potentials — the weights
encoding that "Eiffel Tower" and "Paris" are connected.
Editing those weights directly edits the factor potential,
changing the model's belief about that factual association.
This is the most direct empirical evidence that FFN weights
are factor potentials in the BP sense.

## What It Confirms

That hallucination is causally linked to failure of the
retrieval/grounding mechanism. That factual knowledge is
stored in specific FFN weights — the factor potentials.
That the BP account of hallucination as a grounding failure
is empirically supported by the retrieval head ablation
results.

## What It Leaves Open

Whether retrieval heads implement exact BP gather or a
softer version. Whether the factual associations stored
in FFN layers correspond to the explicit factor tables
of the QBBN or to something more implicit. Whether
editing factor potentials via ROME-style interventions
can be used to ground an otherwise ungrounded model.

## Key References

Meng et al. 2022 — ROME: locating and editing factual associations
Wu et al. 2024 — Retrieval heads mechanistically explain long-context factual recall