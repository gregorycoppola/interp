# Coverage Chart

For each named attention head type: coverage status and
a short description of where it stands.

    GREEN  = fully covered by the BP framework
    YELLOW = partially covered, extension needed
    RED    = genuinely open, no clean BP account

---

GREEN  Previous token head
    Sharp attend-and-copy to position t-1. Direct
    instantiation of the formal projectDim/crossProject
    construction. Nothing beyond what the theory predicts.

GREEN  Induction head
    Two-round BP chain. Round 1 is previous token head.
    Round 2 is content-matched gather. Peirce lower bound
    says two hops require two rounds — circuit achieves
    exactly the minimum. Fully covered.

GREEN  Name mover head
    Sharp entity-copying gather. One node, one value,
    one destination. Same account as previous token head
    but content-addressed rather than position-addressed.
    Fully covered.

GREEN  Duplicate token head
    Sharp gather finding a prior occurrence of the current
    token. Prerequisite gather step before a conclusion-
    drawing update. Same machinery as induction. Covered.

GREEN  Retrieval head
    Sharp gather over long context. Same account as name
    mover and previous token, operating at longer range.
    The hallucination-on-pruning result connects directly
    to the grounding argument. Fully covered.

YELLOW S-inhibition head
    Negative message passing. The log-odds algebra handles
    it — logit(m0) + logit(1-m1) reduces belief when m1
    is high. But the formal weight construction for
    inhibitory messages is not written down. The theory
    accommodates it. The derivation is missing.

YELLOW Positional head
    Position-based routing is a special case of content-
    based routing where the key is position rather than
    semantics. The theory accommodates it in principle.
    The formal characterization of positional routing
    classes and the global-context-node interpretation
    of position-zero heads is not developed.

YELLOW Backup / redundant head
    Multiple heads voting for the same conclusion is k-ary
    OR — already proved in ORDecomposition.lean. The
    connection to the empirical redundancy phenomenon is
    not explicitly stated. Needs a short argument, not
    new machinery.

RED    Function vector head
    Diffuse attention over all W positions. No clean BP
    interpretation. The general theorem technically covers
    it — any weights implement weighted BP on some graph —
    but what that graph is and whether the computation is
    meaningful in BP terms is not established. Dominant
    at large scale. The main open problem.

---

## Summary

    GREEN:  5 types  (previous token, induction, name mover,
                      duplicate token, retrieval)
    YELLOW: 3 types  (S-inhibition, positional, redundant)
    RED:    1 type   (function vector)

The green types are all sharp routing — one neighbor,
one value, focused lookup. The theory was built for this
and covers it completely.

The yellow types need formal development but not new
theory. Each is a known extension of existing machinery.

The red type is the genuine gap. Function vector heads
are diffuse, dominant at scale, and not accounted for.
Solving this is the main open problem in connecting
the BP framework to production models.