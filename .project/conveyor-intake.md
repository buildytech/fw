# Conveyor intake — 2026-09-21

Scope: establish stack-neutral assembly and prepare the first external
Go/Templ product conveyor. This is an evidence report, not a product catalog
or an adapter implementation. No ports are promoted from draft.

## Evidence identity

All five working trees were clean at intake. Upstream freshness is unknown;
no remote release equivalence is claimed.

- fw: `e250d3f9dedf59be688ff7f2a5eb2354c8cc0f7c`
- fwyml: `a8306f1f7f2581be4312c4c61a2dec9ccdac4bd3`
- SiteStarter: `b5a45ef8a429b0a9eb195e52c9fdcd3288cf256c`
- UI8Kit Codegen: `972b49224fb239c72b5b488d9d67b733893b10e7`
- UI8Kit Registry: `d0f3897d1eb039beb4e83103e9780f7d619da8b4`

Read: agent instructions, FW intent/architecture/schemas/ports/scenarios;
compiler extraction policy, resolver, registry loader, materializer, verifier,
tool runner and tests; product stack/UI rules, manifests, BFF composition,
content routes, admin receipt and promotion script; UI8Kit README and button
definition. Findings describe these local revisions only.

## Findings and ownership

1. **CLI / release blocker:** materialize.ts emits Go and npm files for every
   product, including a no-op build. Remove implicit language selection.
   Emit package manifests only for selected declarations; require an explicit
   Go module and language version when synthesizing go.mod. Identity glue
   belongs to selected artifacts. Schema addition: optional go.version.
2. **CLI / release blocker:** copying and hashing via readText corrupts binary
   artifacts. Preserve bytes throughout writes, rollback, digest and removal.
3. **Validator / release blocker:** tools.ts reports missing selected tools as
   warnings, allowing generate/verify success. Missing prerequisites must fail.
4. **CLI:** traversal schedules dependents before dependencies; cycles and
   order-dependent conflicts need explicit failure and regression coverage.
5. **CLI:** registry paths resolve against the invocation directory and artifact
   roots are searched globally. Bind paths to their declaring document.
6. **Contract / documentation:** extraction harness still assigns concrete
   registry records to FW. Align it with external-registry ownership.
7. **Conformance:** scenarios.yaml is a list of observations, not an executable
   cross-language suite. Draft ports must remain draft. Metadata stating
   verified is not independently authenticated test evidence.
8. **Product:** site/feature.go and site/content.go directly include post routes,
   blog defaults and navigation. Omitting post from a manifest cannot remove
   this code. Extract composition and separately owned slices before presets.
9. **Product:** cms/manifest.go checks names and duplicate resources, but does
   not validate the recursive field vocabulary or reject unknown JSON keys.
   Strict Codex validation must use the canonical contract and negative cases;
   do not create a second field language in FW.
10. **Product / external artifact:** admin/generation.json awaits a signed
    generation. Admin profiles cannot be certified until its real receipt and
    bundle pass scripts/promote-admin.mjs. No invented pins or signatures.
11. **Validator:** Retag currently appears as a product prose rule, without a
    selected executable gate in package.json. Pin the real validator and its
    inputs before claiming mandatory enforcement.
12. **UI adapter:** the codegen supports seven emitters, but its README explicitly
    excludes some PHP complex parts. Runtime/brick support must be recorded
    per selected artifact rather than promising universal parity.

## Bounded implementation

Update contract composition semantics and conformance observations first,
mirror schemas, then fix generic CLI behavior and add integration regressions.
Do not add new public ports, framework implementations, product ids, or pins
to either core repository. Preserve existing user changes on reconciliation.
Old locks affected by changed output plans require reviewed sync; do not
silently accept a stale plan. Old Go records need a version or an owned go.mod.

Prepare a product extraction plan with positive/negative acceptance cases.
Source products and UI repositories remain evidence until product-plan
acceptance. No deployment, publication, or edits to GoBackend/FormSet.

## Proof baseline

The existing fwyml suite passed 16 tests before changes. This proves the
existing fixtures, not a built SiteStarter or cross-language conformance.
Post-change results and remaining delivery gates follow below.

## Closeout

Implemented in the companion CLI: explicit ecosystem outputs (no default
Go identity or fake build), byte-preserving materialization and digests,
required-tool failure and fail-fast execution, absolute executable paths,
dependency-first ordering and cycle detection, order-independent conflicts,
Go dependency/script collision checks, and document-relative source roots.
Missing compiler-owned outputs now fail verification too. Dry-run rejects
invalid compositions and fetch refuses incompatible graphs.

FW adds `go.version` to the alpha registry schema; the CLI mirror matches.
Composition documents state migration from old implicit-output locks.
Extraction policy now puts concrete records in the external registry.
The executable assembly regressions live in `fwyml/test/assembly.test.mjs`;
they exercise neutral/npm/Go graphs, bytes, removal, tools and paths. They
are compiler conformance tests, not implementations of the draft port suites.

Validation:

- fwyml: `npm test` passed all 29 tests (16 baseline plus 13 new assembly
  regressions); TypeScript compilation is part of that command.
- SiteStarter: `GOTOOLCHAIN=local GOPROXY=off go test ./...` passed, including
  live backend/BFF smoke. First sandbox attempt failed writing the Go cache;
  the authorized rerun with normal cache access passed.
- UI8Kit Codegen: `bun run check` passed, 33 bricks and 63 parts.
- UI8Kit Registry: `npm run verify` passed, digest 2.0.0, 34 components,
  33 UI artifacts. No publication or full cross-runtime parity run performed.

Product extraction and its release gates are prepared in the companion
`fwyml/.project/.harness/intakes/site-conveyor/plan.md`. The plan includes
site-db, blog-db, blog-markdown and headless-db; admin requires an actual
signed generation. SiteStarter AGENTS.md requires an accepted plan before
product edits, so no source product implementation was changed.

Not complete: generated-file ownership receipts, generic installation,
authenticated conformance provenance, destination/symlink confinement,
complete constraint evaluation and the extracted external product registry.
Do not describe the current CLI as a release-ready SiteStarter conveyor.
