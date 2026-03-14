# Canonical Request Schema

RGIS requires that all operational requests be normalized into a canonical structure before evaluation.

Canonical request:

Q = (actor, subject, operation, context, nonce, timestamp, signature)

## Fields

actor
: identity initiating the request

subject
: governed system or resource

operation
: requested action

context
: optional contextual metadata

## nonce

A nonce is a unique replay-protection token included in every canonical RGIS request.

The nonce:

- MUST be present
- MUST be unique within the replay validation window
- MUST be treated as opaque by the Registry
- MUST be evaluated together with the request timestamp
- SHOULD be cryptographically strong or otherwise collision-resistant

Requests missing a nonce are protocol-invalid and MUST be denied.

## timestamp

The timestamp records when the canonical request was created.

The timestamp:

- MUST be present
- MUST use UTC
- MUST be evaluated together with the nonce
- MUST fall within the accepted clock-skew window

Requests with expired or invalid timestamps MUST be denied.

signature
: cryptographic proof of authenticity
