# Threat Model

The initial design assumes the following adversarial capabilities:

- malicious or compromised coordinator
- duplicate signature submission
- signer identity aliasing attempts
- policy substitution
- roster substitution
- replay across streams or epochs
- rollback to an older predecessor
- backdated signer claims
- use of a revoked or not-yet-valid key
- mixed signatures over near-identical but different statements
- hybrid-suite double counting
- partial compromise of signer classes
- transition equivocation

The design does not assume that a coordinator, wall-clock timestamp supplied by a signer, or cached evaluation result is trustworthy.

Cryptographic compromise and suite succession are handled by the broader Senad+ crypto-agility layer; this repository defines the authorization/evidence semantics that those suite transitions must preserve.
