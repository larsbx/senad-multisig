# Lifecycle Implementation Plan

P0–P2 reuse the root authorization implementation plan and shared schema; they do not create a second evaluator.

## P0 — Freeze bytes before cryptographic integration

1. Freeze CDDL field meanings and numeric keys.
2. Implement deterministic CBOR encoder/validator profile.
3. Produce encoding-only golden vectors.
4. Cross-check vectors in at least two independent implementations.

Acceptance: all implementations produce byte-identical statements and reject forbidden alternate encodings.

## P1 — Registry and policy evaluator

1. Define signer/key registration object.
2. Define proof-of-possession domain separation.
3. Define authenticated registry checkpoint format.
4. Implement pure policy evaluator with no I/O.
5. Add duplicate-signer, role/coalition, and hybrid-component tests.

Acceptance: policy result is a pure deterministic function of authenticated inputs.

## P2 — Signature evidence

1. Add ML-DSA operational evidence profile.
2. Add SLH-DSA identity-critical evidence profile.
3. Keep hybrid companion requirements policy-controlled.
4. Generate positive and negative cryptographic vectors.

Acceptance: independent verifier reproduces every vector result.

## P3 — Succession

1. Signature renewal vectors.
2. Roster and policy cross-vouch vectors.
3. Typed-digest migration.
4. Hash-evidence renewal vectors that rebind archived evidence.
5. Reject old-root-only pseudo-renewal where old collision resistance remains security-critical.

## P4 — Envelope lifecycle

1. ML-KEM envelope profile.
2. Stable `BlobID` / mutable `KeyEnvelope` split.
3. Re-wrap tests.
4. AEAD/symmetric succession supersession links.
5. key-copy inventory and crypto-shred drill.

## P5 — Threshold/aggregation experiments

Only after P0–P4 are stable:

- evaluate threshold or aggregate mechanisms as evidence optimizations;
- prove semantic equivalence to the policy evaluator;
- separately decide whether T2 requires accountable participant disclosure.

No threshold protocol is permitted to redefine quorum semantics.
