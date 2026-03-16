# Security Policy

```text
security priorities
|-- protect keys
|-- limit privileges
|-- verify releases
`-- report issues privately first
```

## Supported Scope

This repository contains reference implementations for stablecoin issuance tooling on Solana. Security-sensitive areas include:

- role validation and privilege boundaries
- mint, freeze, blacklist, and seizure flows
- service authentication and idempotency
- deployment scripts and operator procedures

## Reporting a Vulnerability

- Do not open a public GitHub issue for undisclosed vulnerabilities.
- Report privately to the repository owner with reproduction steps, impact, and any proof material.
- Include the affected component, cluster, commit hash, and whether the issue is exploitable on a public network.

## Response Expectations

- Acknowledge receipt
- assess severity and affected surface
- prepare a fix or mitigation plan
- publish coordinated remediation details after a patch is available

## Operational Security Notes

- Never commit keypairs, seed phrases, API secrets, or private environment files.
- Use narrowly scoped operational roles where possible.
- Treat mainnet deployment as a high-risk workflow with rollback planning and explicit approvals.
