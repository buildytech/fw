# BuildY: Stack-Agnostic Framework Blueprint & Conformance Suite

**Architecture, not code. Ports, not dependencies. Choice, not discipline.**

`fw` is a specification of product capabilities, registry records, and a
conformance suite. A team describes a product declaratively; `fwyml` is the
only CLI that compiles that manifest against this registry into a repository
containing exactly the selected modules, implemented on the stack that team
already knows.

This repository ships schemas, port specs, registry source, and conformance
assets. It does not ship a materializer, a runtime plugin loader, a
mandatory language, or an application with feature switches.

## The rule that defines everything

> If an adapter is not in the product manifest, it is not in the tree, not in
> vendor, and not in the link.

Absence is physical. There is no `if !enabled`.

## Three layers

| Layer | Artifact | Knows | Does not know |
| --- | --- | --- | --- |
| Contract | `fw-spec` | ports, DTOs, invariants, events | languages, vendors, files |
| Adapter | `fw-adapter-*` | one implementation of one port | other adapters, the product |
| Product | a studio repository | which adapters are composed | adapter internals |

## Product manifest

```yaml
schemaVersion: fw.buildy.tech/v0alpha1
kind: Product
metadata:
  name: example-product
composition:
  capabilities:
    workspace: { use: workspace-local, contractRange: ">=1 <2" }
    ui: { use: ui-runtime }
    host: { use: host-desktop }
constraints:
  required: [desktop]
  forbidden: [agent]
```

The manifest selects records without embedding package versions. Exact source
references, artifact versions, and SHA-256 digests live in registry records
and the generated lock.

## Planning documents

| Document | Question it answers |
| --- | --- |
| [`.project/intent.md`](.project/intent.md) | Why this exists and what is out of scope |
| [`.project/architecture.md`](.project/architecture.md) | How the three layers hold |
| [`.project/capabilities.md`](.project/capabilities.md) | Which ports exist and what they promise |
| [`.project/roadmap.md`](.project/roadmap.md) | Phases, deliverables, exit criteria |

## Status

Planning. No CLI, no contract packages, no adapters yet. See the roadmap
for the first milestone.
