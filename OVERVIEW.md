# Overview

A meta-analysis of the mechanistic interpretability
literature through the lens of the Transformers are
Bayesian Networks framework (Coppola 2026).

## What This Repo Is

The shannon paper proves that every sigmoid transformer
implements weighted loopy belief propagation on its
implicit factor graph. Mechanistic interpretability has
been reverse engineering transformers empirically without
this framework. This repo asks: what do the empirical
findings mean in BP terms, and where does the theory
need development?

We run no new experiments. We read the literature and
interpret it.

## Files

    INVENTORY.md                typology of named attention head types
    LITERATURE_LIST.md          catalog of findings tagged by BP concept
    THEORETICAL_COVERAGE.md     coverage analysis — what the theory handles

    docs/INDUCTION_HEADS.md     sharp gather, BP routing confirmed
    docs/FUNCTION_VECTORS.md    diffuse gather, open BP question
    docs/NAMED_CIRCUITS.md      multi-round BP on subgraphs
    docs/RETRIEVAL_HEADS.md     grounding and hallucination connection
    docs/SAE_FEATURES.md        empirical concept inventory
    docs/SUPERPOSITION.md       why concepts exceed dimensions
    docs/UNIVERSALITY.md        routing patterns as structural necessities
    docs/PHASE_TRANSITIONS.md   routing structure emerging discretely
    docs/OPEN_QUESTIONS.md      what the literature has not answered

    future_work/OVERVIEW.md            index of open problems
    future_work/FUNCTION_VECTORS.md    BP account of diffuse attention
    future_work/NEGATIVE_MESSAGES.md   formal construction for inhibition
    future_work/POSITIONAL_ROUTING.md  positional heads as routing classes
    future_work/REDUNDANT_HEADS.md     connect to k-ary OR decomposition

## The Main Finding

The theory covers the sharp routing regime completely.
Sharp attention — one neighbor, one value, focused lookup —
is exactly the formal BP construction, and the empirical
literature confirms this accounts for most named,
well-understood circuits: previous token heads, induction
heads, name mover heads, retrieval heads, duplicate token
heads.

The theory does not yet cover the diffuse routing regime.
Function vector heads — dominant at large scale — have
diffuse attention patterns that do not map cleanly onto
the sharp one-neighbor gather of the formal construction.
This is the main open problem.

The alignment between coverage and grounding is not
accidental. Sharp routing corresponds to grounded inference.
Diffuse routing corresponds to ungrounded inference. The
theory is complete for the grounded case and open for the
ungrounded case.

## Relationship to Other Repos

    shannon         the paper — proves transformer implements BP
    pearl           Peirce lower bound — BP and backprop optimality
    transformer-bp-lean   Lean proof — transformer implements BP
    hard-bp-lean    Lean proof — BP exact on trees
    bayes-learner   empirical — gradient descent finds BP weights
    loopy           empirical — loopy BP on QBBN graphs