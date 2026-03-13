# RGIS
**Registry–Gatekeeper Interface System**

RGIS defines the runtime interoperability protocol between **registry authorities** and **gatekeeper enforcement nodes** under the **GRC-P** governance architecture.

It standardizes how a gatekeeper:

- receives a canonical request
- verifies identity and replay protection
- resolves governance state from a registry authority
- produces a deterministic **PERMIT** or **DENY** decision
- records tamper-evident evidence

## Role in the Stack

```text
GRC-P   → Governance architecture
RGIS    → Runtime interoperability protocol
GovenAI → Registry authority implementation
JobQue  → Gatekeeper implementation
