# Roadmap

Phases are gated by exit criteria, not by dates. A phase ends when its
criteria are demonstrable in a repository, not when its tasks are marked
done.

## M0 — Extraction

Harvest what is already proven in the reference desktop product and restate
it as ports rather than code.

Sources: workspace ownership and revision-checked writes, portable
on-disk layout, build-time product composition, the closed agent event bus,
and the native second view with its debug protocol.

Deliverable: a written port draft for each of the above, plus an explicit
list of what turned out to be implementation detail.

Exit: every draft states its six obligations, including non-obligations.

## M1 — Contract and conformance

Three ports end to end: `workspace`, `ui`, `delivery`.

Deliverables:

- `fw-spec` package with DTOs, events, and invariants
- conformance harness over stdio/JSON-RPC
- conformance suites for the three ports
- two reference adapters per port, in two different languages

Exit: a non-Go adapter passes `workspace` conformance without the core
authors modifying the suite.

## M2 — Generator

The CLI that turns a manifest into a composition root.

Deliverables:

- manifest schema and validation
- `fw new`, `fw sync`, `fw verify`
- two product templates: a desktop host with a TypeScript UI runtime, and a
  server-rendered host with a PHP UI runtime

Exit: both templates build from a manifest alone, and removing a capability
from the manifest removes it from the generated tree.

## M3 — Validation by scenarios

Prove modularity against real compositions rather than examples invented to
fit the design.

Scenarios:

| Product | Capabilities present | Capabilities absent |
| --- | --- | --- |
| Workshop without assistance | workspace, ui, host, editor, explorer, vcs | agent |
| Assisted workshop without browsing | workspace, ui, host, editor, agent | browse |
| Assistant surface only | ui, host, agent, browse | editor, explorer |
| Offline workshop | workspace, ui, host, editor, explorer | vcs, agent |
| Theme studio with local stack | workspace, ui, host, pack, delivery | vcs, agent, terminal |

Exit: for each scenario, files of absent capabilities are not present in the
product tree, the dependency manifest, or the delivered artifact. A grep for
the absent port name returns nothing outside documentation.

## M4 — Expansion

Add `vcs`, `agent`, `browse`, and `pack` to the published contract.

Deliverables: port documents, conformance suites, one adapter each, and a
third adapter language introduced deliberately to expose hidden assumptions
in the harness.

Exit: the third language required no change to any existing port document.

## M5 — Publication

Turn the repository into a product other teams can adopt.

Deliverables:

- documentation as the primary artifact, not an appendix
- studio templates and a getting-started path
- conformance badge and its publication format
- an intake process for third-party adapters, including versioning rules

Exit: an outside team ships a boxed product without reading core internals,
and the reference desktop product is rebuilt as an ordinary consumer of the
contract with no privileged access.

## Standing risks

| Risk | Mitigation |
| --- | --- |
| The contract quietly encodes one language | third adapter language early, in M4 at the latest |
| Conformance lags behind ports | no publication without a suite |
| The reference product keeps special privileges | rebuild it as a plain consumer in M5 |
| Scope creep into a vertical | verticals live in products, never in the core |
| Modularity becomes flags again | M3 grep test is the acceptance gate |
