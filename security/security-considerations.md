# RGIS Security Considerations

RGIS implementations must address the following threats.

## Replay Protection

RGIS assumes that operational requests may be replayed by malicious or faulty actors unless explicitly protected.

To mitigate replay attacks:

- every request MUST include a nonce
- every request MUST include a timestamp
- registries or associated replay-validation mechanisms MUST track nonce reuse within the accepted replay window
- replay validation failures MUST result in DENY outcomes

A replayed request MUST NOT be evaluated as a fresh governance request.

The recommended reason code for replay failure is:

- `NONCE_REPLAY`

## Identity Spoofing

Prevented through cryptographic signature verification.

## Governance Manipulation

Prevented through registry authority separation.

## Evidence Tampering

Prevented through hash-chained event logs.

## Execution Bypass

Prevented by enforcing gatekeeper interception of governed actions.
