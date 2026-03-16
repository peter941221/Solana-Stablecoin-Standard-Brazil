# Solana Stablecoin Standard Brazil

[![CI](https://github.com/peter941221/Solana-Stablecoin-Standard-Brazil/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/peter941221/Solana-Stablecoin-Standard-Brazil/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Docs](https://img.shields.io/badge/docs-index-blue)](docs/README.md)
[![Security](https://img.shields.io/badge/security-policy-red)](SECURITY.md)
[![Contributing](https://img.shields.io/badge/contributing-guide-orange)](CONTRIBUTING.md)
[![Issues](https://img.shields.io/github/issues/peter941221/Solana-Stablecoin-Standard-Brazil)](https://github.com/peter941221/Solana-Stablecoin-Standard-Brazil/issues)
[![Last Commit](https://img.shields.io/github/last-commit/peter941221/Solana-Stablecoin-Standard-Brazil)](https://github.com/peter941221/Solana-Stablecoin-Standard-Brazil/commits/main)
![Solana](https://img.shields.io/badge/Solana-1.18+-9945FF?logo=solana&logoColor=white)
![Anchor](https://img.shields.io/badge/Anchor-0.30.1-2F7DF4)
![Token-2022](https://img.shields.io/badge/Token--2022-enabled-14F195)
![TypeScript](https://img.shields.io/badge/TypeScript-SDK-3178C6?logo=typescript&logoColor=white)
![Rust](https://img.shields.io/badge/Rust-on--chain-000000?logo=rust&logoColor=white)

```text
   _____ ____   _____
  / ___// __/  / ___/  Solana Stablecoin Standard
  \__ \/ _/    \__ \   Token-2022 + Anchor + SDK + CLI + Services
 ___/ /_/     ___/ /   Modular stablecoin rails with preset profiles
/____/(_)    /____/    Built for operations, compliance, and proofs
```

Build stablecoin infrastructure on Solana with a layered stack:

```text
L1  Base program logic      stablecoin-core
L2  Compliance guardrails   transfer-hook
L3  Operator tooling        SDK + CLI + services
L4  Delivery surface        docs + CI + deployment proofs
```

This repository ships two opinionated profiles:

```text
SSS-1
|-- mint
|-- burn
|-- freeze / thaw
|-- pause / unpause
`-- role-based operations

SSS-2
|-- everything in SSS-1
|-- on-chain blacklist registry
|-- transfer-hook enforcement
`-- seizure flow via permanent delegate
```

## Open Source Surface

```text
community
|-- README landing page
|-- docs/ public handbook
|-- issue templates
|-- PR template
|-- contributing guide
`-- security policy
```

- Start in `README.md` for the product overview and quick start.
- Move to `docs/README.md` for the full public documentation map.
- Use GitHub issue templates and the PR template for changes that need review.
- Follow `CONTRIBUTING.md` for local setup and validation expectations.
- Use `SECURITY.md` for private vulnerability reporting.

## Why This Repo Exists

```text
operator
  |
  v
SDK / CLI / API
  |
  v
stablecoin-core --------------------> Token-2022 mint + token accounts
  |
  +----> role PDAs
  |
  `----> config PDA

Token-2022 transfer
  |
  v
transfer-hook ----------------------> blacklist PDAs
  |
  `----> allow / deny
```

- `stablecoin-core` is the main Anchor program for initialization, minting, burning, freezing, pausing, role management, and SSS-2 seizure operations.
- `transfer-hook` is a dedicated Token-2022 hook program that blocks transfers when blacklist rules say a transfer must be denied.
- `sdk/sss-token` provides a TypeScript surface for app developers and automation scripts.
- `cli` provides a Rust operator interface for day-to-day administration.
- `services` provides Fastify services for mint/burn operations, compliance workflows, and event indexing.
- `deployments` stores published proof artifacts for devnet and mainnet environments.

## Feature Map

```text
+-------------------+     +-------------------+     +----------------------+
| stablecoin-core   | --> | transfer-hook     | --> | external operators   |
| mint / burn       |     | blacklist checks  |     | SDK / CLI / APIs     |
| freeze / thaw     |     | read-only guard   |     | wallets / backends   |
| pause / roles     |     | SSS-2 only        |     | audit pipelines      |
+-------------------+     +-------------------+     +----------------------+
```

```text
Role model

MASTER_AUTHORITY
|-- MINTER
|-- BURNER
|-- FREEZER
|-- PAUSER
|-- BLACKLISTER
`-- SEIZER
```

```text
Mint flow

operator -> SDK / CLI -> stablecoin-core -> PDA authority -> Token-2022 mint

Transfer flow (SSS-2)

wallet -> Token-2022 -> transfer-hook -> config + blacklist PDAs -> allow / deny
```

## Repository Layout

```text
solana-stablecoin-standard-brazil/
|-- programs/
|   |-- stablecoin-core/
|   `-- transfer-hook/
|-- sdk/
|   `-- sss-token/
|-- cli/
|-- services/
|-- tests/
|   |-- anchor/
|   `-- sdk/
|-- scripts/
|-- deployments/
|-- docs/
|-- .github/
|   |-- workflows/
|   `-- ISSUE_TEMPLATE/
`-- README.md
```

## Quick Start

### 1. Install toolchains

```text
Node.js      20+
Rust         stable
Solana CLI   required for localnet/devnet/mainnet flows
Anchor CLI   0.30.1
Docker       optional, for service stacks
```

### 2. Install dependencies

```bash
npm ci
cd sdk/sss-token && npm ci
cd ../../services && npm ci
cd ..
```

### 3. Prepare a local validator

```bash
solana-keygen new --no-bip39-passphrase -o ~/.config/solana/id.json
solana config set --url http://127.0.0.1:8890 --keypair ~/.config/solana/id.json
solana-test-validator --reset --quiet --rpc-port 8890
```

Open a second terminal and run:

```bash
solana airdrop 10
anchor build
anchor test --skip-local-validator
```

### 4. Run targeted developer loops

```bash
# Root TypeScript tests
npm test

# CLI help
cargo run -p sss-token-cli -- --help

# SDK build
cd sdk/sss-token && npm run build

# Services tests
cd services && npm test
```

## CLI Surface

```text
sss-token
|-- init
|-- mint
|-- burn
|-- freeze
|-- thaw
|-- pause
|-- unpause
|-- blacklist add/remove/check
|-- seize
|-- minters list/add/remove
|-- status
|-- supply
|-- holders
`-- audit-log
```

Examples:

```bash
cargo run -p sss-token-cli -- init --preset sss-2 --name "DREX" --symbol "DREX"
cargo run -p sss-token-cli -- mint <RECIPIENT> <AMOUNT> --mint <MINT_ADDRESS>
cargo run -p sss-token-cli -- blacklist add <ADDRESS> --reason "OFAC" --mint <MINT_ADDRESS>
cargo run -p sss-token-cli -- seize <TARGET_ATA> --to <TREASURY_ATA> --mint <MINT_ADDRESS>
```

## SDK Surface

```text
SolanaStablecoin
|-- create
|-- fromExisting
|-- mint
|-- burn
|-- freeze
|-- thaw
|-- pause
|-- unpause
|-- compliance.*
`-- roles.*
```

```ts
import { Connection, Keypair, PublicKey } from "@solana/web3.js";
import { Presets, SolanaStablecoin } from "@stbr/sss-token";

const connection = new Connection("https://api.devnet.solana.com");
const authority = Keypair.generate();

const stablecoin = await SolanaStablecoin.create(connection, {
  preset: Presets.SSS_2,
  name: "DREX",
  symbol: "DREX",
  decimals: 6,
  authority,
});

const recipient = new PublicKey("9xYZ...abc");
await stablecoin.mint({ recipient, amount: 1_000_000 });
```

## Services and APIs

```text
services
|-- mint-burn   port 3001
|-- indexer     port 3002
`-- compliance  port 3003
```

```text
mint-burn
|-- POST /api/v1/mint
|-- POST /api/v1/burn
|-- GET  /api/v1/supply
|-- GET  /api/v1/operations
`-- GET  /api/v1/health

indexer
|-- GET    /api/v1/events
|-- GET    /api/v1/events/stream
|-- POST   /api/v1/webhooks
`-- DELETE /api/v1/webhooks/:id

compliance
|-- POST /api/v1/screening/check
|-- GET  /api/v1/blacklist
|-- POST /api/v1/blacklist/add
|-- POST /api/v1/blacklist/remove
|-- GET  /api/v1/audit/export
`-- POST /api/v1/monitoring/rules
```

Run service workflows with either `docker compose up` for the base profile or `docker compose --profile compliant up` for the compliance profile.

## Deployment and Proofs

```text
deployments/
|-- devnet.json
|-- devnet-sss1-proof.json
|-- devnet-sss2-proof.json
`-- mainnet.json
```

Useful commands:

```bash
# Devnet deployment
bash scripts/deploy-devnet.sh

# Mainnet deployment
CONFIRM_MAINNET=1 bash scripts/deploy-mainnet.sh

# Proof verification (PowerShell)
pwsh scripts/verify-devnet.ps1
pwsh scripts/verify-devnet.ps1 -DryRun

# Proof verification (Bash)
bash scripts/verify-devnet.sh
bash scripts/verify-devnet.sh --dry-run
```

The current devnet proof set already includes transaction traces for:

```text
SSS-1
|-- initialize
|-- mint
|-- transfer
|-- freeze
|-- thaw
`-- burn

SSS-2
|-- initialize
|-- mint
|-- transfer
|-- blacklist
|-- blocked transfer
|-- freeze
`-- seize
```

## Documentation Map

```text
docs/
|-- README.md
|-- ARCHITECTURE.md
|-- SDK.md
|-- OPERATIONS.md
|-- SSS-1.md
|-- SSS-2.md
|-- COMPLIANCE.md
|-- API.md
|-- CLI.md
|-- DEPLOYMENT.md
|-- RUNBOOK.md
|-- HACKATHON-DELIVERABLES.md
|-- HACKATHON-SUBMISSION.md
`-- HACKATHON-SUBMISSION-TEMPLATE.md
```

Start here if you want:

- docs index: [`docs/README.md`](docs/README.md)
- architecture: [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)
- operator flows: [`docs/OPERATIONS.md`](docs/OPERATIONS.md)
- SDK usage: [`docs/SDK.md`](docs/SDK.md)
- CLI usage: [`docs/CLI.md`](docs/CLI.md)
- deployment details: [`docs/DEPLOYMENT.md`](docs/DEPLOYMENT.md)
- runbook and rollback: [`docs/RUNBOOK.md`](docs/RUNBOOK.md)

## Contributing

```text
GitHub hygiene
|-- issue templates for bugs, features, and docs
|-- PR template with validation and risk prompts
`-- CI on push to main and on pull requests
```

- Open an issue with the matching template before large changes.
- Keep public-facing documentation in English.
- Prefer small, reviewable pull requests with explicit validation notes.
- Update docs and proof artifacts when behavior changes.

## Compliance Note

This repository is a technical implementation reference, not legal advice. If you plan to issue a real stablecoin, combine these building blocks with jurisdiction-specific legal review, custody controls, key management, monitoring, and incident response.

## License

MIT
