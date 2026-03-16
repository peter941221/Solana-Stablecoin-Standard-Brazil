# Contributing

```text
contribution flow
|-- open issue
|-- align on scope
|-- send focused PR
`-- include validation notes
```

## Ground Rules

- Use English for public-facing docs, pull requests, and issue discussions.
- Keep changes focused and easy to review.
- Update docs and proof references when product behavior changes.
- Avoid committing secrets, keypairs, or local memory files.

## Local Setup

```bash
npm ci
cd sdk/sss-token && npm ci
cd ../../services && npm ci
cd ..
```

Recommended validation commands:

```bash
npm test
anchor test --skip-local-validator
cargo run -p sss-token-cli -- --help
cd sdk/sss-token && npm run build
cd ../../services && npm test
```

## Pull Requests

- Open an issue first for large changes or architectural shifts.
- Describe the problem, the approach, and the validation you ran.
- Call out risk tier as `L`, `M`, or `H`.
- Include screenshots, logs, or proof references when they help reviewers.

## Documentation

- Keep README and `docs/` aligned.
- Prefer ASCII diagrams when they improve scan speed.
- Keep examples runnable or clearly marked as templates.
