# Compliance

```text
compliance posture
|-- separated operational roles
|-- on-chain blacklist evidence
|-- transfer-time enforcement
`-- exportable audit data
```

## Regulatory Design Goals

- Separate operational roles such as minter, burner, freezer, pauser, blacklister, and seizer.
- Keep blacklist state on chain so enforcement can be demonstrated directly from program state.
- Require explicit approval paths for seizure and other high-risk administrative actions.

## Audit Log Shape

JSON example:

```json
{
  "event_type": "TokensMinted",
  "config_address": "3mNP...def",
  "signature": "5rTQ...ghi",
  "slot": 123456,
  "timestamp": "2025-01-15T14:30:00Z",
  "data": {
    "recipient": "9xYZ...abc",
    "amount": 1000000
  }
}
```

CSV example:

```text
timestamp,action,actor,target,amount,details,tx_signature
2025-01-15T14:30:00Z,MINT,9xYZ...abc,3mNP...def,1000000,,5rTQ...ghi
```

## Screening Integration

- The compliance service can call external screening providers.
- If no provider is configured, the service returns `provider_not_configured`.
- Screening responses should be stored alongside transaction evidence in operational systems.

## Monitoring

- Monitoring rules can be configured through the compliance API.
- Webhook alerts can be signed and delivered to downstream control systems.
- Treat transfer-hook rejections as evidence-producing events, not just user-facing errors.
