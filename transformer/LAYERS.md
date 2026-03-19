# Layers: Rounds of BP

## One Layer = One Round

Each transformer layer is one round of belief
propagation. The layer consists of:

    1. attention block  — gather step
    2. FFN block        — update step

After one layer, every token position has:
- gathered evidence from its neighbors via attention
- updated its beliefs based on that evidence via FFN

This is exactly one synchronous BP round: every node
reads from its neighbors and updates simultaneously.

## L Layers = L Rounds

A transformer with L layers runs L rounds of BP per
forward pass. After L rounds, beliefs at distance L
in the factor graph have been updated.

    Llama 3 8B:   L = 32   rounds
    Llama 3 70B:  L = 80   rounds
    Llama 3 405B: L = 126  rounds

L is the reasoning depth — how many sequential
inference steps the model can perform in one forward
pass. A reasoning chain requiring k hops requires
at least k layers. This is the Peirce lower bound
applied to transformer depth.

## The Peirce Lower Bound

The pearl repo proves: for any local algorithm on a
chain of length N, at least N rounds are required
to propagate information N hops. This applies to BP
and to backprop by the same proof.

Applied to transformers: a model with L layers cannot
perform reasoning chains longer than L hops in one
forward pass. This is not a limitation of any specific
architecture — it is a hard lower bound on all local
algorithms.

Chain of thought prompting is the practical solution:
write down intermediate conclusions as tokens, making
them available as new evidence in subsequent forward
passes. Each forward pass is L rounds of BP. Multiple
passes chain together for reasoning deeper than L.

## Layers Are Not Shared

Each layer has its own attention weights and FFN weights.
Layer l has W_Q^l, W_K^l, W_V^l and W1^l, W2^l —
entirely separate from layer l+1.

This is necessary because each round of BP may require
different routing patterns and different factor
potentials. Early layers detect surface features.
Later layers detect abstract relationships. Sharing
weights across layers would force every round to use
the same detectors, breaking the hierarchical structure.

This is the key difference from recurrent networks
which share weights across steps. Recurrent networks
assume all steps are homogeneous. Transformer layers
assume each round is distinct.

## The AND/OR Alternation

Each layer alternates AND (attention) then OR (FFN):

    attention → FFN → attention → FFN → ...

This is the QBBN bipartite computation unrolled over
depth. Conjunction nodes gather required evidence.
Disjunction nodes compute conclusions from gathered
evidence. The strict alternation is Pearl's gather/
update algorithm exactly.

The architecture that has won empirically across modern
AI is the architecture that the analysis of reasoning
already required.