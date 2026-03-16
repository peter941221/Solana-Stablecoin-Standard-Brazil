# Architecture

```text
Applications and operator tooling
        |
        v
SDK + CLI + services
        |
        v
stablecoin-core -----------------> Token-2022 mint and token accounts
        |
        `------------------------> config, role, and blacklist PDAs

Token-2022 transfer
        |
        v
transfer-hook -------------------> allow or deny based on SSS-2 rules
```

## Layer Model

```text
L1  On-chain state and authority    stablecoin-core
L2  Transfer-time enforcement       transfer-hook
L3  Developer and operator access   SDK + CLI + services
L4  Delivery and assurance          docs + tests + CI + proofs
```

## Program Boundaries

- `stablecoin-core` owns stablecoin lifecycle logic: initialize, mint, burn, freeze, thaw, pause, roles, blacklist updates, and seizure.
- `transfer-hook` is a focused Token-2022 guard that reads stablecoin configuration and blacklist entries before a transfer is accepted.
- The hook does not own business state; it enforces rules against state owned by `stablecoin-core`.

## PDA Model

```text
StablecoinConfig PDA
|-- seed: ["stablecoin", mint]
|-- stores mint, authority, features, totals, treasury
`-- acts as mint and freeze authority

RoleAccount PDA
|-- seed: ["role", config, authority]
|-- stores role bitmask and optional minter quota
`-- enables role separation

BlacklistEntry PDA
|-- seed: ["blacklist", config, wallet]
|-- stores blacklist status and metadata
`-- used by SSS-2 enforcement
```

## Core Flows

### Mint

```text
operator
  -> SDK / CLI / service
  -> stablecoin-core
  -> config PDA signs
  -> Token-2022 mint issues supply
```

### Transfer Enforcement

```text
wallet transfer
  -> Token-2022
  -> transfer-hook CPI
  -> read config + blacklist PDAs
  -> allow or deny
```

### Seizure

```text
authorized operator
  -> stablecoin-core
  -> permanent delegate flow
  -> Token-2022 transfer_checked
  -> treasury ATA receives funds
```

## Security Model

- Role separation reduces operational key concentration.
- Feature flags keep `SSS-1` and `SSS-2` behavior explicit.
- PDA-controlled mint and freeze authority removes direct key ownership from the mint itself.
- `SSS-2` seizure requires role validation, feature validation, blacklist validation, and frozen-account validation.
- Transfer enforcement runs at transfer time instead of relying on off-chain monitoring alone.
