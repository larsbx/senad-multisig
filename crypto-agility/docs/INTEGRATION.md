# Integration with senad-multisig

This crypto-agility component owns the cryptographic lifecycle: KEM and envelope migration,
signature and hash evidence renewal, suite succession, and conditional erasure.
The root specification owns authorization semantics, signer eligibility,
policy evaluation, and roster succession. The components remain distinct within one repository.

Baseline inspected: senad-multisig main commit
`6ac939cad0f27c295a984097c12fd0a81a533206`, docs/SPEC.md and
spec/multisig-statement.cddl. The root schema remains a logical sketch, not a frozen interoperable byte format. The seed integer-label sketch is deliberately not imported as a second schema.

The shared logical statement commits to version, domain, purpose, action,
stream ID, object type and typed object root, policy ID and version, typed
roster root, crypto suite epoch, governance epoch, authorization epoch, typed
registry checkpoint, sequence, typed predecessor (null at genesis), optional
transition data, and context. All counted signers sign precisely
ASCII("SENAD+-MULTISIG-V1") concatenated with canonical CBOR of this statement.
Use the root schema field names object_root and signer_roster_root; the seed aliases object_digest and signer_roster_digest do not define additional fields.

Suite epochs MUST resolve to authenticated immutable suite manifests specifying
algorithms, parameter sets, encoding profile, key use, and required hybrid
components. They are distinct from governance and authorization epochs.
Unknown suites or absent manifests MUST fail verification, never select defaults.

Registry checkpoints MUST authenticate registry state and its ordering evidence.
The statement's authorization epoch MUST be consistent with that checkpoint.
Verifier inputs include authenticated registration, purpose, revocation, policy,
and roster evidence. Claimed wall-clock signing time grants no eligibility.
Routine later retirement does not erase valid historical authorization evidence;
compromise handling requires explicit policy and preserved ordering evidence.

Lifecycle transitions MUST be authorized using the committed policy and suite
at the applicable checkpoint. Incumbent authorization and successor acknowledgement
MUST bind the same transition object, including old/new suites, policies,
rosters, activation checkpoint, and predecessor as applicable. A successor-only
assertion does not authorize its own installation. Recovery needs an explicitly
authorized recovery policy, not an implicit bypass.

Distinct SignerID is the counting unit. Multiple algorithms never create extra
signers. No fixed 4-of-8 or other universal quorum is established. Threshold
cryptography is optional; participant disclosure is policy-controlled.

Before production interoperability, freeze field labels and types,
transition schemas, suite and registry manifests, identifier hashing, timestamp
profile, and exact positive/adversarial signing-byte vectors. Until then this
is a specification baseline, not a production implementation or conformance claim.
