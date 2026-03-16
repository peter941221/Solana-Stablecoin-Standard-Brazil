# SSS-1

```text
SSS-1 = minimal stablecoin profile
|-- mint
|-- burn
|-- freeze / thaw
|-- pause / unpause
`-- role-based administration
```

## Summary

`SSS-1` is the base profile for issuers who want controlled minting and administrative safety rails without transfer-time blacklist enforcement.

## Enabled Extensions

- `MintCloseAuthority`
- `MetadataPointer`
- `DefaultAccountState` when configured

Disabled by design:

- `PermanentDelegate`
- `TransferHook`

## PDA Layout

```text
StablecoinConfig PDA
|-- seed: ["stablecoin", mint]
|-- authority and feature flags
`-- supply and operational state

RoleAccount PDA
|-- seed: ["role", config, authority]
`-- bitmask roles plus quota metadata
```

## Instructions

- `initialize`
- `mint`
- `burn`
- `freeze_account`
- `thaw_account`
- `pause`
- `unpause`
- `update_roles`
- `update_minter`
- `transfer_authority`

## Roles

```text
0x01 MASTER_AUTHORITY
0x02 MINTER
0x04 BURNER
0x08 FREEZER
0x10 PAUSER
```

## Intended Use

- Treasury-issued stablecoins with manual compliance outside the transfer path
- Test environments that need simpler operational assumptions
- Products that want the smallest possible on-chain policy surface

## Security Notes

- The config PDA signs privileged mint and freeze actions.
- Every privileged instruction validates the caller role bitmask.
- `SSS-1` rejects compliance-only flows that belong to `SSS-2`.
