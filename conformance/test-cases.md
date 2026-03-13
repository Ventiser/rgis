# RGIS Conformance Test Cases

This document defines baseline tests for verifying RGIS protocol compatibility.

## Request Validation

Test that invalid canonical requests are rejected.

## Signature Verification

Test that invalid signatures produce a DENY decision.

## Nonce Replay

Test that reused nonces are rejected.

## Governance Evaluation

Test that requests fail when governance readiness is false.

## Fail-Closed Behavior

Test that gatekeepers deny requests when the registry is unreachable.

## Evidence Recording

Test that each decision produces an evidence event.

Implementations passing these tests may be considered RGIS-compatible.
