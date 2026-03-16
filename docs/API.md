# API Reference

```text
services
|-- mint-burn   port 3001
|-- indexer     port 3002
`-- compliance  port 3003
```

## Mint and Burn Service

Endpoints:

- `POST /api/v1/mint`
- `POST /api/v1/burn`
- `GET /api/v1/supply`
- `GET /api/v1/operations`
- `GET /api/v1/health`

Example request:

```json
{
  "recipient": "9xYZ...abc",
  "amount": 1000000,
  "memo": "Customer deposit"
}
```

Example response:

```json
{
  "success": true,
  "signature": "5rTQ...ghi",
  "supply": 10000000,
  "timestamp": "2025-01-15T14:30:00Z"
}
```

Auth and idempotency:

- `Authorization: Bearer <api-key>`
- `X-Idempotency-Key: <uuid>`

## Event Indexer

Endpoints:

- `GET /api/v1/events`
- `GET /api/v1/events/stream`
- `POST /api/v1/webhooks`
- `DELETE /api/v1/webhooks/:id`

Webhook payload:

```json
{
  "event": "TokensMinted",
  "timestamp": "2025-01-15T14:30:00Z",
  "data": {},
  "signature": "5rTQ...ghi"
}
```

## Compliance Service

Endpoints:

- `POST /api/v1/screening/check`
- `GET /api/v1/blacklist`
- `POST /api/v1/blacklist/add`
- `POST /api/v1/blacklist/remove`
- `GET /api/v1/audit/export`
- `POST /api/v1/monitoring/rules`

Screening response:

```json
{
  "address": "9xYZ...abc",
  "riskLevel": "low",
  "isBlacklisted": false,
  "sanctions": {
    "ofac": false,
    "eu": false
  },
  "checkedAt": "2025-01-15T14:30:00Z"
}
```

## Usage Notes

- `mock` mode is useful for demos and local UX work.
- `live` mode requires RPC access and the relevant environment variables.
- Use the indexer service when audit evidence must be queryable after on-chain actions.
