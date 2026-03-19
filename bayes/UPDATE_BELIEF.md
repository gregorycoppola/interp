# The updateBelief Function

## What It Is

updateBelief is the core computation of belief
propagation on binary variables. It combines two
independent incoming beliefs into a single updated
belief:

    updateBelief(m0, m1) = m0*m1 / (m0*m1 + (1-m0)*(1-m1))
                         = sigma(logit(m0) + logit(m1))

Both forms are the same computation. The first is
probability space. The second is log-odds space.

## Where It Comes From

Start with Bayes' rule. You have a hypothesis H.
You observe two independent pieces of evidence,
e0 and e1. Starting from a uniform prior P(H) = 0.5:

    P(H | e0, e1) = P(e0 | H) * P(e1 | H) * P(H) / Z

where Z is a normalization constant. For binary H
and binary evidence with P(H | ei) = mi:

    P(H=1 | e0, e1) = m0 * m1 / (m0*m1 + (1-m0)*(1-m1))

This is updateBelief. It is Bayes' rule applied to
two independent binary evidence sources with a
uniform prior. No approximation. Exact.

## Key Properties

Neutral padding: updateBelief(m, 0.5) = m

Combining any belief with 0.5 (maximum uncertainty,
no information) leaves the belief unchanged. This
means unused attention heads — heads that attend to
uninformative positions — do not corrupt the belief.
Padding with 0.5 is the identity operation.

This property is proved formally in hard-bp-lean as
updateBelief_neutral and is used in the transformer
implementation proof to handle the case where a head
does not find a relevant neighbor.

Symmetry: updateBelief(m0, m1) = updateBelief(m1, m0)

The order of evidence does not matter. Two independent
sources combine commutatively.

Certainty propagation: updateBelief(1, m) = 1 for any m

If one source is certain, the combined belief is
certain. This is correct for independent evidence —
one certain source is sufficient.

## In the Transformer

The FFN with BP weights computes updateBelief from
the two beliefs gathered by the two attention heads:

    dim 4 holds neighbor 0's belief (from head 0)
    dim 5 holds neighbor 1's belief (from head 1)
    FFN computes sigma(logit(dim4) + logit(dim5))
    result written to dim 0

This is the formal construction in transformer-bp-lean.
The FFN is not doing arbitrary function approximation.
It is computing one specific function — updateBelief —
with a specific algebraic structure that follows from
Bayes' rule.

## The Weighted Generalization

The general version allows unequal weights:

    sigma(w0 * logit(m0) + w1 * logit(m1) + b)

where w0 and w1 are the relative weights of the two
evidence sources and b is a prior bias. This is the
general Ψor function from the QBBN.

The equal-weight case (w0 = w1 = 1, b = 0) is exact
BP. The weighted case is the general sigmoid transformer
— any weights implement weighted BP on the implicit
factor graph, where the weights encode the relative
reliability of the two evidence sources.

The uniqueness theorem says: if you want exact Bayesian
posteriors, you must have w0 = w1 = 1, b = 0. Any
deviation from these weights introduces systematic
bias. The equal-weight case is the unique solution.