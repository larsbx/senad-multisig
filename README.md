# senad-multisig

Authorization-policy and multisignature evidence layer for Senad+.

This repository defines **what it means for a set of signers to authorize an action** independently of any one signature algorithm, threshold protocol, coordinator, or aggregation format.

## Design principles

- Quorum semantics live in a versioned policy, not in a cryptographic primitive.
- Every counted signer signs the exact same canonical statement.
- The statement commits to the policy, signer roster, suite epoch, governance epoch, object root, and stream predecessor.
- Signer eligibility is proven against authenticated registry/checkpoint state, not a self-asserted signing time.
- One signer counts once even if that signer supplies multiple signature algorithms in a hybrid epoch.
- The coordinator is untrusted.
- Roster/policy succession cross-vouches one exact transition object.
- Future threshold or aggregate schemes are optional optimizations and must not silently redefine authorization semantics.
- No fixed m-of-n governance policy is assumed by this repository.

## Scope

Initial scope:

- canonical multisig statements
- typed digests and domain separation
- signer registries and authenticated checkpoints
- deterministic policy evaluation
- independent signature evidence
- policy and roster rollover
- adversarial golden vectors
- optional accountable-participant evidence

Out of initial scope:

- distributed key generation
- threshold signing protocols
- wallet/UI coordination
- transport protocols

See [docs/SPEC.md](docs/SPEC.md) and [IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md).
