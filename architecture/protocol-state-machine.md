# RGIS Protocol State Machine

This document defines the deterministic execution flow for the Registry–Gatekeeper Interface System (RGIS).

RGIS ensures that operational requests are evaluated against authoritative governance state before execution occurs.

---

## Protocol Flow

The RGIS runtime flow consists of six stages.


Client Request
↓
Gatekeeper Interception
↓
Request Validation
↓
Registry Governance Query
↓
Decision Generation
↓
Execution or Denial
↓
Evidence Recording


---

## State Definitions

### 1. Request Received

The gatekeeper receives a request from a client or upstream system.

The request must be normalized into a canonical request structure.


State: REQUEST_RECEIVED


---

### 2. Canonicalization

The request is converted into the canonical RGIS request structure.


Q = (actor, subject, operation, context, nonce, timestamp, signature)


State:


REQUEST_CANONICALIZED


---

### 3. Identity Verification

The gatekeeper verifies the actor identity and request signature.

Failure results in immediate denial.


State: IDENTITY_VERIFIED


---

### 4. Replay Protection

Nonce validation ensures the request cannot be replayed.


State: NONCE_VALIDATED


---

### 5. Governance Evaluation

The gatekeeper queries the registry authority to determine governance readiness.

The registry evaluates:

- compliance checks
- waivers
- revocations
- authorization


State: GOVERNANCE_EVALUATED


---

### 6. Decision Generation

The gatekeeper generates a deterministic decision.

Possible outcomes:


PERMIT
DENY


Decision rule:


Permit(Q) ⇔ Valid(Q) ∧ Auth(Q) ∧ Ready(Q)


---

### 7. Execution Enforcement

If the decision is **PERMIT**, execution may proceed.

If the decision is **DENY**, execution is blocked.


State: DECISION_ENFORCED


---

### 8. Evidence Recording

All decisions must generate a tamper-evident evidence event.

The event is chained with the previous event hash.


event_hash = hash(event_data + previous_event_hash)

State: EVIDENCE_RECORDED


---

## Failure Semantics

RGIS requires fail-closed enforcement.

Execution must be denied when:

- identity verification fails
- nonce validation fails
- registry authority is unreachable
- governance state cannot be resolved

---

## Deterministic Requirement

Given identical:

- canonical request
- governance state
- authorization state

all compliant implementations must produce identical decisions.

This property enables distributed gatekeeper interoperability.
