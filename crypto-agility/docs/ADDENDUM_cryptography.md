# ADDENDUM — Cryptographic Agility and Succession

## 1. Organizing asymmetry

Senad+ treats confidentiality migration as a pre-data obligation and authenticity migration as a managed renewal obligation.

A captured ciphertext may survive every later policy change. A historical signature, by contrast, can be re-vouched while its incumbent algorithm and validation evidence remain trustworthy. Therefore KEM/envelope readiness is urgent before sensitive production data exists, while signature and authorization evidence use explicit succession paths.

This principle does **not** imply that all historical cryptographic bindings can be renewed merely by signing their old roots. Hash succession has a stronger requirement described in INV-C4B.

## 2. Cryptographic invariants

### INV-C1 — No sensitive plaintext before envelope acceptance

Production-sensitive payload creation is forbidden until the active envelope suite has passed a plaintext-leakage acceptance scan and key-custody acceptance tests.

This gate runs before real sensitive data exists.

### INV-C2 — Cryptographic self-description

Every durable cryptographic object identifies the algorithms, parameter sets, encoding profile, suite epoch, and key identifiers required to interpret and verify it.

Algorithm identifiers are part of the authenticated object. A verifier must never infer an algorithm from byte length, current defaults, filename, or deployment date.

### INV-C3 — Explicit succession links

A successor cryptographic state names its predecessor and the exact transition object that authorizes the change.

Succession is append-only. It does not rewrite the historical object graph.

### INV-C4A — Signature evidence renewal

Before incumbent signature protection becomes unacceptable, a successor authorization may re-vouch existing canonical evidence without re-signing every historical payload.

The successor attestation binds at least:

- the historical evidence/root being renewed;
- the incumbent signature suite identifier;
- the successor signature suite identifier;
- the governing policy identifier and version;
- the authorization checkpoint/epoch; and
- the predecessor renewal identifier, if any.

Renewal after the predecessor has already become untrustworthy does not retroactively establish historical validity.

### INV-C4B — Hash evidence renewal

Hash succession is not satisfied by computing a successor hash over only an incumbent root when future integrity still depends on the collision resistance of the incumbent hash.

Before the incumbent content-binding hash loses acceptable security, the preserved evidence needed for later replay must be rebound under the successor hash. The renewal process must include the archived objects and evidence necessary to reconstruct the security claim, consistent with hash-tree-renewal semantics.

Historical identifiers may remain recorded, but the new evidence path must not depend solely on the old hash remaining collision resistant.

### INV-C5 — Stable blob address across key re-wrap

A stable content address identifies the immutable ciphertext blob, not its mutable key wrapper.

Conceptually:

```text
CiphertextBlob = AEAD_Encrypt(DEK, plaintext, authenticated_context)
BlobID         = Digest(ciphertext_blob)
KeyEnvelope    = Wrap(KEK, DEK, BlobID, envelope_context)
```

Re-wrapping the DEK changes `KeyEnvelope` but not `CiphertextBlob` or `BlobID`.

A symmetric-cipher or AEAD succession that requires creation of a new ciphertext produces a new blob and a supersession link rather than mutating the old address.

### INV-C6 — Complete-key-erasure condition for crypto-shredding

Crypto-shredding is claimed only after every authorized copy, export, backup, cache, derivation path, recovery wrapper, and recipient-access path capable of recovering the subject DEK has been irrecoverably destroyed or rendered computationally inaccessible.

Under that complete-erasure condition, retained ciphertext is computationally
inaccessible only under the stated encryption and key-management assumptions.
This does not erase previously obtained plaintext or adversarial key copies.
Deleting one wrapper is not sufficient evidence of erasure.

The system must maintain a key-copy/custody inventory sufficient to verify the erasure condition.

### INV-C7 — Multisignature semantics are policy semantics

A multisignature authorization is defined by a canonical statement, an authenticated signer roster, an authenticated policy, and independently verifiable evidence that the policy was satisfied.

The authorization meaning must not depend on a particular aggregation or threshold-signature implementation.

### INV-C8 — One authorization, one canonical byte string

Every counted signer authorizes exactly the same canonical `ToBeSigned` bytes.

Signatures over different policy versions, roster roots, object roots, suite epochs, authorization checkpoints, sequence numbers, stream identifiers, or predecessors are not combinable.

### INV-C9 — Quorum identity, not signature count

