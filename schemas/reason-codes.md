# RGIS Reason Codes

This document defines the standardized reason codes returned by gatekeeper implementations when evaluating requests under the RGIS protocol.

Reason codes provide deterministic explanations for PERMIT or DENY outcomes and ensure interoperability across independent implementations.

All RGIS-compliant gatekeepers SHOULD return one of the following codes when issuing a decision response.

---

# Decision Outcomes

Gatekeepers produce one of two outcomes:

PERMIT  
DENY

Reason codes explain **why** the decision occurred.

---

# Standard Reason Codes

## Protocol Validation Errors

### PROTOCOL_INVALID

The request does not conform to the RGIS canonical request structure.

---

### SIGNATURE_INVALID

The request signature cannot be verified using the provided identity credentials.

---

### TIMESTAMP_INVALID

The request timestamp is outside the acceptable clock skew window.

---

### NONCE_REPLAY

The nonce has been previously used and the request may represent a replay attempt.

---

# Identity Failures

### IDENTITY_UNKNOWN

The actor identity cannot be resolved.

---

### IDENTITY_UNTRUSTED

The actor identity exists but is not trusted by the enforcement environment.

---

# Authorization Failures

### AUTHORIZATION_FAILED

The actor does not possess authorization to perform the requested operation.

---

### DELEGATION_INVALID

The delegation chain authorizing the actor is invalid or incomplete.

---

# Governance State Failures

### GOVERNANCE_UNSATISFIED

Required governance checks are not satisfied.

---

### WAIVER_EXPIRED

A required waiver exists but has expired.

---

### SUBJECT_REVOKED

The subject has been revoked by registry authority.

---

### READINESS_FALSE

The governance readiness evaluation returned false.

---

# Registry Failures

### REGISTRY_UNAVAILABLE

The registry authority cannot be reached.

---

### REGISTRY_ERROR

The registry authority returned an internal error.

---

# Security Failures

### SECURITY_POLICY_BLOCK

The request violates enforcement security policy.

---

# Permit Outcome Code

### GOVERNANCE_SATISFIED

All governance checks, authorizations, and validations are satisfied.

The gatekeeper may permit execution.

---

# Fail-Closed Requirement

RGIS implementations MUST default to **DENY** when:

- registry authority cannot be reached
- governance state cannot be verified
- protocol validation fails
- identity verification fails

This ensures deterministic fail-closed behavior.
