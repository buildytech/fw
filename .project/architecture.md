# Architecture

## Dependency direction

```text
fw-spec           contract: ports, DTOs, events, invariants
    ^
    |  implements, never imported by
fw-adapter-*      one implementation of one port, one language
    ^
    |  declared in the manifest
product           composition root owned by the studio
```

The contract never imports an adapter. An adapter never imports another
adapter. The product is the only place where concrete technologies meet.

## Three layers

| Layer | Artifact | Knows | Does not know |
| --- | --- | --- | --- |
| Contract | `fw-spec` | ports, DTOs, invariants, events | languages, vendors, files |
| Adapter | `fw-adapter-*` | one port implementation | other adapters, the product |
| Product | studio repository | which adapters are composed | adapter internals |

## Physical absence

A module that is not selected must be absent from the dependency graph, not
disabled at runtime.

| Mechanism | Strength | When to use |
| --- | --- | --- |
| Generator output | writes only requested targets | UI runtimes, templates, scaffolding |
| Manifest dependency | compiler never sees the module | capability adapters |
| Separate repository | source is never cloned | cross-language adapters |

Build tags inside one source tree are a half measure: the code still lives
in the same repository and still constrains the language. They are
acceptable inside a single adapter, never as the product boundary.

## Product manifest

One human- and machine-readable file at the product root:

```yaml
schemaVersion: urn:fwyml:manifest:v0alpha1
kind: Product
metadata:
  name: example-product
composition:
  capabilities:
    workspace: { use: workspace-local, contractRange: ">=1 <2" }
    ui: { use: ui-runtime }
    host: { use: host-desktop }
  slices: []
constraints:
  required: [desktop]
  forbidden: [agent]
```

The manifest selects registry records; it does not name package versions or
implementations. Exact artifacts, source references, and digests are resolved
into the lock. Absent keys mean absent capabilities. There is no `enabled:
false`.

## Materializer boundary

`fwyml` is the only CLI and materializer. It reads an FW product
manifest plus registry data and writes the composition root. This
repository does not publish an `fw` executable or product-generation
commands. Adapter pins are not stored here.

| Command (in `fwyml`) | Effect |
| --- | --- |
| `fwyml validate` | check a product manifest against FW schemas |
| `fwyml resolve` | compute the selected graph, absences, and lock proposal |
| `fwyml fetch` | acquire selected pinned Git sources |
| `fwyml sync` | reconcile an existing root after a manifest change |
| `fwyml verify` | run selected validators and FW conformance |
| `fwyml generate` | run selected generate-phase tools against a matching lock |
| `fwyml context` | emit a grounded pack for an external agent or QA |

`fwyml sync` declares dependencies from registry records; it does not vendor
adapter source into the product unless a record's source kind says so.
Removing a capability from the manifest removes owned glue and the
dependency only when the former outputs still match their recorded ownership
digests. `fwyml verify` first checks the persisted lock against the manifest,
registry snapshot, and materialization plan, then runs selected validators.

## Conformance as the gate

The conformance suite is the most valuable asset in this repository.

- every port ships reference scenarios and expected observable effects
- an adapter in any language runs the same suite
- execution happens over a thin stdio/JSON-RPC harness, so no shared
  runtime is required
- the badge `conformance: workspace@1.2 passed` is the only definition of
  compatible

This is what replaces discipline. Inside one product, discipline still
applies — one UI runtime, one host, one kit. Between products, freedom
applies.

## Repository topology

| Repository | Contents |
| --- | --- |
| `fw` | schemas, port specs, capability records, conformance, documentation |
| `fwyml` | generic CLI, resolver, materializer, lock, context pack |
| external registry | adapter, generator, validator, slice, and delivery pins |
| studio repositories | product manifests and product code |

A monorepo is acceptable inside one adapter family. It is not acceptable
between the contract and its implementations: that collapses back into one
repository governed by rules.

## Versioning

The contract uses semantic versioning independently of any adapter. Each
capability is versioned separately, for example `workspace@1` and
`agent@0`. A product may pin a capability contract range; records supply exact
artifact versions and source digests to the lock.

| Change | Level |
| --- | --- |
| New optional operation or event | minor |
| Changed observable behavior or invariant | major |
| New adapter idiom or internal rewrite | not a contract event |

Experimental ports stay at major zero and carry no compatibility promise.

Record features, platforms, and runtimes are generic compatibility terms. They
may satisfy required or forbidden product constraints, but do not add a port
or teach FW an implementation name.

## Ownership boundaries

These are contract-level, not implementation detail.

- A selected workspace folder is authoritative. A product must not invent a
  second content API in the view layer.
- Host paths, process handles, and secrets never reach the presentation
  layer.
- Agent runtimes are bridges behind a closed event bus. A vendor SDK is an
  adapter, never a core dependency.
- Neighbouring processes belong to an adapter with an explicit lifecycle.
- Where a platform does not expose a capability, the contract says so.
  Faking it is a defect, not a feature.
