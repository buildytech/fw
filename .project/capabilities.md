# Capabilities

A capability is a port: DTOs, operations, events, invariants, and a
conformance suite. A capability is not a package, a feature flag, or a UI
panel.

Rule: a capability is published only when its conformance suite exists.

## Catalog

| Id | Owns | Typical adapters | Status |
| --- | --- | --- | --- |
| `workspace` | folder as source of truth, read/write, revisions, search | `fs-local` | planned, M1 |
| `ui` | primitives and screens | `ui-solid`, `ui-react`, `ui-latte`, `ui-templ` | planned, M1 |
| `delivery` | packaged artifact and its layout | `portable-windows`, `installer`, `image` | planned, M1 |
| `host` | window, process, IPC | `host-wails-go`, `host-tauri-rust`, `host-cli` | planned, M2 |
| `editor` | text buffer, syntax, selection | `editor-codemirror` | planned, M2 |
| `explorer` | tree of the workspace | `explorer-default` | planned, M2 |
| `vcs` | history, staging, diff | `vcs-git-cli` | planned, M4 |
| `agent` | closed event bus for assisted work | `agent-cursor-node` | planned, M4 |
| `browse` | second native view plus debug protocol | `browse-webview2` | planned, M4 |
| `pack` | neighbouring processes and their lifecycle | `pack-nginx-mariadb` | planned, M4 |
| `terminal` | interactive TTY | `terminal-conpty` | backlog |
| `languages` | diagnostics, format, definitions | `languages-lsp` | backlog |
| `policy` | permissions for mutating operations | — | backlog |
| `secrets` | credential storage and redaction | — | backlog |
| `telemetry` | traces and diagnostics, opt-in | — | backlog |

## Contract obligations per capability

Each port document must state all six items. A missing item blocks
publication.

1. **DTOs** — data crossing the boundary, with explicit optionality.
2. **Operations** — request, response, and failure modes.
3. **Events** — what the adapter may emit and when.
4. **Invariants** — what remains true regardless of implementation.
5. **Non-obligations** — what the port explicitly refuses to promise.
6. **Conformance** — scenarios and observable expectations.

## Notes carried from practice

`workspace` is the anchor. Files in the opened folder are authoritative;
revision checks precede writes; a product must not add a second content
API. Most other capabilities resolve their paths through it.

`ui` delegates to a generator rather than defining components. The proven
model is one definition per primitive, one printer per runtime, and parity
tests over a canonical DOM. A product selects one UI runtime; the core
knows several, the product knows one.

`agent` is a bus, not an SDK. The event set is closed. Bridges are
adapters, at most one live per session, and the presentation layer never
imports a vendor client.

`browse` is a second native view plus a debug protocol. Contract-level
honesty matters here: an embedded render host is not a full browser, and
device emulation is unavailable in that context. The port must document
the absence instead of simulating it.

`pack` covers neighbouring executables such as a web server and a database
placed beside the product. It owns start, stop, health, ports, and logs. No
vertical is named in the contract; a WordPress theme studio is a product
that selects a `pack` adapter, not a branch inside the core.

`delivery` owns the on-disk layout of a shipped product, including the
portable case where state lives beside the executable rather than in a
per-user profile.

## Adding a capability

1. Write the port document with all six obligations.
2. Write the conformance suite before any adapter.
3. Implement two adapters in different languages.
4. Prove a product can omit the capability entirely.
5. Register the port in the manifest schema and the CLI.

Step four is the acceptance test for modularity. If omission is not
possible, the port is wrongly scoped.
