# Senad+ Multisignature Specification

## 1. Organizing principle

Multisignature is a property of **authorization policy**, not of a particular signature primitive.

A valid authorization consists of:

1. one canonical statement;
2. one committed signer roster;
3. one committed versioned policy;
4. a set of independently verifiable signature entries; and
5. a deterministic proof that the eligible signer set satisfies the policy.

A future threshold or aggregate signature MAY compress or accelerate evidence, but MUST NOT redefine which coalition is authorized to act.

## 2. Core invariants

### MS-1 — Semantic quorum independence

Quorum meaning is defined entirely by a versioned policy artifact. It MUST NOT depend on signature ordering, coordinator behavior, a particular signature algorithm, or possession of a shared group private key.

### MS-2 — One statement, one byte string

Every counted signer MUST sign the same canonical `ToBeSigned` byte string.

Any change to the object root, policy, roster, purpose, suite epoch, governance epoch, stream, sequence, predecessor, or transition data creates a different authorization.

### MS-3 — Policy and roster commitment

The canonical statement MUST commit to:

- `PolicyID`
- `PolicyVersion`
- `SignerRosterRoot`
- `CryptoSuiteEpoch`
- `GovernanceEpoch`

### MS-4 — Untrusted coordinator

The coordinator may distribute signing requests, collect signatures, assemble evidence, and publish bundles.

It MUST NOT be trusted to determine the signed bytes, choose the applicable policy, alter signer eligibility, or declare quorum satisfaction.

### MS-5 — Checkpoint-based eligibility

Signer eligibility MUST be proven against authenticated registry/checkpoint state.

A signer-provided wall-clock timestamp is not sufficient evidence that a key was valid and unrevoked at authorization time.

An authorization SHOULD bind an `AuthorizationEpoch` and `RegistryCheckpoint` (or equivalent authenticated ordering evidence).

### MS-6 — Distinct signer counting

Quorum cardinality is counted by distinct `SignerID`, not by raw signature count.

A signer supplying multiple algorithm components in a hybrid suite contributes one signer toward quorum.

### MS-7 — Succession cross-vouches one transition

Roster and policy rollover MUST use one canonical transition statement.

The incumbent authorization and successor acknowledgement MUST bind to the same transition object.

Historical objects are not re-signed merely because governance membership changes.

### MS-8 — Policy-controlled accountability

The protocol distinguishes:

- **authorization-only**: prove that a qualifying coalition authorized the action;
- **accountable authorization**: additionally preserve participant identities or transcript evidence.

Participant disclosure is therefore a policy/profile requirement, not a universal property of every threshold scheme.

## 3. Canonical statement

Logical fields:

```text
MultisigStatement = {
    Version,
    Domain,
    Purpose,
    Action,
    StreamID,
    ObjectType,
    ObjectRoot,
    PolicyID,
    PolicyVersion,
    SignerRosterRoot,
    CryptoSuiteEpoch,
    GovernanceEpoch,
    AuthorizationEpoch,
    RegistryCheckpoint,
    Sequence,
    PreviousStatementID,
    TransitionData?,
    Context
}
```

`ObjectRoot`, `SignerRosterRoot`, `PreviousStatementID`, and other digest-bearing fields MUST use typed digests:

```text
Digest = {
    AlgorithmID,
    DigestBytes
}
```

The canonical signing bytes are:

```text
ToBeSigned =
    ASCII("SENAD+-MULTISIG-V1") ||
    CanonicalCBOR(MultisigStatement)
```

The canonical CBOR profile MUST freeze exact encoding behavior, including:

- deterministic map ordering
- no duplicate keys
- no indefinite-length items
- no floating point
- NFC-normalized text before encoding
- protocol-defined timestamp representation if timestamps are present

## 4. Signature entry

```text
SignatureEntry = {
    SignerID,
    KeyID,
    RoleID?,
    SignatureSuiteID,
    SignatureBytes,
    RegistrationRef,
    EligibilityProofRef,
    AuxiliaryEvidence?
}
```

Fields in a signature entry are claims until verified against authenticated registry/checkpoint state.

## 5. Evidence bundle

```text
MultisigEvidence = {
    Statement,
    PolicyArtifact,
    SignerRosterArtifact,
    SignatureEntries[],
    RegistryProofs[],
    EligibilityProofs[],
    TransitionProofs[],
    EvaluationRecord?
}
```

`EvaluationRecord` is a cache only. Verifiers MUST recompute authorization from canonical evidence.

## 6. Policy semantics

The policy language MUST be finite and deterministic.

It SHOULD support, as needed:

- minimum distinct signer count
- minimum/maximum by role or trust class
- required signer IDs
- required role classes
- excluded signer combinations
- required signature profiles
- validity rules
- transition-specific rules
- accountable-participant requirements

This repository deliberately does **not** define a universal 4-of-8, 6-of-8, or other fixed quorum.

## 7. Deterministic verification

A verifier MUST:

1. validate and canonicalize the statement;
2. reconstruct `ToBeSigned`;
3. verify the policy artifact and version;
4. verify the signer roster and root;
5. verify authenticated registry/checkpoint evidence;
6. reject duplicate signer counting;
7. verify all required signature components;
8. discard invalid or ineligible entries;
9. construct the distinct eligible signer set;
10. evaluate all policy constraints;
11. verify stream sequence and predecessor rules;
12. verify transition constraints where applicable;
13. return the deterministic authorization result.

## 8. Threshold and aggregation boundary

Threshold signing, DKG, and signature aggregation are not part of the initial trusted core.

A future optimization may be added only if it preserves the same policy semantics and replay guarantees as the canonical evidence model.
