# Hackathon Deliverables

```text
submission package
|-- source repository
|-- on-chain programs
|-- SDK and CLI
|-- service layer
|-- docs and proofs
`-- CI workflow
```

## Checklist

- [x] MIT-licensed source repository
- [x] Anchor programs in `programs/stablecoin-core` and `programs/transfer-hook`
- [x] TypeScript SDK in `sdk/sss-token`
- [x] Rust CLI in `cli`
- [x] Service layer in `services`
- [x] Tests in `tests/anchor` and `tests/sdk`
- [x] GitHub Actions workflow in `.github/workflows/ci.yml`
- [x] Public documentation in `docs/`
- [x] Devnet deployment summary in `deployments/devnet.json`
- [x] Devnet proofs in `deployments/devnet-sss1-proof.json` and `deployments/devnet-sss2-proof.json`
- [x] Demo scripts in `scripts/demo-sss1.ts` and `scripts/demo-sss2.ts`
- [x] Deploy scripts for devnet and mainnet

## Program IDs

- `stablecoin_core`: `5T8qkjgJVWcUVza36JVFq3GCiKwAXhunKc8NY2nNbtiZ`
- `transfer_hook`: `5gVGKwPB7qstEN5Kp8fJGCURGPGz2GQnYHQAtD1zKSLB`

## Proof Highlights

- `SSS-1 initialize`: `zRQV34z9B5d1fBq5tsBCSKj9cR7tCoBSt8A4xSQ6bSTksa8b3CKPtKLCQGVNkvA5ezKWRJbZwyd3G6Cr8ZrdEwF`
- `SSS-2 initialize`: `4YJrXkUqqccdjTeAPjF7BnKWvmBhfvxtQT1duGRWE3ECx3YTsruWrZ5uUSfVX3CvEGZau77JTxr6FeeiePhxApet`

## Reproduce Proofs

```bash
NODE_OPTIONS=--dns-result-order=ipv4first DISABLE_AIRDROP=1 \
SSS_CORE_PROGRAM_ID=5T8qkjgJVWcUVza36JVFq3GCiKwAXhunKc8NY2nNbtiZ \
SSS_TRANSFER_HOOK_PROGRAM_ID=5gVGKwPB7qstEN5Kp8fJGCURGPGz2GQnYHQAtD1zKSLB \
SOLANA_RPC_URL=https://api.devnet.solana.com SOLANA_COMMITMENT=confirmed \
AUTHORITY_KEYPAIR_PATH=/path/to/id.json \
PROOF_PATH=deployments/devnet-sss1-proof.json \
npx tsx scripts/demo-sss1.ts
```

```bash
NODE_OPTIONS=--dns-result-order=ipv4first DISABLE_AIRDROP=1 \
SSS_CORE_PROGRAM_ID=5T8qkjgJVWcUVza36JVFq3GCiKwAXhunKc8NY2nNbtiZ \
SSS_TRANSFER_HOOK_PROGRAM_ID=5gVGKwPB7qstEN5Kp8fJGCURGPGz2GQnYHQAtD1zKSLB \
SOLANA_RPC_URL=https://api.devnet.solana.com SOLANA_COMMITMENT=confirmed \
AUTHORITY_KEYPAIR_PATH=/path/to/id.json \
PROOF_PATH=deployments/devnet-sss2-proof.json \
npx tsx scripts/demo-sss2.ts
```

## Notes

- The blocked transfer in the `SSS-2` proof is expected behavior and part of the compliance demonstration.
