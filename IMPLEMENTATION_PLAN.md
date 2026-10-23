# Implementation Plan

## Phase 1 — Freeze the semantic core

1. assign stable field labels for the canonical statement
2. freeze the deterministic CBOR profile
3. define typed digest identifiers
4. define domain-separation strings
5. generate byte-exact encoding vectors

## Phase 2 — Registry and eligibility

1. define signer registration artifact
2. define proof-of-possession requirements
3. define authenticated registry checkpoints
4. define authorization epochs / ordering semantics
5. test revocation, retirement, and replacement boundaries

## Phase 3 — Policy evaluator

Implement a pure deterministic evaluator with no network callbacks.

Minimum supported constraint families:

- distinct signer thresholds
- role/trust-class minima and maxima
- mandatory signer or class requirements
- excluded coalitions
- signature-profile requirements
- transition rules
- accountable-participant requirements

## Phase 4 — Independent signature evidence

Verify each signature entry independently against the exact same `ToBeSigned` bytes and checkpoint-derived eligibility state.

One `SignerID` counts at most once.

## Phase 5 — Succession

Implement canonical policy/roster transitions with incumbent authorization and successor acknowledgement over one exact transition object.

## Phase 6 — Golden vectors

Add the positive and adversarial cases in `vectors/README.md`.

## Phase 7 — Optional optimizations

Only after the semantic and replay layers are stable:

- coordinator service
- aggregate signature encodings
- threshold signing
- DKG experiments

These optimizations must preserve authorization semantics rather than define them.
