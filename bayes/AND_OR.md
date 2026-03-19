# AND and OR: The Boolean Structure

## The Two Gates

All reasoning decomposes into two operations:
assembling evidence (AND) and drawing conclusions (OR).

AND asks: are all the required conditions present
simultaneously? It fires only when every input is true.
It enforces simultaneity — no conclusion before all
prerequisites are met.

OR asks: is any of the supporting conditions present?
It fires when at least one input is true. It aggregates
evidence — multiple independent reasons for the same
conclusion combine additively.

## The QBBN Structure

The QBBN (Quantified Boolean Bayesian Network) formalizes
this into a bipartite factor graph:

Conjunction nodes (Ψand): deterministic AND. All inputs
must be simultaneously present. The output is 1 iff
all inputs are 1. Used to assemble conjunctive evidence
before a conclusion is drawn.

Disjunction nodes (Ψor): probabilistic OR. Each input
independently supports the conclusion. The output is
the updateBelief combination of all inputs. Learned
from data — the weights encode how strongly each
input supports the conclusion.

The graph alternates: AND nodes feed OR nodes feed
AND nodes. This is the bipartite structure. Every
inference step is either evidence assembly (AND) or
conclusion drawing (OR), never both at once.

## The Transformer Implementation

Attention is AND. The residual stream enforces
simultaneity — all attention heads write their results
before the FFN runs. The FFN has no mechanism to run
on partial inputs. That architectural enforcement of
simultaneity is conjunction.

FFN is OR. Once attention has assembled the evidence,
the FFN computes the probabilistic conclusion from
all gathered inputs simultaneously. The sigmoid
activation implements the probabilistic OR via the
log-odds algebra.

The alternating layers are the bipartite structure
unrolled over depth:

    attention (AND) → FFN (OR) → attention (AND) → FFN (OR) → ...

Each layer is one level of the boolean reasoning graph.
Each attention block assembles. Each FFN block concludes.

## Probabilistic vs Deterministic

The deterministic version of AND is used in the
completeness proof — it enforces exact logical
conjunction. The probabilistic version allows graded
inputs and produces graded outputs.

The deterministic version of OR is also used in the
completeness proof. The probabilistic version — the
learned Ψor with sigmoid weights — is what the
transformer implements.

Both versions are the same computation in the limit
as beliefs become certain. The probabilistic versions
are the natural generalization to uncertain reasoning.

## Why This Structure

The AND/OR bipartite structure is not arbitrary. It
is the minimal structure needed for complete boolean
reasoning. Prawitz (1965) identified these as the
fundamental rules of natural deduction — introduction
and elimination of conjunction and disjunction. The
QBBN encodes Prawitz's rules as a factor graph.

The transformer implements this structure. Not because
it was designed to — because gradient descent found
it. The architecture that has won empirically across
modern AI is the architecture that the analysis of
reasoning already required.