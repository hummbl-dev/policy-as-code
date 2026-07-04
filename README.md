# Policy as Code

`policy-as-code` explores how to represent security, compliance, access, and operational policies as version-controlled, reviewable, testable, auditable, and agent-operable source material.

## Working Definition

`Policy-as-Code` means treating security, compliance, access, and operational policies as canonical operational state expressed through files, schemas, examples, tests, and reviewable change history.

## Goals

- Define the domain clearly.
- Collect prior art and examples.
- Provide reusable schemas and templates.
- Support human review and agent execution.
- Preserve auditability, provenance, and governance boundaries.

## Non-Goals

- This repo is not a universal standard.
- This repo is not legal, security, compliance, or operational advice.
- This repo does not canonize HUMMBL/BaseN/Ownward concepts unless explicitly marked and audited.

## Packet status

- `seed` -> `v0.1-draft`

## v0.1 packet locations

- Boundary: [`docs/v0.1-boundary.md`](docs/v0.1-boundary.md)
- Schema: [`schemas/policy-as-code-v0.1.json`](schemas/policy-as-code-v0.1.json)
- Example: [`examples/policy-rule-v0.1.example.json`](examples/policy-rule-v0.1.example.json)
- Fixtures: [`fixtures/valid/policy-rule-v0.1.valid.json`](fixtures/valid/policy-rule-v0.1.valid.json), [`fixtures/invalid/policy-rule-v0.1.invalid.json`](fixtures/invalid/policy-rule-v0.1.invalid.json)

## Status

Public seed repository. Initial executable packet in progress as `seed` -> `v0.1-draft`.
