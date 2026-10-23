# Golden Vector Obligations

The first vector corpus must include both positive and adversarial cases.

## Required vectors

1. valid generic multisignature authorization
2. duplicate signer counted twice — reject
3. same signer using multiple algorithms counted twice — reject
4. signature over a different object root — reject
5. signature over a different policy version — reject
6. correct signatures evaluated against a substituted roster — reject
7. signer invalid at the authenticated checkpoint — reject
8. later routine key retirement does not invalidate an earlier valid authorization
9. signature profile incompatible with the action purpose — reject
10. valid policy/roster rollover
11. successor-only rollover — reject
12. incumbent and successor authorize different transition objects — reject
13. malformed or noncanonical CBOR — reject
14. Unicode normalization mismatch — reject
15. wrong stream predecessor or rollback — reject
16. policy exclusion rule violated — reject
17. accountable-authorization policy missing required participant evidence — reject

## Vector contents

Each positive vector should contain:

- human-readable diagnostic form
- exact canonical CBOR bytes
- `ToBeSigned`
- typed statement identifier
- registry/checkpoint artifacts
- public keys
- signature entries
- policy evaluation trace
- expected final result

Each negative vector should identify the mandatory rejection condition without making consensus depend on diagnostic ordering.
