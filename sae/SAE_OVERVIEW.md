# What SAEs Are and Why They Exist

## The Problem: Superposition

A D-dimensional vector has D orthogonal directions —
D axes that are completely independent. If you store
one concept per axis you can store exactly D concepts
with zero interference.

But trained transformers store far more than D concepts
in a D-dimensional residual stream. They do this by
using non-orthogonal directions — directions that are
not perpendicular to each other. You can fit far more
than D nearly-orthogonal directions in D-dimensional
space. The number grows exponentially with D.

The cost is interference. When one concept fires it
partially activates nearby directions. When you read
the residual stream you see a dense mixture of many
overlapping concept activations — not clean independent
signals but a superposed blur.

This is superposition. The model is trading interference
for capacity — storing more concepts than it has
dimensions, accepting some cross-talk as the price.

## The Solution: Sparse Coding

Superposition works because concepts are sparse. Most
concepts are inactive at any given token position. Only
a small fraction fire simultaneously. If you know the
concepts are sparse, you can recover them from the
mixture.

Sparse coding is the mathematical framework for this.
Given a dense signal that is a sparse mixture of
known directions, find which directions are active
and how strongly. This is a well-studied problem in
signal processing — it is how compressed sensing works,
how independent component analysis works, how the
brain is believed to represent sensory information.

## The SAE Architecture

A sparse autoencoder applies sparse coding to the
residual stream.

It has two parts. An encoder takes the D-dimensional
residual stream vector and projects it up to a much
larger space — say F dimensions where F >> D. Each
of the F dimensions is a candidate feature direction.
The encoder applies a ReLU nonlinearity to enforce
sparsity — most of the F activations are zero.

A decoder takes the sparse F-dimensional activation
and projects it back down to D dimensions, trying to
reconstruct the original residual stream vector.

Training minimizes two things simultaneously:
reconstruction error (reconstruct accurately) and
sparsity penalty (keep most activations zero). The
tension between these two objectives forces the SAE
to find the most efficient sparse representation —
the fewest active features that still reconstruct
the signal accurately.

## What the SAE Learns

The decoder weight matrix has shape D x F. Each column
is one feature direction — a D-dimensional vector
pointing in the direction of one concept in residual
stream space.

The encoder learns to detect when each feature direction
is present in the input. The decoder learns to
reconstruct the input from the detected features.

After training, each of the F features has learned to
fire on specific inputs and be silent otherwise. Feature
47 might fire on tokens related to the Golden Gate
Bridge. Feature 1203 might fire on tokens in Python
code. Feature 8847 might fire on tokens expressing
uncertainty.

The features are interpretable because the sparsity
constraint forces monosemanticity — a feature that
fires on many unrelated things cannot be sparse, so
the training pressure pushes each feature toward
one specific meaning.

## Why F >> D

The dictionary size F is a hyperparameter. Anthropic
has trained SAEs with F ranging from 512 to millions
of features on models with D = 4096.

F should be larger than D because the model has
superposed more than D concepts into D dimensions.
The SAE needs enough features to represent all of
them. In practice researchers find that larger
dictionaries find more interpretable features,
suggesting the true concept count is much larger
than D.

The choice of F is related to the concept count
question. In the BP framework, the number of concepts
is 2n² for a grounded model with n nodes. For an
ungrounded model, F is the empirical proxy for 2n² —
the SAE dictionary size is an estimate of how many
distinct routing classes the model has learned.