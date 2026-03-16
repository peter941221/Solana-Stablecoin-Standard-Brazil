# Deployment

```text
deploy
|-- build programs
|-- publish to target cluster
|-- record program ids and proofs
`-- verify critical flows
```

## Public References

- Detailed runbook: `docs/RUNBOOK.md`
- Devnet summary: `deployments/devnet.json`
- Devnet proof set: `deployments/devnet-sss1-proof.json`, `deployments/devnet-sss2-proof.json`
- Mainnet summary template: `deployments/mainnet.json`

## Devnet Workflow

Prerequisites:

- Solana CLI installed
- Anchor CLI installed
- funded devnet keypair

Scripted deployment:

```bash
ANCHOR_PROVIDER_URL=https://api.devnet.solana.com \
ANCHOR_WALLET=~/.config/solana/id.json \
bash scripts/deploy-devnet.sh
```

Proof generation:

```bash
SSS_CORE_PROGRAM_ID=<stablecoin-core program id> \
SSS_TRANSFER_HOOK_PROGRAM_ID=<transfer-hook program id> \
AUTHORITY_KEYPAIR_PATH=~/.config/solana/id.json \
npx tsx scripts/demo-sss1.ts

SSS_CORE_PROGRAM_ID=<stablecoin-core program id> \
SSS_TRANSFER_HOOK_PROGRAM_ID=<transfer-hook program id> \
AUTHORITY_KEYPAIR_PATH=~/.config/solana/id.json \
npx tsx scripts/demo-sss2.ts
```

Manual verification:

```bash
pwsh scripts/verify-devnet.ps1
pwsh scripts/verify-devnet.ps1 -DryRun

bash scripts/verify-devnet.sh
bash scripts/verify-devnet.sh --dry-run
```

## Mainnet Workflow

```bash
CONFIRM_MAINNET=1 \
ANCHOR_PROVIDER_URL=https://api.mainnet-beta.solana.com \
ANCHOR_WALLET=~/.config/solana/id.json \
bash scripts/deploy-mainnet.sh
```

Mainnet expectations:

- approvals and release selection happen before deployment
- program ids and transaction references are recorded immediately after deployment
- client configs and downstream services are updated in the same release window

## Docker Compose

```bash
docker compose up
docker compose --profile compliant up
```

Environment expectations for live mode:

- `PROGRAM_ID`
- `MINT_ADDRESS`
- `AUTHORITY_KEYPAIR_PATH`
- `SOLANA_RPC_URL`
- `DATABASE_URL`
- `REDIS_URL`
- `SCREENING_API_KEY` for compliance integrations
