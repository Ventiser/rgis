# RGIS Security Considerations

RGIS is designed to enforce deterministic governance decisions before operational execution occurs.  
Implementations MUST account for the following classes of security risk.

The protocol assumes a **fail-closed posture**: when security validation cannot be completed, the request MUST be denied.

---

## Replay Protection

RGIS assumes that operational requests may be replayed by malicious or faulty actors unless explicitly protected.

To mitigate replay attacks:

- every canonical request MUST include a `nonce`
- every canonical request MUST include a `timestamp`
- registries or associated replay-validation mechanisms MUST track nonce reuse within the accepted replay window
- registries MUST reject requests containing reused or expired nonces
- replay validation failures MUST result in `DENY` outcomes

A replayed request MUST NOT be evaluated as a fresh governance request.

The recommended reason code for replay failure is:


NONCE_REPLAY


---

## Identity Spoofing

Gatekeepers and registries MUST verify the identity of request actors before governance evaluation.

Identity verification SHOULD be implemented using cryptographic mechanisms such as:

- public-key signatures
- certificate validation
- signed tokens or equivalent mechanisms

Requests with unverifiable or invalid identity credentials MUST be denied.

---

## Governance Manipulation

Governance rules MUST originate exclusively from a registry authority.

Gatekeepers MUST NOT:

- define governance rules
- override registry governance state
- substitute local policy for registry authority decisions

This separation prevents enforcement systems from silently redefining governance logic.

---

## Evidence Tampering

Governance evidence MUST be tamper-evident.

Registries SHOULD record governance events using append-only evidence logs that provide:

- event ordering
- integrity verification
- traceability of governance decisions

Recommended mechanisms include cryptographic hash chaining or equivalent tamper-evident logging systems.

---

## Execution Bypass

Execution systems MUST NOT bypass the gatekeeper enforcement boundary.

All governed operational actions MUST pass through a gatekeeper capable of performing RGIS validation.

If a governed action can be executed without gatekeeper validation, governance enforcement may be circumvented.

---

## Fail-Closed Behavior

RGIS implementations MUST deny execution when security validation cannot be completed.

Examples include:

- registry authority unreachable
- invalid request structure
- nonce or replay validation failure
- identity verification failure
- protocol response validation failure

In these situations the gatekeeper MUST return a deterministic `DENY` outcome.
