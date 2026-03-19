# Attention Head Typology

A meta-analysis of the distinct functional types of attention
head found in the mechanistic interpretability literature.
Not a list of papers — a catalog of the types themselves.

The central question: how many distinct kinds of attention
head exist, how common is each, and is the set closed or
open?

---

## Type 1: Previous Token Head

What it does: attends sharply to the immediately preceding
token position and copies its value into the current
residual stream.

How common: universal. Found in every transformer studied.
Appears in early layers. Often the first named head type
to emerge during training.

Set status: closed. This is a fully characterized type.
One specific behavior, one specific attention pattern,
well understood mechanistically.

BP interpretation: direct implementation of the gather
step. One neighbor, sharp attention, copy one value.
Closest to the formal projectDim/crossProject construction.

---

## Type 2: Induction Head

What it does: finds the previous occurrence of the current
token and copies the value of the token that followed it.
Implements [A][B]...[A] -> [B].

How common: universal. Found in every transformer studied
regardless of size or architecture. Emerges in a sharp
phase transition coinciding with in-context learning
improvement.

Set status: closed. Fully characterized. Two-layer circuit
with previous token head as prerequisite.

BP interpretation: two-round BP inference chain. Round 1:
previous token head writes shifted identity into residual
stream. Round 2: induction head matches against it and
copies the following value. Minimum two layers required —
consistent with Peirce lower bound.

---

## Type 3: Function Vector Head

What it does: encodes a latent representation of the entire
task being demonstrated in context. Diffuse attention
pattern — reads across many positions rather than attending
sharply to one.

How common: present in all models studied, increasingly
dominant at larger scales. In small models induction heads
drive ICL; in large models FV heads drive it.

Set status: partially characterized. The behavior is clear
but the mechanism is less well understood than induction
heads. Many FV heads start as induction heads during
training and transition to FV behavior.

BP interpretation: open. Diffuse attention over W positions
is a weighted mixture of W neighbors' beliefs. This does
not map cleanly onto the sharp one-neighbor gather of the
formal BP construction. Closest candidate is loopy BP on
a fully connected graph with attention weights as edge
weights — but this is not yet established.

---

## Type 4: Name Mover Head

What it does: copies a specific named entity (a person's
name, an indirect object) from where it appears in context
to the output position. Part of the IOI circuit.

How common: specific to tasks requiring entity tracking
and copying. Found in GPT-2 Small and other models on IOI-
type tasks. Likely present in any model trained on text
with named entities.

Set status: closed within the IOI circuit. Whether this
generalizes to a broader class of entity-copying heads
is open.

BP interpretation: gather step for a specific entity node.
Retrieves the belief at the entity's position and routes
it to where it is needed for the output. Sharp, focused,
one entity at a time.

---

## Type 5: S-Inhibition Head

What it does: suppresses a candidate answer by writing
a signal that inhibits name mover heads from copying the
subject name. Implements negative evidence in the IOI
circuit.

How common: specific to tasks requiring disambiguation
between competing candidates. Present in IOI circuit.
Likely a specific instance of a broader class of
inhibition or suppression heads.

Set status: open. S-inhibition is one named instance.
Whether there is a general class of inhibition heads
with common mechanism is not established.

BP interpretation: update step that reduces belief in
a candidate node. Passing a negative message — evidence
against a conclusion rather than for it. The BP framework
accommodates negative evidence naturally via the logit
algebra; logit(p) + logit(1-q) can reduce the combined
belief.

---

## Type 6: Retrieval Head

What it does: retrieves specific factual information from
distant positions in a long context. Sharp attention,
content-addressed, copies one specific fact forward.
Pruning causes hallucination.

How common: present in models with long context ability.
Likely a scaled-up version of the induction/name-mover
pattern operating over longer distances.

Set status: partially characterized. Identified as a
functional class but mechanism less fully reverse
engineered than induction heads.

BP interpretation: gather step for grounding. The head
is retrieving a specific piece of evidence from the
context and routing it to where it is needed for
inference. Loss of retrieval heads causes hallucination —
consistent with the BP account of hallucination as
grounding failure.

---

## Type 7: Duplicate Token Head

What it does: identifies positions where the current
token (or a semantically related token) appeared
previously in context. Writes a signal marking that
a repetition occurred.

How common: part of the IOI circuit. Likely present in
any model that needs to track entity repetition.

Set status: closed within IOI. Broader class unclear.

BP interpretation: gather step for identity matching.
Finds where a node has appeared before and writes that
information into the residual stream for downstream
heads to use. A prerequisite gather before a conclusion-
drawing update.

---

## Type 8: Backup Head

What it does: performs the same function as a primary
head (e.g. name mover) but fires when the primary head
is ablated or fails. Implements redundancy.

How common: found in the IOI circuit. Likely a general
phenomenon — models appear to implement redundant
circuits for important computations.

Set status: open. Redundancy as a general phenomenon
is documented but the full extent is not cataloged.

BP interpretation: parallel BP paths to the same
conclusion. Multiple heads voting for the same belief
update, with the combined evidence being more robust
than any single head. Consistent with the OR gate
structure — multiple independent evidence sources
combining additively in log-odds space.

---

## Type 9: Positional Head

What it does: attends primarily based on relative or
absolute position rather than content. Implements
position-sensitive operations like attending to the
first token, the last token, or a fixed offset.

How common: found across many models. Position-zero
heads (attending to the first token) are common and
appear to implement a form of global context storage.

Set status: partially characterized. Several named
positional patterns are documented but not
systematically cataloged.

BP interpretation: gather step with positional rather
than content-based routing. The routing key is position
rather than content — a special case of the general
routing where the Q·K score peaks at a specific offset
regardless of content.

---

## Summary

    type                    common?     closed?     BP concept
    previous token          universal   yes         gather (sharp)
    induction               universal   yes         gather (multi-round)
    function vector         universal   partial     gather (diffuse) — open
    name mover              task-specific yes        gather (entity)
    S-inhibition            task-specific partial    update (negative)
    retrieval               long-context partial     gather (grounding)
    duplicate token         task-specific yes        gather (identity)
    backup / redundant      general     open        OR (parallel evidence)
    positional              common      partial     gather (positional)

## How Many Types Are There?

The set is not closed. The nine types above are the named
and relatively well-characterized ones. The literature
consistently finds a long tail of attention heads whose
function is not clearly identified. Estimates suggest
that named, well-understood head types account for a
minority of all heads in a large model — perhaps 10-20%
in small models, less in large ones.

The 16,128 distinct AND weight patterns in Llama 3 405B
(L * H = 126 * 128) are instantiating some distribution
over these types. How many of each type, and what the
full typology looks like at that scale, is an open
empirical question.

The BP framework predicts the typology should be
organized around routing patterns — what information
is being gathered from where. The empirical typology
broadly confirms this: most named head types are
distinguished by what they attend to and what they
copy, not by how they process what they receive.
The processing (FFN/update step) is less well
characterized by head type.