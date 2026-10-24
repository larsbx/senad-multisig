# Worked T2 Cryptographic Schedule

## Status

This document is a worked schedule framework. It intentionally does **not** invent quorum numbers or signer classes. Those values become normative only when a T2 governance policy explicitly adopts them.

## Phase T2.0 — Pre-data acceptance

Required before sensitive payload creation:

- envelope/KEM suite selected and self-described;
- plaintext leakage scan passes;
- DEK generation and custody path tested;
- re-wrap preserves blob address;
- crypto-shred inventory test demonstrates all recovery paths can be enumerated;
- deterministic encoding vectors pass.

Exit condition: the system can prove that a sample payload never appears outside the intended encrypted boundary and that the blob can be addressed independently of its key wrapper.

## Phase T2.1 — Identity-critical authority bootstrap

- create initial signer registry;
- require proof of possession for registered signing keys;
- bind roles/purposes to authenticated registry state;
- establish identity-critical signature profile;
- emit genesis authorization checkpoint.

## Phase T2.2 — Operational multisignature

- implement canonical `MultisigStatement`;
- implement deterministic policy evaluator;
- collect independent signature evidence;
- reject duplicate signer counting;
- reject mismatched statement bytes;
- evaluate eligibility against authenticated checkpoints.

The actual quorum expression is a versioned policy input.

## Phase T2.3 — Succession

Exercise independently:

1. signature-suite renewal without re-signing historical payloads;
2. signer-roster rollover with incumbent authorization and successor acknowledgment over the same transition bytes;
3. policy rollover;
4. hash-evidence renewal that rebinds archived evidence under a successor hash;
5. envelope re-wrap with stable `BlobID`;
6. AEAD/symmetric succession via new ciphertext plus supersession link.

## Phase T2.4 — Recovery and erasure

- exercise recovery-policy authorization;
- prove recovery cannot bypass checkpoint/policy rules;
- perform subject-key destruction drill;
- verify all enumerated DEK recovery paths fail afterward;
- retain the destruction evidence without retaining decrypting material.

## Confidence epoch

Classical hybrid components remain controlled by policy criteria, not a guessed retirement date. A later identity-critical amendment changes the required per-signer suite profile.
