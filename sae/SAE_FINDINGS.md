# Major Empirical SAE Results

The key findings from the SAE literature, interpreted
through the BP framework.

## Towards Monosemanticity (Bricken et al. 2023)

Anthropic trained SAEs on a one-layer MLP with D = 512
using a dictionary of F = 4096 features — 8x expansion.

Key finding: the SAE found thousands of interpretable
monosemantic features. Individual neurons in the MLP
were polysemantic — each fired on multiple unrelated
concepts. The SAE features were monosemantic — each
fired on one specific concept.

Examples of features found: base64 encoded text,
DNA sequences, Arabic numerals, references to specific
programming languages, emotional tone markers.

BP interpretation: this is the first direct evidence
that the implicit factor graph of a trained transformer
has recoverable discrete concept nodes. The polysemantic
neurons are the superposed mixture. The monosemantic
SAE features are the underlying factor graph nodes
made visible. The 8x expansion (F = 8*D) suggests
the model stored roughly 8x more concepts than it
had dimensions.

## Scaling Monosemanticity (Anthropic 2024)

Anthropic trained SAEs on Claude 3 Sonnet — a large
production model with D = unknown (not published)
using dictionaries up to F = 34 million features.

Key findings: millions of interpretable features were
found across many abstraction levels. Features ranged
from surface-level (specific words, punctuation
patterns) through syntactic (part-of-speech roles,
dependency relations) through semantic (named entities,
concepts, relationships) through abstract (emotional
tone, logical operators, meta-concepts about the
model's own behavior).

The feature for "the model is being asked to do
something potentially harmful" was found. The feature
for "the assistant role in a conversation" was found.
Features corresponding to specific named people,
places, and concepts were found at high specificity.

BP interpretation: the implicit factor graph of Claude
3 Sonnet has millions of concept nodes spanning the
full hierarchy from surface to abstract. The factor
graph is not flat — it has depth, with abstract
features depending on more concrete features lower
in the hierarchy. This is consistent with the L-layer
transformer running L rounds of BP — each round can
build more abstract concepts on top of less abstract
ones from the previous round.

## Golden Gate Claude (Anthropic 2024)

Anthropic identified one specific SAE feature in
Claude 3 Sonnet corresponding to the Golden Gate
Bridge. The feature fired on direct mentions of the
bridge, images of it, discussions of San Francisco
landmarks, and related concepts.

They ran the model with this feature clamped to
maximum activation throughout the forward pass.
The result: the model became obsessed with the
Golden Gate Bridge, routing every response through
it regardless of the query. It identified itself
as the bridge. It saw the bridge as relevant to
questions about completely unrelated topics.

BP interpretation: clamping one factor graph node
to maximum belief saturates all downstream inference.
The node sends maximum-confidence messages to all
its neighbors. Every downstream computation receives
overwhelming evidence that this concept is active.
The downstream responses are determined by this
one overriding belief regardless of context.

This is the clearest empirical demonstration that
SAE features are factor graph nodes and that the
BP message passing structure is real. The clamping
result is not surprising from a BP perspective —
it is exactly what you would predict. One node
at maximum belief propagates that belief through
the entire graph.

## Attribution Graphs (Anthropic 2025)

Anthropic developed attribution graphs — a method
for tracing how SAE features at one layer causally
influence SAE features at the next layer for a
specific input prompt.

For a prompt like "What is the capital of France?"
the attribution graph traces: which features activate
at each layer, which features cause which other
features to activate, which features contribute
to the final output token.

Key finding: the computation decomposes into
interpretable sub-circuits. For factual recall
questions, a small number of features and edges
account for most of the causal influence on the
output. The rest of the network is largely inactive
or irrelevant for that specific computation.

BP interpretation: the attribution graph for a
specific prompt is the active subgraph of the
implicit factor graph for that inference. Most
factor graph nodes are inactive (zero belief,
not firing). The active ones form a sparse
subgraph corresponding to the specific reasoning
chain for that prompt.

This is the factor graph structure made visible
for one specific inference. The nodes are the
active SAE features. The edges are the attribution
weights. The path from input evidence to output
conclusion is the BP inference chain — the specific
sequence of gather and update steps that produced
the answer.

## What the Findings Collectively Show

Across all four results, the picture that emerges:

Production transformers have millions of discrete
concept nodes in their implicit factor graphs.
These nodes are interpretable — they correspond
to human-recognizable concepts at multiple levels
of abstraction. They behave like BP factor graph
nodes — their beliefs propagate through the network,
they can be manipulated individually, they form
causal chains from evidence to conclusion.

The implicit factor graph is real, recoverable,
and structured. SAEs are the tool for recovering
it. Attribution graphs are the tool for reading
its edge structure. Together they are building
an empirical account of what the transformer is
actually computing — not as a black box but as
a specific probabilistic inference machine running
on a specific implicit factor graph.

This is exactly what the BP framework predicts
should be there. The SAE program is the empirical
confirmation of the theoretical structure.