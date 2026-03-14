# RGIS v1.0
Registry–Gatekeeper Interface System

Status: Draft  
Parent Standard: GRC-P  
Maintainer: Ventiser  
License: CC BY 4.0

## Abstract

RGIS defines the runtime protocol between registry authorities and gatekeeper enforcement nodes.

The protocol determines whether an operational action is admissible under authoritative governance state before execution occurs.

RGIS standardizes:

- canonical request structure
- identity verification requirements
- governance state resolution
- deterministic decision semantics
- reason codes
- fail-closed behavior
- evidence generation

## Nonce and Replay Protection

RGIS requires replay protection for all protocol-valid requests.

Every canonical request MUST include a nonce.

### Nonce Requirements

A nonce MUST:

- be present in every request
- be unique within the applicable replay window
- be treated as opaque by the Registry
- be bound to the submitted request context
- be evaluated together with the request timestamp

### Replay Detection

A Registry MUST reject a request when:

- the nonce has already been observed within the replay window
- the nonce is malformed or missing
- the timestamp is outside the accepted validity window
- the request context does not match the original nonce-bound request context when such binding is enforced

### Fail-Closed Requirement

If replay protection cannot be validated, the request MUST be denied.

### Recommended Outcome

When replay is detected, the Registry or Gatekeeper SHOULD return:

- `DENY`
- `NONCE_REPLAY`
