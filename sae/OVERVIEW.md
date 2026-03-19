# Sparse Autoencoders

This directory covers sparse autoencoders (SAEs) as the
primary empirical tool for recovering the implicit factor
graph of a trained transformer.

The central claim: SAE features are the empirical face
of the implicit routing classes predicted by the BP
framework. The SAE dictionary is an empirical attempt
to recover the 2n² concept structure from a trained
ungrounded model.

## Files

    SAE_OVERVIEW.md         what SAEs are and why they exist
    SAE_AND_BP.md           SAE features in BP terms
    SAE_FINDINGS.md         major empirical results
    SAE_OPEN_QUESTIONS.md   what remains unknown

## The One-Sentence Version

Superposition hides the factor graph inside the weights.
SAEs unhide it.