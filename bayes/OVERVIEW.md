# Bayesian Networks: An Overview

A Bayesian network is a way of encoding what you know
and reasoning about what you don't know. It is a graph
where nodes are uncertain quantities and edges are
relationships between them. Given some observations,
the network computes the probability of everything else.

## The Basic Idea

Suppose you want to know whether it will rain. You know
two things: whether there are clouds, and whether the
forecast said rain. Each piece of evidence is uncertain.
Each influences your belief about rain. A Bayesian
network encodes these relationships and combines the
evidence correctly.

The key word is correctly. Not approximately. Not by
heuristic. By the rules of probability theory — Bayes'
rule applied systematically across the whole network.

## Why Graphs

The graph structure encodes independence assumptions.
If clouds and forecast are independent given rain —
knowing rain tells you everything about both, so they
don't influence each other directly — you can draw
that in the graph by having both point to rain but
not to each other.

Independence assumptions are what make inference
tractable. A fully general joint distribution over
N variables requires 2^N numbers to specify. A
Bayesian network with independence structure requires
far fewer — one small table per node, encoding how
it depends on its parents.

## What Bayesian Networks Are For

Diagnosis: given symptoms, what is the probability
of each disease? The symptoms are observed evidence.
The diseases are hidden variables. The network
propagates evidence backward from symptoms to causes.

Prediction: given causes, what are the likely effects?
The causes are observed. The effects are queried.
The network propagates evidence forward from causes
to effects.

Explanation: given an observation, which cause best
explains it? The network computes the posterior
probability of each cause given the evidence.

These are not three different things. They are three
directions of inference on the same network. Forward,
backward, and abductive. The network handles all three
with the same algorithm.

## The Connection to Transformers

The shannon paper proves that a sigmoid transformer
is a Bayesian network. Not that it approximates one,
or that it behaves like one — that it is one. The
weights define the network. The forward pass runs
inference on it.

Understanding Bayesian networks is therefore the same
as understanding what transformers compute. The
transformer vocabulary — attention, FFN, residual
stream, layers — is the implementation language.
The Bayesian network vocabulary — nodes, edges, beliefs,
messages, rounds — is the computational language.
This directory teaches the computational language.