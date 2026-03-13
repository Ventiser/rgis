# RGIS Wire Format

This document defines the transport and message encoding rules for the Registry–Gatekeeper Interface System (RGIS).

The wire format standardizes how canonical requests and deterministic decision responses are serialized, transmitted, signed, and verified between registry authorities and gatekeeper implementations.

---

# 1. Transport Model

RGIS messages are exchanged between:

- **Gatekeeper**
- **Registry Authority**

The initial transport binding for RGIS v1.0 is:

- **HTTPS**
- **JSON**
- **UTF-8**

All RGIS protocol messages MUST be transmitted over TLS-protected channels.

---

# 2. Content Type

All RGIS request and response bodies MUST use:

```text
application/json
Character encoding MUST be UTF-8.

3. Request Method

The default registry query method is:

POST

Example endpoint:

POST /rgis/v1/decision

A gatekeeper sends a canonical request payload to the registry authority and receives a deterministic decision response.

4. Canonical Request Envelope

A registry query request MUST contain the following top-level fields.

JSON Structure
{
  "protocol": "RGIS",
  "version": "1.0",
  "request_id": "6d3d7f84-41a2-4ec5-bd1d-8d9f5d7a7f22",
  "actor": {
    "actor_id": "agent:billing-bot-01",
    "actor_type": "agent"
  },
  "subject": {
    "subject_id": "system:payments-api",
    "subject_type": "service"
  },
  "operation": "payment.create",
  "context": {
    "tenant_id": "tenant:zenpay-demo",
    "environment": "production"
  },
  "nonce": "2f8df7a3db2f4e13a13e3d80d1c972e7",
  "timestamp": "2026-03-14T16:00:00Z",
  "signature": {
    "algorithm": "ed25519",
    "key_id": "agent:billing-bot-01#key-1",
    "value": "BASE64_SIGNATURE"
  }
}
5. Request Fields
protocol

MUST equal:

RGIS
version

MUST equal the supported protocol version.

Example:

1.0
request_id

A globally unique identifier for the request.

Recommended format:

UUID

actor

Object identifying the requesting entity.

Required fields

actor_id

actor_type

Example values:

human

agent

service

partner_system

subject

Object identifying the governed target.

Required fields

subject_id

subject_type

Example values:

service

workflow

api

dataset

process

operation

A string representing the requested action.

Examples:

payment.create

ticket.issue

profile.read

workflow.execute

context

Optional object containing contextual metadata relevant to governance evaluation.

Examples:

tenant

environment

region

customer type

transaction class

Context keys MUST be deterministic and consistently serialized.

nonce

A unique replay-protection token.

The nonce MUST be:

unique within the replay window

non-empty

treated as opaque by the registry

timestamp

UTC timestamp in RFC 3339 format.

Example:

2026-03-14T16:00:00Z
signature

Cryptographic signature metadata and value.

Required fields

algorithm

key_id

value

Supported algorithms for RGIS v1.0 SHOULD include:

ed25519

rsa-pss-sha256

6. Canonical Serialization Rules

To preserve deterministic evaluation, RGIS request signing MUST be based on canonical JSON serialization.

Implementations MUST ensure:

UTF-8 encoding

lexicographic key ordering

no insignificant whitespace

stable object field ordering

deterministic number and string representation

The signing payload MUST be the canonical serialized request body excluding the signature.value field.

7. Decision Response Envelope

The registry authority MUST return a deterministic response.

JSON Structure
{
  "protocol": "RGIS",
  "version": "1.0",
  "request_id": "6d3d7f84-41a2-4ec5-bd1d-8d9f5d7a7f22",
  "decision_id": "fcb2df04-40d8-4d33-885f-ccf1a7d1bc90",
  "decision": "DENY",
  "reason_code": "READINESS_FALSE",
  "registry_state_hash": "HEX_SHA256_HASH",
  "timestamp": "2026-03-14T16:00:01Z",
  "signature": {
    "algorithm": "ed25519",
    "key_id": "registry:govenai#key-1",
    "value": "BASE64_SIGNATURE"
  }
}
8. Response Fields
decision

MUST be one of:

PERMIT
DENY
reason_code

MUST be one of the standardized RGIS reason codes.

Examples:

GOVERNANCE_SATISFIED

AUTHORIZATION_FAILED

READINESS_FALSE

REGISTRY_UNAVAILABLE

registry_state_hash

A hash representing the governance state used to evaluate the request.

This field is critical for replayability and audit verification.

Recommended format:

lowercase hex SHA-256 digest

timestamp

UTC RFC 3339 timestamp indicating when the decision was created.

signature

Cryptographic signature applied by the registry authority over the canonical response.

9. HTTP Status Codes

HTTP status codes describe transport-level outcomes, not governance outcomes.

Recommended mapping
HTTP Status	Meaning
200	valid protocol response returned
400	malformed RGIS request
401	identity/authentication failure
409	nonce replay detected
422	request syntactically valid but protocol-invalid
500	registry internal error
503	registry unavailable

Governance outcomes MUST still be expressed using the decision and reason_code fields.

A denied governance decision does NOT require a non-200 HTTP status.

10. Signing Rules

Both requests and responses SHOULD be signed.

Request Signing

The gatekeeper or requesting actor signs the canonical request.

Response Signing

The registry authority signs the canonical decision response.

Signatures MUST be computed over canonical JSON serialization.

The signature.value field MUST NOT be included in the bytes being signed.

11. Hashing Rules

The default hashing algorithm for RGIS v1.0 SHOULD be:

SHA-256

The following values SHOULD be hashed where applicable:

canonical request

canonical response

registry state snapshot

evidence event payload

12. Evidence Binding

The decision response SHOULD be bindable to an evidence record.

Recommended evidence fields:

request_id

decision_id

decision

reason_code

registry_state_hash

timestamp

event_hash

prev_event_hash

Where evidence is persisted externally, these values MUST remain consistent with the wire response.

13. Fail-Closed Wire Behavior

If a gatekeeper cannot obtain a valid, signed, and parseable RGIS decision response, it MUST treat the request as denied.

This includes cases where:

the response is malformed

the response signature is invalid

the protocol version is unsupported

the registry authority is unreachable

the registry state hash is missing when required

14. Minimal Registry Decision Endpoint

The minimal RGIS decision API for v1.0 is:

POST /rgis/v1/decision

Input:

canonical request envelope

Output:

deterministic decision response envelope

This endpoint is sufficient for interoperable registry–gatekeeper implementations.

15. Example Permit Response
{
  "protocol": "RGIS",
  "version": "1.0",
  "request_id": "6d3d7f84-41a2-4ec5-bd1d-8d9f5d7a7f22",
  "decision_id": "1fce3a31-f7b0-48fe-8ba7-470f2da4c4f0",
  "decision": "PERMIT",
  "reason_code": "GOVERNANCE_SATISFIED",
  "registry_state_hash": "7e4f54c4db55b4c9f8d8e4f6d8d3d63b4d59e3e1fcb9ad1d7ab1ccf4f0d8a9e2",
  "timestamp": "2026-03-14T16:00:01Z",
  "signature": {
    "algorithm": "ed25519",
    "key_id": "registry:govenai#key-1",
    "value": "BASE64_SIGNATURE"
  }
}
16. Example Deny Response
{
  "protocol": "RGIS",
  "version": "1.0",
  "request_id": "6d3d7f84-41a2-4ec5-bd1d-8d9f5d7a7f22",
  "decision_id": "fcb2df04-40d8-4d33-885f-ccf1a7d1bc90",
  "decision": "DENY",
  "reason_code": "READINESS_FALSE",
  "registry_state_hash": "7e4f54c4db55b4c9f8d8e4f6d8d3d63b4d59e3e1fcb9ad1d7ab1ccf4f0d8a9e2",
  "timestamp": "2026-03-14T16:00:01Z",
  "signature": {
    "algorithm": "ed25519",
    "key_id": "registry:govenai#key-1",
    "value": "BASE64_SIGNATURE"
  }
}
17. Interoperability Note

Independent implementations are conformant only if they:

parse the same request structure

canonicalize and sign the same payload bytes

produce the same decision semantics

return standardized response fields

enforce fail-closed behavior when protocol validity fails
