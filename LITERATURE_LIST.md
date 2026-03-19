# Empirical Inventory

A flat catalog of named findings from the mechanistic
interpretability literature, tagged by their relationship
to the Transformers are Bayesian Networks framework.

BP concepts used as tags:
- gather: the attention step that retrieves a neighbor's belief
- update: the FFN step that computes a new belief from gathered evidence
- factor-potential: the weights encoding a relationship between nodes
- routing-class: a distinct inferential identity in the implicit factor graph
- concept-node: a specific node in the implicit factor graph being active
- multi-round: a computation requiring multiple BP rounds
- phase-structure: the discrete emergence of routing structure during training
- grounding: the connection between a token and a declared entity

Stance toward BP account:
- confirms: finding is predicted by or consistent with BP account
- challenges: finding is in tension with BP account
- open: finding is relevant but relationship to BP account is not yet clear

---

| Finding | Paper / Year | What was found | BP concept | Stance |
|---|---|---|---|---|
| Previous token head | Elhage et al. 2021 | Attention head that sharply attends to the immediately preceding token and copies it into the residual stream | gather | confirms |
| Induction head | Olsson et al. 2022 | Attention head that finds the previous occurrence of the current token and copies the following token's value | gather | confirms |
| Induction circuit | Olsson et al. 2022 | Two-layer circuit: previous token head feeds induction head, together implementing pattern match and copy | multi-round | confirms |
| ICL phase transition | Olsson et al. 2022 | Induction heads emerge in a sharp discontinuous transition during training, coinciding with sudden ICL improvement | phase-structure | confirms |
| Name mover heads | Wang et al. 2022 | Heads in the IOI circuit that copy the indirect object name to the output position | gather | confirms |
| S-inhibition heads | Wang et al. 2022 | Heads that suppress the subject name as a candidate answer in the IOI task | update | confirms |
| Duplicate token heads | Wang et al. 2022 | Heads that find where the subject name appeared earlier in context | gather | confirms |
| IOI circuit | Wang et al. 2022 | Full end-to-end circuit for indirect object identification in GPT-2 Small, involving ~26 heads across multiple layers | multi-round | confirms |
| Function vector heads | Todd et al. 2024 | Heads encoding a latent task representation rather than copying specific tokens; diffuse attention pattern | gather | open |
| Induction-to-FV transition | Todd et al. 2024 | Many FV heads start as induction heads during training and transition to the FV mechanism | phase-structure | open |
| FV scale dependence | Todd et al. 2024 | FV heads become more important than induction heads at larger model scales | gather | open |
| Modular arithmetic circuit | Nanda et al. 2023 | Transformers implement modular addition via Fourier representations and interference patterns in embedding space | update | open |
| Grokking transition | Nanda et al. 2023 | Sharp transition from memorization to generalization coinciding with formation of the modular arithmetic circuit | phase-structure | confirms |
| Docstring circuit | Various | Circuit for completing Python docstrings by copying argument names from function signature via sharp attention | gather | confirms |
| Greater-than circuit | Hanna et al. 2023 | Circuit implementing year comparison via specific attention heads identifying decade and unit digits | multi-round | confirms |
| FFN as key-value memory | Geva et al. 2021 | FFN hidden units function as key-value memories: keys are input patterns, values are output distributions | factor-potential | confirms |
| Factual association localization | Meng et al. 2022 | Factual associations are stored in specific FFN layers at specific token positions, editable via ROME | factor-potential | confirms |
| ROME editing | Meng et al. 2022 | Directly editing FFN weights changes the model's factual beliefs in a targeted and predictable way | factor-potential | confirms |
| Retrieval heads | Wu et al. 2024 | Specific attention heads responsible for retrieving facts across long contexts; pruning causes hallucination | gather, grounding | confirms |
| Pruning causes hallucination | Wu et al. 2024 | Removing retrieval heads causes the model to generate unsupported content; non-retrieval head pruning does not | grounding | confirms |
| SAE monosemantic features | Bricken et al. 2023 | SAEs trained on one-layer MLP find thousands of interpretable monosemantic features in superposition | concept-node | confirms |
| Scaling monosemanticity | Anthropic 2024 | SAEs on Claude Sonnet find millions of interpretable features including highly specific concepts | concept-node | confirms |
| Golden Gate Bridge feature | Anthropic 2024 | One SAE feature corresponding specifically to the Golden Gate Bridge; clamping it causes model-wide obsession | concept-node | confirms |
| Feature clamping effect | Anthropic 2024 | Forcing one feature to maximum activation corrupts all downstream inference, routing every response through that concept | concept-node, multi-round | confirms |
| Attribution graphs | Anthropic 2025 | Tracing how features activate other features across layers, revealing the edge structure of the implicit factor graph | factor-potential | confirms |
| Superposition hypothesis | Elhage et al. 2022 | Models represent more features than dimensions by encoding features as non-orthogonal directions | concept-node | open |
| Polysemanticity | Elhage et al. 2022 | Individual neurons respond to multiple unrelated concepts due to superposition | routing-class | open |
| Universality of induction heads | Olsson et al. 2022 | Induction heads appear in every transformer language model studied regardless of size or training data | routing-class | confirms |
| Universality of curve detectors | Olah et al. 2020 | Curve detector features appear in every vision model studied | routing-class | confirms |
| Circuit reuse in IOI variants | Wang et al. 2022 | IOI circuit shows 92-100% component reuse across sentence variants | routing-class | confirms |
| Attention head specialization | Various | A small number of distinct head types account for most named behaviors; long tail of less interpretable heads | routing-class | open |
| Depth / algorithm correspondence | Sanford et al. 2024 | One-layer transformers cannot solve induction efficiently; two layers required | multi-round | confirms |
| Two-layer induction necessity | Ekbote et al. 2025 | Two-layer transformers provably represent induction heads on any-order Markov chains; one layer cannot | multi-round | confirms |