A signer contributing multiple algorithm components during a hybrid epoch counts once toward signer cardinality and the policy MUST NOT count these components as additional signers.

Hybrid components may all be required for that signer's evidence bundle, but they do not create multiple independent signers.

### INV-C10 — Eligibility is checkpointed, not self-timestamped

Signer eligibility is evaluated against authenticated ordering evidence such as an authorization epoch, registry checkpoint, ledger position, or equivalent committed state.

A self-asserted signing timestamp is not sufficient to prove that a key was registered, unrevoked, or authorized at that time.

### INV-C11 — Roster and policy succession cross-vouch exact transition bytes

An incumbent authorization and successor acknowledgment must bind the same canonical transition object.

The transition names the incumbent and successor policy/roster roots, activation checkpoint, predecessor transition, and required authorization rules.

The transition does not re-sign historical payloads.

### INV-C12 — Typed digest references

Every durable digest reference carries an explicit algorithm identifier and digest bytes:

```text
DigestRef = {
  algorithm_id,
  digest_bytes
}
```

No protocol field called `root`, `hash`, or `digest` may be interpreted without its algorithm identity being authenticated in the same object or in an unambiguous authenticated suite context.

### INV-C13 — Scoped sequencing

Sequence numbers are scoped to an authenticated stream or authority identifier.

```text
StreamID
Sequence
PreviousStatementID
```

This prevents normal concurrent histories in different streams from being misclassified as forks while still making rollback and equivocation detectable within a stream.

## 3. Suite roles

The initial policy separates signature roles:

- **identity-critical evidence** — genesis, trust-root registration, signer-registry authority changes, suite rollover, policy-root rollover, and equivalent long-lived authority changes may require an SLH-DSA profile;
- **operational evidence** — routine traffic may use an ML-DSA-based profile, including a classical companion component while a hybrid-confidence policy remains active;
- **KEM/envelope protection** — ML-KEM is the initial PQ KEM vocabulary for new envelope profiles.

These are policy defaults, not wire-format assumptions. Concrete parameter sets and hybrid combinations are identified by suite IDs.

## 4. Confidence epoch

No calendar date is embedded for retirement of classical hybrid components.

Retirement is a future policy amendment evaluated against explicit criteria such as:

- standards maturity and errata disposition;
- deployed implementation diversity;
- cryptanalytic evidence;
- interoperability results;
- operational recovery readiness; and
- estate-wide migration completeness.

The amendment must itself satisfy the then-current identity-critical authorization policy.

## 5. Canonical encoding profile

Normative cryptographic statements use deterministic CBOR based on RFC 8949 Core Deterministic Encoding Requirements with these additional restrictions:

1. no floating-point values;
2. no indefinite-length items;
3. duplicate map keys rejected;
4. text normalized to Unicode NFC before encoding;
5. schema fixes whether a field is text, bytes, integer, array, or map;
6. tags are either required by schema or forbidden; optional tag-equivalent representations are not allowed;
7. map-key representation is fixed by schema;
8. timestamps, where unavoidable, use one exact UTC representation and are not relied on as authorization-order evidence;
9. security decisions use committed epochs/checkpoints rather than parsed wall-clock claims.

For this lifecycle profile, timestamps MUST be untagged text of the exact form
`YYYY-MM-DDTHH:MM:SS.ffffffZ`: UTC, uppercase T/Z, exactly six fractional
second digits, valid Gregorian date and time, seconds 00 through 59. Offsets,
leap-second values, and alternate precision MUST be rejected. Timestamps are
metadata only; ordering and eligibility still require authenticated checkpoints.
Encoders normalize text to NFC; verifiers reject received non-NFC text rather
than silently rewriting signed input. The shared wire labels and schema-specific
context/transition forms remain pending P0 acceptance.

Golden vectors contain the exact encoded bytes, not merely diagnostic notation.

## 6. References

- NIST FIPS 203 — Module-Lattice-Based Key-Encapsulation Mechanism Standard (ML-KEM)
- NIST FIPS 204 — Module-Lattice-Based Digital Signature Standard (ML-DSA)
- NIST FIPS 205 — Stateless Hash-Based Digital Signature Standard (SLH-DSA)
- RFC 8949 — Concise Binary Object Representation (CBOR)
- RFC 4998 — Evidence Record Syntax (ERS), especially ordinary renewal versus hash-tree renewal
