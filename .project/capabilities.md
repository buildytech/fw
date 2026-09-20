# Capabilities

A capability is a port: DTOs, operations, events, invariants, and a
conformance suite. A capability is not a package, a feature flag, a stack
name, or a product type.

Rule: a capability is published only when its conformance suite exists.
Adapters that implement a port live in an external registry.

## Catalog

| Id | Owns | Status |
| --- | --- | --- |
| `host` | process lifecycle and presentation transport | draft |
| `ui` | one selected runtime and generated-file ownership | draft |
| `workspace` | folder as source of truth, read/write, revisions | draft |
| `vcs` | history, staging, diff | draft |
| `agent` | closed event bus for assisted work | draft |
| `delivery` | packaged artifact and its layout | draft |

Unselected ports are physically absent. Product-specific ports belong in an
external registry until more than one product family needs them.

## Contract obligations per capability

Each port document must state all six items. A missing item blocks
publication.

1. **DTOs** — data crossing the boundary, with explicit optionality.
2. **Operations** — request, response, and failure modes.
3. **Events** — what the adapter may emit and when.
4. **Invariants** — what remains true regardless of implementation.
5. **Non-obligations** — what the port explicitly refuses to promise.
6. **Conformance** — scenarios and observable expectations.

## Invariants that stay in the contract

`workspace`, when selected, is authoritative for files. A product must not
add a second content API in the view layer. Other selected capabilities
resolve paths through it.

`ui` does not require a framework or ship primitives. It records one
selected runtime and generated-file ownership. External generators are
registry records, not FW code.

`agent` is a bus, not an SDK. The event set is closed. Bridges are
adapters. The presentation layer never imports a vendor client.

`delivery` owns the on-disk layout of a shipped product, including the
portable case where state lives beside the executable.

## Adding a capability

1. Write the port document with all six obligations.
2. Write the conformance suite before any adapter.
3. Keep adapter implementations in an external registry.
4. Prove a product can omit the capability entirely.
5. Register the port in the manifest schema. `fwyml` consumes that schema.

Step four is the acceptance test for modularity. If omission is not
possible, the port is wrongly scoped.
