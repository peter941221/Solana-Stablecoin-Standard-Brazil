# Hackathon Submission

```text
Solana Stablecoin Standard
|-- two preset profiles
|-- on-chain programs
|-- SDK + CLI + services
`-- devnet proof artifacts
```

## Project Description

Solana Stablecoin Standard is a modular framework for issuing controlled and compliance-aware stablecoins on Solana. It includes on-chain programs, a TypeScript SDK, a Rust CLI, service templates, public documentation, and devnet proof artifacts.

## Links

- Repository: `https://github.com/peter941221/Solana-Stablecoin-Standard-Brazil`

## Highlights

- `SSS-1`: mint, burn, freeze, thaw, pause, and role-based administration
- `SSS-2`: everything in `SSS-1` plus blacklist enforcement and seizure
- Full-stack delivery across programs, SDK, CLI, services, docs, tests, and CI

## Architecture

```text
Applications / integrations
        |
        v
SDK + CLI + services
        |
        v
stablecoin-core + transfer-hook
        |
        v
Token-2022 state and audit evidence
```

## Verification

PowerShell:

```bash
pwsh scripts/verify-devnet.ps1
pwsh scripts/verify-devnet.ps1 -DryRun
pwsh scripts/verify-devnet.ps1 -ProofTag demo-2026-02-24
```

Bash:

```bash
bash scripts/verify-devnet.sh
bash scripts/verify-devnet.sh --dry-run
bash scripts/verify-devnet.sh --proof-tag demo-2026-02-24
```

Manual proof generation:

```bash
NODE_OPTIONS=--dns-result-order=ipv4first DISABLE_AIRDROP=1 \
SSS_CORE_PROGRAM_ID=5T8qkjgJVWcUVza36JVFq3GCiKwAXhunKc8NY2nNbtiZ \
SSS_TRANSFER_HOOK_PROGRAM_ID=5gVGKwPB7qstEN5Kp8fJGCURGPGz2GQnYHQAtD1zKSLB \
SOLANA_RPC_URL=https://api.devnet.solana.com SOLANA_COMMITMENT=confirmed \
AUTHORITY_KEYPAIR_PATH=/path/to/id.json \
PROOF_PATH=deployments/devnet-sss1-proof.json \
npx tsx scripts/demo-sss1.ts
```

```bash
NODE_OPTIONS=--dns-result-order=ipv4first DISABLE_AIRDROP=1 \
SSS_CORE_PROGRAM_ID=5T8qkjgJVWcUVza36JVFq3GCiKwAXhunKc8NY2nNbtiZ \
SSS_TRANSFER_HOOK_PROGRAM_ID=5gVGKwPB7qstEN5Kp8fJGCURGPGz2GQnYHQAtD1zKSLB \
SOLANA_RPC_URL=https://api.devnet.solana.com SOLANA_COMMITMENT=confirmed \
AUTHORITY_KEYPAIR_PATH=/path/to/id.json \
PROOF_PATH=deployments/devnet-sss2-proof.json \
npx tsx scripts/demo-sss2.ts
```

## Notes

- The blocked transfer in `SSS-2` is an expected proof of blacklist enforcement.
