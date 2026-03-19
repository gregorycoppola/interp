# Open Questions

What the mechanistic interpretability literature has not
yet answered, framed in BP terms.

## 1. The BP interpretation of diffuse attention

Induction heads are sharp and map cleanly onto the BP
gather step. Function vector heads are diffuse and do not.
What is the correct BP interpretation of a weighted
mixture over all W positions? Is it loopy BP on a fully
connected graph, mean-field approximation, or something
else? Does the answer depend on the sharpness of the
attention distribution?

## 2. The implicit factor graph of a trained model

The shannon paper defines the implicit factor graph G(W)
for any sigmoid transformer. What does G(W) look like
for a trained production model? Can it be recovered from
the weights directly, or only empirically via SAEs and
attribution graphs? How does its structure relate to the
training data distribution?

## 3. The concept count

How many concepts does a production transformer have?
The grounded case gives 2n² exactly. For an ungrounded
model, n must be inferred from the weights. SAEs give
a proxy — the dictionary size — but the relationship
between SAE dictionary size and the true routing class
count is not established. Is there a way to count routing
classes directly from the weights without running SAEs?

## 4. Superposition and BP compatibility

Is exact BP compatible with superposition, or does
superposition necessarily introduce approximation? If
features are non-orthogonal, can BP messages still be
passed cleanly between them? Is there a generalization
of the BP weight construction that works for overcomplete
non-orthogonal feature sets?

## 5. The grokking connection

Grokking involves a sharp transition from memorization
to generalization. In BP terms: the model moves from
an approximate weight configuration to the exact BP
weight configuration. Can the grokking transition be
predicted from the factor graph structure? Is grokking
always accompanied by the formation of a specific
interpretable circuit?

## 6. Scaling of circuit types

At small scale, induction heads dominate. At large scale,
function vector heads dominate. What drives this transition?
In BP terms: does the implicit factor graph change
structure as the model scales, or do larger models just
implement the same factor graph more accurately? What
happens to the 16K distinct AND patterns at larger scale
— do they become more or less interpretable?

## 7. The backward pass

The shannon paper and the pearl repo prove that backprop
is also a MessagePassingAlgorithm with the same lower
bound as BP. Can the backward pass of a trained model
be interpreted as lambda messages in Pearl's two-pass
algorithm? Would running the backward pass at inference
time — computing lambda as well as pi — reduce
hallucination on loopy graphs?

## 8. Retrieval heads and grounding

Retrieval heads function as implicit grounding. Can
they be made explicit — can a model be modified to
use retrieval heads as a declared grounding mechanism,
connecting tokens to specific entries in a knowledge
base? Would this reduce hallucination on factual queries
in the way the no-hallucination corollary predicts for
grounded models?