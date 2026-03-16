# SDK

```text
app code
  -> SolanaStablecoin
      |-- lifecycle methods
      |-- compliance module
      `-- role manager
```

## Package

- Package name: `@stbr/sss-token`
- Purpose: provide a TypeScript interface for creating and operating SSS stablecoins
- Runtime target: Solana `web3.js` + Anchor-compatible workflows

## Install

```bash
npm install @stbr/sss-token
```

Local development:

```bash
cd sdk/sss-token
npm ci
npm run build
```

## Presets

```text
SSS-1
|-- mint / burn
|-- freeze / thaw
`-- pause / roles

SSS-2
|-- everything in SSS-1
|-- transfer hook
|-- blacklist support
`-- seizure support
```

## Main API

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

## Example

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

## Custom Configuration

- Use a preset when you want a standard behavior profile.
- Use extension overrides when you need a custom profile.
- Confidential transfer is currently rejected by the SDK.

Common overrides:

- `permanentDelegate`
- `transferHook`
- `defaultAccountFrozen`

## Error Handling

- The SDK wraps Anchor failures into typed JavaScript errors where possible.
- Use `try/catch` around transaction submission and surface readable failures to operators.
- Treat proof generation and administrative actions as auditable workflows, not silent background actions.
