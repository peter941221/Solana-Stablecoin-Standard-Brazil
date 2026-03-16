# Hackathon Submission Template

```text
copy this template
|-- fill project identity
|-- attach proof links
|-- explain architecture
`-- show how judges can verify the build
```

## Project Name

Solana Stablecoin Standard

## One-Line Summary

Modular standard for issuing controlled and compliance-aware stablecoins on Solana with programs, SDK, CLI, services, and proof artifacts.

## Links

- Repository: `<REPO_URL>`
- `SSS-1` proof: `deployments/devnet-sss1-proof.json`
- `SSS-2` proof: `deployments/devnet-sss2-proof.json`
- Deployment summary: `deployments/devnet.json`

## What It Does

- `SSS-1` covers mint, burn, freeze, thaw, pause, and role-based administration.
- `SSS-2` adds transfer-hook enforcement, blacklist management, and seizure support.

## Why It Matters

It provides a reusable foundation for stablecoin projects that need strong operational controls and a path toward compliance-aware issuance on Solana.

## Architecture

```text
programs
|-- stablecoin-core
`-- transfer-hook

operator and developer layer
|-- SDK
|-- CLI
`-- services
```

## How To Verify

PowerShell:

```bash
pwsh scripts/verify-devnet.ps1
pwsh scripts/verify-devnet.ps1 -DryRun
pwsh scripts/verify-devnet.ps1 -ProofTag <proof-tag>
```

Bash:

```bash
bash scripts/verify-devnet.sh
bash scripts/verify-devnet.sh --dry-run
bash scripts/verify-devnet.sh --proof-tag <proof-tag>
```

Manual proof generation:

```bash
NODE_OPTIONS=--dns-result-order=ipv4first DISABLE_AIRDROP=1 \
SSS_CORE_PROGRAM_ID=<stablecoin-core-program-id> \
SSS_TRANSFER_HOOK_PROGRAM_ID=<transfer-hook-program-id> \
SOLANA_RPC_URL=https://api.devnet.solana.com SOLANA_COMMITMENT=confirmed \
AUTHORITY_KEYPAIR_PATH=/path/to/id.json \
PROOF_PATH=deployments/devnet-sss1-proof.json \
npx tsx scripts/demo-sss1.ts
```

```bash
NODE_OPTIONS=--dns-result-order=ipv4first DISABLE_AIRDROP=1 \
SSS_CORE_PROGRAM_ID=<stablecoin-core-program-id> \
SSS_TRANSFER_HOOK_PROGRAM_ID=<transfer-hook-program-id> \
SOLANA_RPC_URL=https://api.devnet.solana.com SOLANA_COMMITMENT=confirmed \
AUTHORITY_KEYPAIR_PATH=/path/to/id.json \
PROOF_PATH=deployments/devnet-sss2-proof.json \
npx tsx scripts/demo-sss2.ts
```

## Notes

- Mention any expected blocked transfers or compliance checks so reviewers do not mistake them for failures.
