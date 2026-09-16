# Intent

BuildY Framework (`fw`) lets an agency assemble a boxed software product on
the stack its team already knows, while keeping one shared set of
invariants. The deliverable is a contract plus a conformance suite, not an
executable with feature switches.

## The problem

Two failure modes keep repeating.

A single reference application accumulates every feature and hides the
unwanted ones behind flags. Disabled code still ships, still needs
maintenance, and still dictates the language and the host runtime.

The alternative — every agency rewriting the same workshop — multiplies
drift. Nothing is comparable, nothing is reusable, and quality depends on
individual discipline.

`fw` takes the third path, already proven for UI primitives by
`@ui8kit/codegen`: define the contract once, print idiomatic
implementations per target, and let a test suite enforce identity.

## Position

`fw` is:

- a catalog of capability **ports** with DTOs, events, and invariants
- a **conformance suite** that any implementation can run, in any language
- a **generator** that produces the composition root of a product
- a set of reference products, none of which is privileged

`fw` is not:

- a UI framework, an IDE, or an editor
- a runtime plugin loader or a module marketplace
- a dependency injection container for one language
- a hosting platform or a vendor SDK wrapper

## Scope

In scope:

- capability contracts and their versioning
- cross-language conformance execution over a thin protocol
- product manifest format and the `fw` CLI that reads it
- product templates and delivery layouts
- documentation as the primary product

Out of scope:

- adapter implementations (separate repositories, separate lifecycles)
- opinionated business logic of any vertical
- a mandatory language, host runtime, or UI runtime
- telemetry collection, licensing servers, multi-tenant services

## Success criteria

1. An outside team ships a boxed product without reading core internals.
2. Removing a capability from the manifest shrinks the tree and the
   delivered artifact. Absence is observable, not cosmetic.
3. The same conformance suite passes for implementations in at least three
   languages, including one the core authors did not write.
4. The existing desktop IDE exists as an ordinary product on top of `fw`,
   with no special access.
5. A new capability can be added without changing any existing adapter.

## Anti-goals

- A core that knows about WordPress, or any other vertical, by name.
- A second CSS system, a parallel widget tree, or an `iframe` substituting
  for a native view.
- Emulating platform features the platform does not expose.
- Build flags presented as modularity.
- Governance by review convention where a test could decide instead.

## Related work in the same family

`@ui8kit/codegen` already answers the UI half: one brick definition, seven
runtimes, parity tests as the gate, and a generator that writes only the
requested targets. `fw` applies the same shape to host capabilities.
