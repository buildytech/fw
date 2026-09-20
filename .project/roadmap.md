# Roadmap

Phases are gated by exit criteria, not by dates. A phase ends when its
criteria are demonstrable in a repository, not when its tasks are marked
done.

## M0 — Extraction

Harvest proven product behavior and restate it as ports rather than code.

Deliverable: a written port draft for each extracted contract, plus an
explicit list of what turned out to be implementation detail.

Exit: every draft states its six obligations, including non-obligations.

## M1 — Contract and conformance

Three ports end to end: `workspace`, `ui`, `delivery`.

Deliverables:

- `fw-spec` package with DTOs, events, and invariants
- conformance harness over stdio/JSON-RPC
- conformance suites for the three ports

Exit: an adapter the core authors did not write passes `workspace`
conformance without modifying the suite.

## M2 — Schemas consumed by `fwyml`

Publish the schemas and registry kinds that `fwyml` compiles. Materialization
commands live only in `fwyml`. Adapter pins live only in an
external registry.

Deliverables:

- product-manifest, registry-envelope, lock, and harness-result schemas
- registry kinds for capability, adapter, generator, validator, guidance,
  vertical-slice, product-template, and delivery

Exit: a product resolves and builds from a manifest through `fwyml` plus an
external registry alone, and removing a capability from the manifest
removes it from the generated tree. No document or package in this
repository assigns those commands to an `fw` executable.

## M3 — Validation by scenarios

Prove modularity against real compositions rather than examples invented to
fit the design. Product names stay in the external registry and the
consumer repository.

Exit: for each scenario, files of absent capabilities are not present in the
product tree, the dependency manifest, or the delivered artifact. A grep for
the absent port name returns nothing outside documentation.

## M4 — Expansion

Publish additional ports only when each has a conformance suite.

Exit: a third adapter language required no change to any existing port
document.

## M5 — Publication

Turn the repository into a contract other teams can adopt.

Deliverables:

- documentation as the primary artifact, not an appendix
- conformance badge and its publication format
- an intake process for third-party adapters, including versioning rules

Exit: an outside team ships a boxed product without reading core internals,
and every consumer is an ordinary user of the contract with no privileged
access.

## Standing risks

| Risk | Mitigation |
| --- | --- |
| The contract quietly encodes one language | third adapter language early, in M4 at the latest |
| Conformance lags behind ports | no publication without a suite |
| One product keeps special privileges | every consumer stays ordinary |
| Scope creep into a vertical | verticals live in products, never in the core |
| Adapter pins return to this repository | external registry only |
| Modularity becomes flags again | M3 absence test is the acceptance gate |
