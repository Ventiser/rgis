# RGIS Protocol Flow

This diagram illustrates the runtime flow of the Registry–Gatekeeper Interface System (RGIS).

The protocol ensures that operational actions are evaluated against governance authority before execution occurs.

```mermaid
flowchart TD

A[Client Request] --> B[Gatekeeper Intercepts Request]

B --> C[Canonicalize Request]

C --> D[Verify Identity and Signature]

D --> E[Validate Nonce]

E --> F[Query Registry Authority]

F --> G[Evaluate Governance State]

G --> H{Decision}

H -->|PERMIT| I[Allow Execution]

H -->|DENY| J[Block Execution]

I --> K[Record Evidence Event]

J --> K

K --> L[Append to Evidence Chain]
