# Intent

BuildY Framework (`fw`) lets a team assemble a boxed software product on
the stack it already knows, while keeping one shared set of invariants. The
deliverable is a contract plus a conformance suite, not an executable with
feature switches.

## The problem

Two failure modes keep repeating.

A single reference application accumulates every feature and hides the
unwanted ones behind flags. Disabled code still ships, still needs
maintenance, and still dictates the language and the host runtime.

The alternative — every team rewriting the same product shape — multiplies
drift. Nothing is comparable, nothing is reusable, and quality depends on
individual discipline.

`fw` takes the third path: define the contract once, let external adapters
implement it per target, and let a test suite enforce identity.

## Position

`fw` is:

- a catalog of capability **ports** with DTOs, events, and invariants
- a **conformance suite** that any implementation can run, in any language
- **schemas and capability records** that describe what a product may select
- a contract that privileges no product type and no stack

`fw` is not:

- a UI framework, an IDE, a CMS, or an ops console
- a product materializer or CLI (`fwyml` owns those commands)
- a runtime plugin loader or a module marketplace
- a warehouse of adapters or product templates
- a dependency injection container for one language
- a hosting platform or a vendor SDK wrapper

## Scope

In scope:

- capability contracts and their versioning
- cross-language conformance execution over a thin protocol
- product manifest, registry, lock, and harness-result schemas
- documentation as the primary product

Out of scope:

- adapter implementations (external registry, separate lifecycles)
- opinionated business logic of any vertical
- a mandatory language, host runtime, or UI runtime
- telemetry collection, licensing servers, multi-tenant services

## Success criteria

1. An outside team ships a boxed product without reading core internals.
2. Removing a capability from the manifest shrinks the tree and the
   delivered artifact. Absence is observable, not cosmetic.
3. The same conformance suite passes for implementations in at least three
   languages, including one the core authors did not write.
4. Site, admin, ops, and desktop products are ordinary consumers of the
   same contract, with no special access.
5. A new capability can be added without changing any existing adapter.

## Anti-goals

- A core that knows about a CMS, an IDE, a panel, or any other vertical,
  by name.
- A second CSS system, a parallel widget tree, or an `iframe` substituting
  for a native view.
- Emulating platform features the platform does not expose.
- Build flags presented as modularity.
- Governance by review convention where a test could decide instead.
- Adapter pins or stack-named tools stored in this repository.
