# SSS-2

```text
SSS-2 = compliant stablecoin profile
|-- everything in SSS-1
|-- transfer-hook enforcement
|-- blacklist registry
`-- seizure path to treasury
```

## Summary

`SSS-2` extends the base stablecoin profile with transfer-time compliance controls for issuers who need stronger on-chain enforcement.

## Added Features

- Transfer-hook enforcement for senders and receivers
- On-chain blacklist entries
- Permanent delegate based seizure flow
- Expanded role set for compliance operations

## Enabled Extensions

- `MintCloseAuthority`
- `MetadataPointer`
- `TransferHook`
- `PermanentDelegate`
- `DefaultAccountState` when configured

## Transfer Flow

```text
transfer request
  -> Token-2022
  -> transfer-hook
  -> load config and blacklist PDAs
  -> allow or deny
```

## Blacklist and Seizure Model

```text
BlacklistEntry PDA
|-- seed: ["blacklist", config, wallet]
`-- active state used during transfer checks

Seize requirements
|-- caller has MASTER_AUTHORITY or SEIZER
|-- permanent delegate enabled
|-- wallet is blacklisted
`-- token account is frozen
```

## Additional Instructions

- `add_to_blacklist`
- `remove_from_blacklist`
- `seize`

## Roles

Additional SSS-2 roles:

- `BLACKLISTER`
- `SEIZER`

## Security Notes

- The transfer hook reads state but does not mutate business data.
- Seizure is guarded by multiple independent checks.
- A blocked transfer is expected behavior when blacklist rules apply.
