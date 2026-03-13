# Governance of the RGIS Protocol

## Purpose

This repository contains the specification and supporting materials for **RGIS v1.0** — the **Registry–Gatekeeper Interface System**.

RGIS defines the runtime interoperability protocol between:

- registry authorities
- gatekeeper enforcement nodes

The protocol standardizes:

- canonical request structure
- identity and replay requirements
- governance state resolution
- deterministic permit/deny decisions
- evidence generation
- fail-closed behavior

This repository is the canonical public home of the **RGIS protocol specification**.

---

## Stewardship

The RGIS protocol is stewarded by **Ventiser**.

Ventiser is responsible for:

- maintaining the specification
- publishing versioned updates
- reviewing protocol changes
- preserving deterministic semantics
- maintaining interoperability requirements

Stewardship does **not** mean that Ventiser is the only party permitted to build a registry or gatekeeper implementation.

RGIS is intended to support interoperable implementations under the GRC-P architecture.

---

## Scope of this Repository

This repository defines the **runtime protocol** between registry authority and gatekeeper enforcement.

It includes:

- protocol specification
- request and response schemas
- reason codes
- wire format rules
- protocol state machine
- interoperability requirements
- security considerations

This repository does **not** define a specific registry implementation or gatekeeper product.

Examples of implementations include:

- registry authority systems such as GovenAI
- gatekeeper systems such as JobQue
- independent compatible implementations built by third parties

---

## Relationship to GRC-P

RGIS is the runtime protocol operating under the **GRC-P standard**.

In simple terms:

- **GRC-P** defines the governance architecture
- **RGIS** defines the runtime protocol

The GRC-P standard is maintained in a separate repository.

---

## Change Management

Changes to RGIS must preserve deterministic interoperability.

Substantive changes may include:

- clarifications to protocol behavior
- extensions to schema definitions
- additional reason codes
- security clarifications
- wire format refinements
- new protocol versions

Protocol changes SHOULD be reviewed carefully before publication.

---

## Versioning

RGIS uses explicit versioning.

Examples:

- `v1.0`
- `v1.1`
- `v2.0`

### Versioning principles

- **Patch-level changes** clarify wording without changing protocol meaning
- **Minor versions** add non-breaking protocol capabilities
- **Major versions** introduce breaking changes to protocol semantics or interoperability

Implementations SHOULD declare the RGIS version they support.

---

## Compatibility

An implementation may describe itself as compatible with RGIS only when it preserves the core protocol requirements.

These requirements include:

- canonical request normalization
- deterministic decision semantics
- standardized reason codes
- fail-closed behavior
- verifiable evidence generation
- protocol-valid signing and replay protection

Compatibility claims must not be made where these requirements are not met.

---

## Conformance and Certification

Formal conformance testing and compatibility certification may be published separately by Ventiser.

Future certification may include:

- request canonicalization testing
- protocol response validation
- reason code compatibility
- wire format interoperability
- fail-closed enforcement validation
- evidence consistency testing

Publication of this repository does not by itself grant certification or trademark rights.

---

## Contributions

At this stage, stewardship remains centrally managed.

Feedback, issues, and protocol observations may be reviewed and incorporated at the discretion of the maintainer.

Independent interoperable implementations are encouraged, provided they preserve the protocol semantics defined in this repository.

---

## Repository Integrity

The purpose of this repository is to preserve RGIS as a deterministic, interoperable runtime protocol.

Changes should strengthen:

- protocol clarity
- interoperability
- determinism
- evidence consistency
- security posture
- implementation readiness
