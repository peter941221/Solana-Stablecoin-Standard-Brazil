# Deployment Runbook

```text
release runbook
|-- preflight
|-- deploy
|-- verify
|-- update downstream systems
`-- keep rollback path ready
```

## Scope

- Deploy `stablecoin-core` and `transfer-hook`
- record program ids, proofs, and release references
- verify critical operator and compliance flows

## Preflight Checklist

1. Confirm the working tree matches the intended release commit.
2. Confirm Solana CLI and Anchor CLI versions.
3. Confirm the upgrade authority keypair and balance.
4. Confirm target RPC URL and cluster.
5. Confirm which release tag or commit hash is being deployed.

## Devnet Runbook

1. Deploy the programs.

   ```bash
   ANCHOR_PROVIDER_URL=https://api.devnet.solana.com \
   ANCHOR_WALLET=~/.config/solana/id.json \
   bash scripts/deploy-devnet.sh
   ```

2. Record ids and addresses in `deployments/devnet.json`.
3. Generate proof artifacts with the demo scripts.
4. Verify the core flows and store signatures.
5. Update any client config that depends on program ids.

## Mainnet Runbook

1. Confirm approvals and release sign-off.
2. Deploy the programs.

   ```bash
   CONFIRM_MAINNET=1 \
   ANCHOR_PROVIDER_URL=https://api.mainnet-beta.solana.com \
   ANCHOR_WALLET=~/.config/solana/id.json \
   bash scripts/deploy-mainnet.sh
   ```

3. Record ids and addresses in `deployments/mainnet.json`.
4. Verify a minimal operator flow, including transfer-hook behavior.
5. Update SDK, CLI, and service configuration for the released program ids.

## Rollback Plan

### Option A

Use the same program id and redeploy the previous known-good binaries.

```bash
anchor build
anchor deploy --program-name stablecoin-core
anchor deploy --program-name transfer-hook
```

### Option B

Use a new program id only if upgrade authority is lost or the original id cannot be reused.

Required follow-up:

- generate new program keypairs
- update configs and downstream systems
- reinitialize or migrate operational state where necessary

## Post-Deploy Checklist

- update deployment summaries and proof files
- tag the release and archive release artifacts
- notify downstream services and operators of any program id changes
