# Agent notes

`fw` is the stack-agnostic contract: schemas, port specs, capability
records, and conformance. It is not a CLI, not a product, and not an
adapter warehouse.

`fwyml` is the only compiler. A product manifest plus an **external**
registry become a repository that contains exactly the selected modules.
Unselected capabilities are physically absent. There is no `if !enabled`.

Why this exists, what is out of scope: [`.project/intent.md`](.project/intent.md).
Layers and ownership: [`.project/architecture.md`](.project/architecture.md).
Published ports: [`.project/capabilities.md`](.project/capabilities.md).
Public summary: [`README.md`](README.md).

## Do

- Keep ports observable and stack-agnostic.
- Publish a capability only with a conformance suite.
- Put adapter pins, generators, validators, and product catalogs in an
  external registry.
- Keep schema `$id` and `schemaVersion` as URNs (`urn:fwyml:*`). Do not
  point them at a website.

## Do not

- Add adapter implementations, stack-named tools, or product verticals.
- Teach the contract about a site, CMS, IDE, ops console, or runtime.
- Publish an `fw` executable or materializer. That is `fwyml`.
- Vendor a second content API or a privileged reference product.
