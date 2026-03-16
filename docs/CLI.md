# CLI

```text
binary
  sss-token
    |-- init
    |-- mint / burn
    |-- freeze / thaw
    |-- pause / unpause
    |-- blacklist
    |-- seize
    |-- minters
    `-- status and reporting
```

## Purpose

The CLI is the operator-facing Rust binary for administrative and reporting workflows against deployed stablecoin programs.

## Global Options

- `--cluster devnet|testnet|mainnet|localnet|URL`
- `--keypair /path/to/keypair.json`
- `--output text|json`

## Common Commands

```bash
sss-token init --preset sss-2 --name "DREX" --symbol "DREX"
sss-token mint <RECIPIENT> <AMOUNT> --mint <MINT_ADDRESS>
sss-token burn <AMOUNT> --mint <MINT_ADDRESS>
sss-token freeze <TOKEN_ACCOUNT> --mint <MINT_ADDRESS>
sss-token thaw <TOKEN_ACCOUNT> --mint <MINT_ADDRESS>
sss-token pause --mint <MINT_ADDRESS>
sss-token unpause --mint <MINT_ADDRESS>
sss-token blacklist add <ADDRESS> --reason "OFAC" --mint <MINT_ADDRESS>
sss-token blacklist remove <ADDRESS> --mint <MINT_ADDRESS>
sss-token blacklist check <ADDRESS> --mint <MINT_ADDRESS>
sss-token seize <TARGET_ATA> --to <TREASURY_ATA> --mint <MINT_ADDRESS>
```

## Reporting Commands

```bash
sss-token minters list --mint <MINT_ADDRESS>
sss-token minters add <ADDRESS> --quota 1000000 --mint <MINT_ADDRESS>
sss-token minters remove <ADDRESS> --mint <MINT_ADDRESS>
sss-token status --mint <MINT_ADDRESS>
sss-token supply --mint <MINT_ADDRESS>
sss-token holders --mint <MINT_ADDRESS>
sss-token audit-log --mint <MINT_ADDRESS>
```

## Operator Notes

- Prefer `--output json` when integrating with scripts or dashboards.
- Use role-specific keys in production rather than a broad admin wallet.
- Treat seizure, blacklist, and pause commands as auditable control actions.
