# The Three Softmaxes

There are three softmaxes in a transformer. They do
three completely different jobs. Conflating them is
the most common source of confusion about what
transformers compute.

## Softmax 1: Attention (Routing)

Location: inside each attention head, applied to the
W query-key dot products.

Job: routing — which token position should I look at?
This is a differentiable argmax. It concentrates weight
on the position whose key best matches the current query.

BP meaning: the gather step. The softmax implements
content-based routing in the factor graph — finding
the neighbor whose belief to retrieve.

Sharp vs diffuse: if the softmax is sharp (one position
gets most of the weight), the head is doing exact BP
gather. If diffuse (weight spread across many positions),
the head is doing something softer — a weighted mixture
that the formal BP construction does not directly cover.

## Softmax 2: The FFN (Inference)

Location: the sigmoid activation inside the FFN hidden
layer (in the sigmoid transformer of the formal proof).

Job: Bayesian inference — what is my updated belief
given the gathered evidence?

    updateBelief(m0, m1) = sigma(logit(m0) + logit(m1))

This is the Turing-Good algebra of independent evidence:
log-odds add for independent evidence sources, sigmoid
converts back to probability. This is not routing. This
is inference.

BP meaning: the update step. The sigmoid is the exact
inverse of logit — it is the function required to
implement the Bayesian update rule for binary
independent evidence. The formal BP proof requires
sigmoid here specifically.

Note: production models use SwiGLU rather than sigmoid.
SwiGLU preserves the qualitative OR structure but
the exact BP weight correspondence requires extension.

## Softmax 3: Output (Generation)

Location: the final layer, applied to the D-dimensional
output vector projected to vocabulary size (128K).

Job: generation — which token comes next?

    P(next token) = softmax(W_out * x_final)

This is a probability distribution over the vocabulary.
In a standard LLM the logits are computed by pattern
matching against the final hidden state. In a QBBN
transformer the output is the marginal distribution
over the truth value of each proposition, computed
by integrating out all other propositions via BP.

BP meaning: the output distribution. In the grounded
case this is the exact marginal posterior at the query
node. In the ungrounded case it is whatever the implicit
factor graph computes.

## The Key Distinction

    Softmax 1: routing     — where to look
    Softmax 2: inference   — what to conclude
    Softmax 3: generation  — what to output

The QBBN is not replacing any of the three softmaxes.
It is replacing the logits fed into Softmax 3 — instead
of pattern-matched logits from an ungrounded hidden
state, they are exact BP marginals from a grounded
factor graph.

The architecture was always capable of exact inference.
It just needed the right logits.