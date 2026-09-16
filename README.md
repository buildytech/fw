# BuildY: Stack-Agnostic Framework Blueprint & Conformance Suite

**Architecture, not code. Ports, not dependencies. Choice, not discipline.**

`fw` is a specification of product capabilities plus a generator for the
composition root. A team describes a product declaratively and gets a
repository that contains exactly the selected modules, implemented on the
stack that team already knows.

This repository ships a contract and a conformance suite. It does not ship
a runtime plugin loader, a mandatory language, or an application with
feature switches.

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
product: wp-theme-studio
contract: 1.x
capabilities:
  workspace: { adapter: fs-local }
  ui:        { adapter: ui-latte }
  host:      { adapter: host-php-cli }
  preview:   { adapter: pack-nginx-mariadb }
  # git, agent, terminal, browse are absent
delivery: portable-windows
```

The manifest is the only place where concrete technologies meet.

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
