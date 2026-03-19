# Future Work

The open theoretical problems at the intersection of the
BP framework and the mechanistic interpretability literature.
Ranked by importance.

Each file describes one open problem: what is missing,
why it matters, and what a solution would look like.

## The Four Open Problems

    1. FUNCTION_VECTORS.md      BP account of diffuse attention
    2. NEGATIVE_MESSAGES.md     formal construction for inhibition
    3. POSITIONAL_ROUTING.md    positional heads as routing classes
    4. REDUNDANT_HEADS.md       connect to k-ary OR decomposition

## Priority

Problem 1 is the dominant open problem. Function vector
heads are the main behavior of large models and the
BP framework has no clean account of them. Solving this
would extend the theory from the grounded sharp-routing
regime to the ungrounded diffuse-routing regime.

Problems 2, 3, and 4 are extensions of the existing
framework rather than genuinely new problems. The theory
accommodates them in principle. The formal development
is missing. Each is probably tractable without new
theoretical machinery.

## What Solving Problem 1 Would Mean

A BP account of diffuse attention would mean: given a
head with attention weights spread across many positions,
characterize the implicit factor graph node and edge
structure that this head is implementing BP on. This
would require either showing that soft attention over W
positions is exact BP on a fully connected graph with
those edge weights, or identifying what approximation
it corresponds to and bounding the error.

If solvable, it would extend the no-hallucination
corollary to the ungrounded case — or explain precisely
why it cannot be extended, which would itself be a
significant result.