# Operations

```text
daily operations
|-- issue supply
|-- reduce supply
|-- freeze or thaw
|-- pause or resume
|-- manage blacklist
`-- manage roles and evidence
```

## Operating Principles

- Separate keys and roles by function.
- Record transaction signatures for every administrative action.
- Update internal evidence trails after each privileged transaction.
- Rehearse pause and rollback procedures before production launch.

## Mint and Burn SOP

1. Confirm the caller has `MASTER_AUTHORITY`, `MINTER`, or `BURNER` as needed.
2. Confirm the stablecoin is not paused.
3. Submit the transaction through SDK, CLI, or service.
4. Record signature, amount, operator, and resulting supply.

## Freeze and Thaw SOP

1. Confirm the caller has `MASTER_AUTHORITY` or `FREEZER`.
2. Confirm the token account belongs to the target mint.
3. Submit `freeze` or `thaw`.
4. Record the reason, affected account, and signature.

## Pause and Unpause SOP

1. Confirm the caller has `MASTER_AUTHORITY` or `PAUSER`.
2. Pause when issuance must stop during an incident or investigation.
3. Unpause only after the incident owner signs off.
4. Record timestamps and incident references.

## Blacklist SOP

```text
SSS-2 only
operator
  -> add or remove blacklist entry
  -> transfer-hook reads the new state
  -> future transfers are allowed or denied
```

1. Confirm transfer-hook is enabled.
2. Confirm the caller has `MASTER_AUTHORITY` or `BLACKLISTER`.
3. Add or remove the blacklist entry.
4. Record the reason and supporting compliance reference.

## Seizure SOP

```text
SSS-2 only
role check + feature check + blacklist check + frozen-account check
```

1. Confirm permanent delegate is enabled.
2. Confirm the target wallet is blacklisted.
3. Confirm the target token account is frozen.
4. Submit the seizure transaction to the treasury destination.
5. Record signature, amount, target account, and approval trail.

## Role Management SOP

1. Reserve `MASTER_AUTHORITY` for the smallest possible key set.
2. Grant scoped roles for daily operations.
3. Set minter quotas where operationally appropriate.
4. Revoke stale or unused roles on a regular cadence.
