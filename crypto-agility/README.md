# Cryptographic agility lifecycle

Recovered and audited specification seed, integrated into senad-multisig at the user's direction. This component covers KEM migration, envelope protection, signature/hash succession, rewrapping, conditional crypto-shredding, suite epochs, and long-term evidence preservation.

Authorization remains defined by [the root specification](../docs/SPEC.md) and [its logical schema](../spec/multisig-statement.cddl). There is no second authorization schema or policy evaluator here. Threshold cryptography remains optional; no fixed 4-of-8 governance policy is adopted.

- [Lifecycle invariants](docs/ADDENDUM_cryptography.md)
- [Integration boundary](docs/INTEGRATION.md)
- [Threat model](docs/THREAT_MODEL.md)
- [Schedule framework](docs/T2_SCHEDULE.md)
- [Implementation gates](IMPLEMENTATION_PLAN.md)
- [Vector obligations](vectors/README.md)
- [Import provenance](docs/IMPORT_PROVENANCE.md)

Status: specification baseline only. No production implementation, executed cryptographic vectors, or byte-level interoperability is claimed.
