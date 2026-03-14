# Registry–Gatekeeper Separation Invariants

## Purpose

RGIS defines the runtime protocol between **registry authorities** and **gatekeeper enforcement nodes** operating under the GRC-P governance architecture.

To preserve deterministic governance enforcement, the protocol assumes strict separation between:

- governance authority
- runtime enforcement

These invariants define the minimum behavioral guarantees required of RGIS-compliant systems.

---

## Registry Responsibilities

A Registry participating in RGIS MUST:

- publish authoritative governance state
- maintain governance checks and waivers
- compute governance readiness
- maintain the canonical governance evidence ledger
- respond deterministically to RGIS protocol requests

The Registry MUST NOT execute operational actions on behalf of governed systems.

---

## Gatekeeper Responsibilities

A Gatekeeper participating in RGIS MUST:

- intercept operational requests
- validate request structure
- verify actor identity
- validate nonce or replay protection
- consult Registry authority via RGIS
- enforce PERMIT or DENY outcomes
- commit enforcement evidence to the Registry

---

## Protocol Separation Guarantees

### Rule Definition

Gatekeepers **MUST NOT define governance rules locally.**

All governance rules originate from Registry authority state.

---

### Decision Authority

Gatekeepers **MUST base enforcement decisions exclusively on Registry authority state** and protocol-valid request context.

Local policy substitutions **MUST NOT replace Registry authority decisions.**

---

### Evidence Ownership

Canonical governance evidence **MUST be recorded by the Registry.**

Gatekeepers MAY store local telemetry or temporary execution records but **MUST NOT treat these as canonical governance evidence.**

---

### Enforcement Behavior

If Registry authority cannot be consulted or validated, Gatekeepers **MUST fail closed** and deny execution.

---

### Protocol Integrity

Gatekeepers **MUST verify that Registry responses conform to RGIS protocol semantics before enforcing a decision.**

Invalid or unverifiable responses **MUST result in DENY outcomes.**

---

## Security Rationale

These protocol constraints ensure:

- enforcement cannot redefine governance
- governance cannot silently enforce actions
- execution cannot bypass enforcement
- audit evidence remains authoritative and verifiable

This preserves the architectural separation required by GRC-P.

---

## Summary

RGIS assumes the following invariant relationship:
