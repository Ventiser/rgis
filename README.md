# RGIS
Registry–Gatekeeper Interface System

![RGIS Version](https://img.shields.io/badge/RGIS-v1.0--draft-blue)

RGIS defines the runtime interoperability protocol between **registry authorities** and **gatekeeper enforcement nodes** operating under the **GRC-P governance architecture**.

The protocol standardizes how operational requests are evaluated against authoritative governance state before execution occurs.

RGIS defines:

- canonical request structure
- identity and replay validation
- registry governance queries
- deterministic **PERMIT / DENY** decisions
- standardized reason codes
- fail-closed enforcement
- tamper-evident governance evidence

---

# Role in the Stack


GRC-P → Governance architecture
RGIS → Runtime interoperability protocol
GovenAI Registry → Registry authority implementation
JobQue Gatekeeper → Gatekeeper enforcement implementation


- **GRC-P** defines the governance control architecture.
- **RGIS** defines the runtime protocol between registry and gatekeeper.
- **GovenAI Registry** is a registry authority implementation.
- **JobQue Gatekeeper** is a runtime gatekeeper implementation.

---

# Relationship to GRC-P

RGIS operates under the **Governance Runtime Control Protocol (GRC-P)** standard.

GRC-P repository:

https://github.com/Ventiser/grc-p

In simple terms:

- **GRC-P** defines the governance architecture.
- **RGIS** defines the runtime protocol used to enforce that architecture.

---

# Protocol Overview

The RGIS runtime flow:


Client Request
↓
Gatekeeper Intercepts Request
↓
Canonical Request Validation
↓
Registry Governance Query
↓
Permit / Deny Decision
↓
Execution Allowed or Blocked
↓
Evidence Event Recorded


Gatekeepers MUST enforce **fail-closed semantics**.

If governance state cannot be verified, the request **MUST be denied**.

---

# Threat Model Summary

RGIS enforces governance decisions before operational execution occurs.

The protocol protects against several classes of attack by separating **authority**, **enforcement**, and **execution**.


Client / Actor
│
│ Signed Request
│ + Nonce
│ + Timestamp
▼
Gatekeeper
(Runtime Enforcement)
│
│ Replay Protection
│ Identity Verification
│
▼
Registry Authority
(Governance State)
│
│ Rule Evaluation
│ Governance Readiness
│
▼
Decision Response
(PERMIT / DENY)
│
▼
Execution System
│
▼
Evidence Ledger
(Hash-chained governance record)


### Threats Addressed

| Threat | Mitigation |
|------|------|
Replay attacks | Nonce + timestamp validation |
Identity spoofing | Cryptographic request verification |
Governance manipulation | Registry authority separation |
Execution bypass | Mandatory gatekeeper enforcement |
Evidence tampering | Tamper-evident governance logs |

If any security validation fails, the protocol **fails closed and denies execution**.

---

# Protocol Status

| Version | Status |
|--------|--------|
| v1.0 | Draft |

---

# Repository Structure


specs/ RGIS protocol specification
schemas/ request, response, and reason code schemas
architecture/ protocol state machine and runtime flow
wire-format/ transport and message encoding rules
security/ protocol security considerations
conformance/ interoperability requirements
diagrams/ protocol flow diagrams


---

# Status

Draft publication of **RGIS v1.0**.

---

# Related Repositories

GRC-P — Governance architecture  
https://github.com/Ventiser/grc-p

RGIS — Registry–Gatekeeper protocol  
https://github.com/Ventiser/rgis

GovenAI Registry — reference registry authority implementation  
https://github.com/Ventiser/govenai-registry

JobQue Gatekeeper — reference runtime enforcement implementation  
https://github.com/Ventiser/jobque-gatekeeper

---

# Stewardship

Maintained by **Ventiser**.
