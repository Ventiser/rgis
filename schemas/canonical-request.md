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

nonce
: unique token preventing replay attacks

timestamp
: request creation time

signature
: cryptographic proof of authenticity
