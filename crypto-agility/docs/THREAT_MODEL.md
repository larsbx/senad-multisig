# Threat Model

## Protected properties

The design aims to preserve:

- confidentiality of stored payloads;
- deterministic replay and verification;
- authenticated cryptographic-suite identity;
- long-term evidence continuity across signature and hash succession;
- signer-policy integrity;
- rollback/fork detection within each sequence stream;
- stable ciphertext addressing across key re-wraps; and
- auditable crypto-shredding claims.

## Adversaries

Assume attackers may:

- retain ciphertext indefinitely;
- compromise a cryptographic primitive after historical use;
- compromise individual signer keys;
- operate or subvert the signature coordinator;
- reorder, duplicate, suppress, or splice evidence objects;
- substitute policies or signer rosters;
- backdate unauthenticated timestamps;
- retain stale key wrappers or backups;
- exploit ambiguous serialization; and
- attempt downgrade to older suite epochs.

## Explicit non-assumptions

The system does not assume:

- coordinators are trusted;
- current cryptographic algorithms remain secure forever;
- a wall-clock timestamp asserted by a signer proves authorization ordering;
- deleting a single key wrapper proves cryptographic erasure;
- an old root wrapped by a new hash repairs a broken old content-binding hash; or
- a threshold-signature implementation defines governance semantics.

## Critical failure conditions

Security claims fail if any of the following occur without compensating evidence:

1. sensitive plaintext is produced before an accepted envelope path exists;
2. a predecessor signature/hash is renewed only after it is already untrustworthy;
3. hash succession preserves only an old digest whose collision resistance is now relied upon;
4. decryption-capable key material survives a claimed crypto-shred event;
5. verifier implementations accept multiple byte encodings for the same normative statement;
6. signer eligibility is inferred from claimed timestamps instead of authenticated checkpoints;
7. a coordinator can alter the policy/roster after signatures are collected; or
8. hybrid algorithm components are counted as independent signers.
