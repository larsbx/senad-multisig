# Golden Vector Requirements

The first implementation artifact is a deterministic vector corpus.

Each positive vector must include:

- human-readable diagnostic form;
- exact deterministic CBOR bytes;
- exact `ToBeSigned` bytes;
- typed object and roster digests;
- signer-registration/checkpoint evidence;
- signature evidence;
- policy input;
- deterministic evaluation trace; and
- expected authorization result.

Required adversarial vectors include:

1. valid minimal policy-satisfying authorization;
2. duplicate signer presented under two keys — must not double count;
3. one signer supplies multiple hybrid algorithm components — counts once;
4. signature over a different object digest — reject;
5. signature over a different policy version — reject;
6. signature over a substituted roster digest — reject;
7. ineligible/revoked signer at the authenticated checkpoint — reject;
8. signer retired after a valid checkpoint — historical authorization remains valid under policy;
9. claimed wall-clock time conflicts with checkpoint state — checkpoint governs;
10. non-deterministic/forbidden CBOR representation — reject;
11. Unicode text not in required normalization form — reject;
12. sequence rollback within one `StreamID` — reject;
13. same sequence in a different authorized stream — independently evaluated, not automatically a fork;
14. successor-only roster rollover without incumbent authorization — reject;
15. incumbent and successor sign different transition bytes — reject;
16. valid signature-suite renewal over historical evidence;
17. invalid “hash renewal” that protects only an old root while relying on a deprecated hash — reject as insufficient renewal evidence;
18. valid hash-evidence renewal rebinding preserved evidence under the successor hash;
19. key re-wrap changes `KeyEnvelope` but leaves `CiphertextBlob` and `BlobID` unchanged;
20. crypto-shred test with one surviving recovery wrapper — erasure claim fails.

Negative-vector diagnostics should identify stable reason codes, but consensus validity must not depend on which error a verifier reports first.
