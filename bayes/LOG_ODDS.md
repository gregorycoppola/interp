# The Log-Odds Algebra

## The Core Insight

Probabilities multiply when combining independent
evidence. Log-odds add. Addition is simpler than
multiplication and maps naturally onto neural network
linear operations.

The log-odds of a probability p is:

    logit(p) = log(p / (1-p))

High probability maps to large positive log-odds.
Low probability maps to large negative log-odds.
Maximum uncertainty (p = 0.5) maps to zero.

## The Turing-Good Tradition

This algebra was developed not by probabilists but
by cryptanalysts. During World War II, Alan Turing
and I.J. Good at Bletchley Park needed a practical
method for combining independent evidence when
breaking Enigma.

Their insight: for a binary hypothesis H and
independent evidence sources e0 and e1:

    logit(P(H | e0, e1)) = logit(P(H)) + W(H:e0) + W(H:e1)

where W(H:e) = logit(P(H|e)) - logit(P(H)) is the
weight of evidence. Independent evidence contributes
additively to the log-odds. This is not an approximation
— it is exact for independent evidence.

Pearl (1988) applied this to graphical models via
the sum-product algorithm. The shannon paper connects
this tradition to the transformer for the first time.

## The Algebra

Log-odds addition defines a binary operation on (0,1):

    m0 ⊕ m1 = sigma(logit(m0) + logit(m1))

Properties:
    commutative:   m0 ⊕ m1 = m1 ⊕ m0
    associative:   (m0 ⊕ m1) ⊕ m2 = m0 ⊕ (m1 ⊕ m2)
    identity:      m ⊕ 0.5 = m     (0.5 contributes nothing)
    inverse:       m ⊕ (1-m) = 0.5 (opposite evidence cancels)

The identity element is 0.5 — maximum uncertainty.
Combining any belief with a 0.5 belief leaves it
unchanged. This is the formal statement that a
neutral prior contributes nothing.

## Why Sigmoid Is Natural

The sigmoid function sigma: R -> (0,1) is the exact
inverse of logit: sigma(logit(p)) = p.

A sigmoid FFN computing:

    sigma(w0 * logit(m0) + w1 * logit(m1) + b)

is performing weighted log-odds addition and converting
back to probability space in one operation. The sigmoid
is not a design choice motivated by gradient flow. It
is the exact function required to implement the
Turing-Good-Pearl algebra.

This is why the title claim is true: a sigmoid
transformer is a Bayesian network. The sigmoid
activation makes the FFN computation exactly the
weight-of-evidence combination.

## The Boolean Limit

Classical boolean logic is the limit of this algebra
as beliefs become certain.

logit(1) = +infinity  (certain true)
logit(0) = -infinity  (certain false)
logit(0.5) = 0        (no information)

In the limit, the algebra reduces to boolean AND and
OR. But the probabilistic version handles uncertainty
gracefully — instead of FALSE AND TRUE = FALSE, you
get partial evidence combining to partial certainty.
The hard boolean logic is the degenerate case of
the probabilistic algebra.