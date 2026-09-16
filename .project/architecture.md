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
product: wp-theme-studio
contract: 1.x
capabilities:
  workspace: { adapter: fs-local }
  ui:        { adapter: ui-latte }
  host:      { adapter: host-php-cli }
  preview:   { adapter: pack-nginx-mariadb }
delivery: portable-windows
```

Absent keys mean absent capabilities. There is no `enabled: false`.

## Generator

The CLI reads the manifest and writes the composition root.

| Command | Effect |
| --- | --- |
| `fw new` | scaffold a product repository from the manifest |
| `fw sync` | reconcile an existing root after a manifest change |
| `fw verify` | run conformance for the selected capabilities |
| `fw pack` | produce the delivery artifact through the `delivery` adapter |

`fw sync` declares dependencies; it does not vendor adapter source into the
product. Removing a capability from the manifest removes generated glue and
the dependency, and `fw verify` then fails if product code still references
the port.

## Conformance as the gate

The conformance suite is the most valuable asset in this repository. It
copies the parity approach proven in `@ui8kit/codegen`, where a canonical
renderer is the executable specification rather than a review convention.

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
| `buildytech/fw` | contract, conformance, CLI, templates, documentation |
| `buildytech/fw-adapters-<family>` | adapters grouped by language or platform |
| `buildytech/fw-examples` | reference products, including the desktop IDE |
| studio repositories | product manifests and product code |

A monorepo is acceptable inside one adapter family. It is not acceptable
between the contract and its implementations: that collapses back into one
repository governed by rules.

## Versioning

The contract uses semantic versioning independently of any adapter. Each
capability is versioned separately, for example `workspace@1` and
`agent@0`. A product pins a range.

| Change | Level |
| --- | --- |
| New optional operation or event | minor |
| Changed observable behavior or invariant | major |
| New adapter idiom or internal rewrite | not a contract event |

Experimental ports stay at major zero and carry no compatibility promise.

## Ownership boundaries carried over from practice

These were learned in the reference desktop product and are contract-level,
not implementation detail.

- The workspace folder is authoritative. A product must not invent a second
  content API in the view layer.
- Host paths, process handles, and secrets never reach the presentation
  layer.
- Agent runtimes are bridges behind a closed event bus. A vendor SDK is an
  adapter, never a core dependency.
- Neighbouring processes (web server, database, sidecar) belong to a `pack`
  adapter with an explicit lifecycle, ports, and logs.
- Where a platform does not expose a capability, the contract says so.
  Faking it is a defect, not a feature.
