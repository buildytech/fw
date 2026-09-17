# Composition semantics

Vertical slices are registry records. They are not FW capabilities.

Each `vertical-slice` record declares:

- required capabilities;
- provided surfaces;
- compatible UI and host families;
- required adapters or abstract adapter constraints;
- generated or copied files it owns;
- dependencies and conflicts;
- removal behavior;
- guidance and validators it contributes.

A product manifest selects one UI runtime through its `ui` adapter. A slice
may list several UI families; resolution keeps exactly one runtime already
chosen by the product.

## Compatibility selection

The manifest may attach a `contractRange` to each selected capability. The
selected adapter's declared contract must satisfy that range. Contract ranges
are evaluated by `fwyml`; a registry id, package name, or implementation
family is not special to the resolver.

`constraints.required` and `constraints.forbidden` are evaluated against
generic compatibility terms declared by selected records:

- record ids and provided capabilities;
- declared `features`;
- supported platforms and runtimes.

Features are descriptive compatibility terms, not a second capability
catalog. They let a product require a portable, platform, lifecycle, or
runtime property without teaching FW about any product vertical.

Selected records may not require conflicting exact versions of one dependency.
Resolution fails before materialization with a stable dependency-conflict
diagnostic; record ordering never chooses a version implicitly.

Removal is deterministic. After a slice is dropped, no file, dependency,
menu item, route, generated binding, or delivery artifact it owns remains.

The lock records a digest for every owned output. A sync may delete an output
that was owned by the previous lock only when its current digest matches that
recorded value. A changed output is a conflict, never a deletion.

The same `agent-chat-surface` contract may resolve to a Svelte or Solid
artifact through adapter and slice data. FW does not import either
framework.
