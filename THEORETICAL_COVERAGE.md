# Theoretical Coverage

For each named attention head type: where it stands with
respect to the BP framework. Then a meta-summary of overall
coverage.

---

## Previous Token Head

Status: fully covered.

This is the formal BP construction instantiated in a trained
model. Sharp attention on one specific neighbor, copy one
value into a designated residual stream dimension. The
projectDim/crossProject weight construction from the shannon
paper is exactly this. Nothing in the empirical description
goes beyond what the theory predicts.

---

## Induction Head

Status: fully covered, with one structural note.

The two-layer circuit maps directly onto a two-round BP
inference chain. Round 1 is a previous token head gather.
Round 2 is a content-matched gather using the result of
round 1. The Peirce lower bound says two hops require two
rounds — the circuit achieves exactly that minimum.

The structural note: induction uses content-based routing
rather than explicit index matching. The shannon paper
constructs routing by stored index. Trained models learn
to route by content similarity. Both are valid instances
of the gather step — content similarity is just a softer
version of index matching that generalizes across surface
forms. The theory covers it.

---

## Function Vector Head

Status: open. The main gap in the theory.

Diffuse attention over all W positions does not map onto
the sharp one-neighbor gather of the formal construction.
The general theorem says any weights implement weighted
BP on some implicit factor graph — so FV heads are
technically covered. But what the implicit factor graph
looks like for a diffuse head, and whether the computation
has a meaningful BP interpretation, is not established.

This is the most important gap. FV heads dominate at
scale, so if the BP account cannot characterize them
it is missing the dominant behavior of large models.

---

## Name Mover Head

Status: fully covered.

Entity-copying is a specific instance of the gather step:
find the node corresponding to a named entity, retrieve
its belief, route it to the output position. Sharp,
content-addressed, one value copied. The theory covers
this as a routing class with specific (nodeType, ownIndex,
neighborIndex) — the entity's position is the neighbor
index, the output position is the own index.

---

## S-Inhibition Head

Status: covered in principle, not yet formally developed.

Negative evidence — suppressing a candidate — is
accommodated by the log-odds algebra. logit(p) +
logit(1-q) reduces the combined belief when q is high.
The BP framework handles negative messages naturally.
But the shannon paper does not explicitly develop the
negative message case, and the formal weight construction
for inhibition heads is not written down.

Work to do: extend the formal construction to cover
inhibitory messages and verify that S-inhibition heads
instantiate this.

---

## Retrieval Head

Status: covered, with a grounding connection.

Retrieval heads are sharp gather steps over long context.
The BP account is the same as name mover heads and
previous token heads — find the right neighbor, copy
the value. The additional significance is in the
hallucination connection: pruning retrieval heads causes
hallucination, which the theory predicts as grounding
failure. The coverage here is both mechanistic (gather
step) and semantic (grounding).

---

## Duplicate Token Head

Status: fully covered.

Finding where a token appeared before is a gather step
with content-based routing. The result is written into
the residual stream as a prerequisite for a subsequent
gather. Two-round chain, both rounds gather steps.
Covered by the same account as induction heads.

---

## Backup / Redundant Head

Status: covered in principle.

Multiple heads voting for the same belief update is
the OR gate structure — independent evidence sources
combining additively in log-odds space. The theory
explicitly handles multiple independent evidence sources
via the log-odds additivity of the updateBelief function.
The formal construction uses two heads per layer but
the OR decomposition theorem says k-ary OR reduces to
binary chains, so arbitrarily many evidence sources are
handled.

Work to do: explicitly connect redundant heads to the
k-ary OR decomposition result in the paper.

---

## Positional Head

Status: partially covered.

Position-based routing is a special case of content-based
routing where the routing key is position rather than
semantic content. The theory accommodates this — position
can be encoded in the token embedding and used as the
routing key just as a semantic index can. But the formal
construction does not develop positional routing explicitly,
and the BP interpretation of globally attending to position
zero (a common pattern) is not spelled out.

Work to do: characterize positional heads as a routing
class and identify what factor graph structure they
correspond to.

---

## Meta-Summary

    type                coverage    work needed
    previous token      full        none
    induction           full        none
    function vector     open        major — dominant at scale
    name mover          full        none
    S-inhibition        partial     extend to negative messages
    retrieval           full        none
    duplicate token     full        none
    backup/redundant    partial     connect to k-ary OR result
    positional          partial     develop positional routing case

Of nine named types, four are fully covered, three are
partially covered, and one is open.

The four fully covered types — previous token, induction,
name mover, retrieval, duplicate token — are all sharp
gather steps. The theory was built around sharp gathering
and covers this class completely.

The partially covered types — S-inhibition, backup,
positional — require extensions of the existing formal
construction but no new theoretical machinery. The
framework accommodates them; the specific formal
development is missing.

The one open type — function vector heads — is the
genuine theoretical gap. It is also the most important
gap because FV heads become dominant at scale. A BP
account of diffuse attention is needed to cover the
behavior of large models. This is the main open problem
at the intersection of the BP theory and the
interpretability literature.

## What the Coverage Says

The theory covers the sharp routing regime completely.
Sharp attention — one neighbor, one value, focused lookup —
is exactly the formal BP construction, and the empirical
literature confirms that this regime accounts for most
named, well-understood circuits.

The theory does not yet cover the diffuse routing regime.
Diffuse attention — weighted mixture over many positions,
abstract task encoding — is the dominant behavior at scale
and the main gap.

The gap is not arbitrary. Sharp routing corresponds to
grounded inference — specific entities, specific facts,
specific rules. Diffuse routing corresponds to ungrounded
inference — abstract patterns, task-level representations,
statistical regularities. The theory is complete for the
grounded case and open for the ungrounded case. That
alignment is not a coincidence.