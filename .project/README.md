# .project

Operator notes for BuildY Framework (`fw`). These documents are the source
of intent; the repository root README is the public summary.

| Document | Scope |
| --- | --- |
| [`intent.md`](intent.md) | Purpose, scope boundary, success criteria, anti-goals |
| [`architecture.md`](architecture.md) | Three layers, manifest, fwyml boundary, conformance, topology |
| [`capabilities.md`](capabilities.md) | Port catalog and contract obligations |
| [`roadmap.md`](roadmap.md) | Phases M0–M5 with exit criteria |

## Working rules

- English only in this repository.
- Brand-neutral core: no customer, mascot, or product names in the contract.
- No adapter code or product materializer lands here. This repository holds
  schemas, spec, registry source, conformance, and documentation. `fwyml`
  is the only CLI.
- A capability is not published until it has a conformance suite.
- Prefer removing a port over weakening its invariants.

## Decisions

Architecture decisions live in `decisions/` as numbered records once the
first contract package exists. Until then this folder is the record.
