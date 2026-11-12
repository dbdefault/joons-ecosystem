# Projects

## Comparison

| Product | Language | Runtime | Storage | License | Status |
|---|---|---|---|---|---|
| JOONS CORE | Not specified | Not specified | Not specified | Not specified | Private, in operation; source not published |
| JOONS Agent Guard | Python | 3.9+ | SQLite, one file | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS Allocator | Python | 3.9-3.13 | SQLite | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Toolkit | JavaScript | Not specified | In-memory coordinator; durable request ledger | MIT | Public reference implementation, early |
| JOONS MCP Probe | JavaScript ESM | Node.js 20+ | Not specified | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Diff | JavaScript ESM | Node.js 20+ | Versioned JSON snapshots | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Policy Gate | JavaScript ESM | Node.js 20+ | Versioned JSON policy and snapshot inputs | Apache-2.0 | Public, v0.1.0 Preview |

## JOONS CORE

- Status: Private, in operation; source not published.
- Version, license, language, repository: Not specified; source is not published.
- Capabilities: MCP protocol endpoint, tool registry and transports; connects agents to computers and phones, files, knowledge, web and Git; selects target machines across several machines; can connect external MCP servers.
- Limitations: Source is private. None of the other products is bundled into or imported by CORE today.

## JOONS Agent Guard

- Status: Public, v0.1.0 Preview. Version: 0.1.0. License: Apache-2.0. Language: Python 3.9+, standard library and SQLite. [Repository](https://github.com/dbdefault/joons-agent-guard).
- Capabilities: ResourceLock provides durable exclusive locks; CircuitBreaker provides a per-actor single run slot with closed, open and half_open states; ReplayGuard handles retried requests at most once. Asyncio adapters and an operator CLI are available.
- Limitations: Single machine, one SQLite file. Locks never expire on their own and are advisory.

## JOONS Allocator

- Status: Public, v0.1.0 Preview. Version: 0.1.0. License: Apache-2.0. Language: Python 3.9-3.13, standard library and SQLite. [Repository](https://github.com/dbdefault/joons-allocator).
- Capabilities: Idempotent task submission; priority and FIFO leasing; one running task per worker; exclusive named resources; lease-token fencing; approval, cancel, pause, resume, retry and recover; append-only audit; worker registry with heartbeat; asyncio facade and operator CLI. Agent Guard hooks are optional.
- Limitations: Single machine; no lease expiry.

## JOONS MCP Toolkit

- Status: Public reference implementation, early. Version: Not specified. License: MIT. Language: JavaScript. [Repository](https://github.com/dbdefault/joons-mcp-toolkit).
- Capabilities: Reference patterns for an in-memory idempotent request coordinator, host routing, hybrid retrieval with optional reranking, and a durable request ledger.
- Limitations: An educational reference, not production JOONS CORE; it neither exposes nor requires CORE. The request coordinator is in-memory.

## JOONS MCP Probe

- Status: Public, v0.1.0 Preview. Version: 0.1.0. License: Apache-2.0. Language: JavaScript ESM; runtime: Node.js 20+; depends on the official MCP SDK. [Repository](https://github.com/dbdefault/joons-mcp-probe).
- Capabilities: Inspects MCP servers over stdio or Streamable HTTP; checks initialization, protocol information, tool names, duplicates, descriptions, inputSchema and outputSchema via JSON Schema compilation, and pagination. It records timings and failure classifications, produces JSON and Markdown reports with PASS/WARNING/FAIL, redacts secrets, bounds time, size and tool count, and cleans up child-process trees.
- Failure kinds: `spawn_failed`, `connection_refused`, `handshake_failed`, `protocol_mismatch`, `invalid_response`, `timeout`, `process_exited`, `auth_failed`.
- CLI exit codes: 0 pass, 1 warning with `--fail-on warning`, 2 fail, 3 usage error, 4 output error.
- Limitations: Read-only; v0.1.0 sends only `initialize`, `notifications/initialized`, `tools/list` and `ping`, and does not call tools. It focuses on protocol behaviour and tool contracts rather than host monitoring or tool-list browsing. Install from GitHub; it is not on npm.

## JOONS MCP Diff

- Category: MCP Development Tools. Core question: What changed, and will it break my agents?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-diff/releases/tag/v0.1.0). Apache-2.0, JavaScript ESM, Node.js 20+, official MCP SDK. [Repository](https://github.com/dbdefault/joons-mcp-diff).
- Capabilities: Read-only `tools/list` snapshots over stdio or Streamable HTTP, saved as versioned canonical JSON; compare snapshots with BREAKING, NON_BREAKING and UNKNOWN classifications, conservative UNKNOWN for unprovable changes, and rename hints only. Terminal, Markdown and JSON reports, `--fail-on` for CI and exit codes 0–4 are available. Snapshots never store commands, paths, URLs, headers, environment values or tokens.
- Install: `npm install -g github:dbdefault/joons-mcp-diff#v0.1.0`.
- Boundary: Probe inspects one server's current protocol behaviour and tool contract. Diff compares the tool contract at two points in time; it does not need Probe. Use Probe for health and Diff in CI for contract drift. Diff is not bundled into CORE and has no library integration with the other ecosystem products today.

## JOONS MCP Policy Gate

- Category: MCP Development Tools: contract policy checks. Core question: Does this tool list follow our rules?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-policy-gate/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, zero runtime dependencies. [Repository](https://github.com/dbdefault/joons-mcp-policy-gate).
- Capabilities: Static policy checks for MCP tool contracts, built for CI. Reads a `joons-mcp-diff` schemaVersion 1 snapshot and a versioned JSON policy. Checks allow/deny name globs, required tools, max tool count, description and input-schema requirements, required annotation keys, and user-defined risk classes with dated approvals. Deny wins and cannot be lowered. Strict validation rejects unknown keys, conflicts and policies that can never produce a finding. Outputs PASS/WARN/FAIL in terminal, JSON and Markdown, offers `--fail-on failure|warning|none`, exit codes 0-4 and `validate-policy`.
- Install: `npm install -g github:dbdefault/joons-mcp-policy-gate#v0.1.0`.
- Boundary: Probe inspects one server's current behaviour; Diff compares two points in time; Policy Gate checks one snapshot against a written policy. Policy Gate is installed and run independently, needs neither Probe nor Diff at runtime, and Diff is an easy way to make a snapshot. It does not block tool calls at runtime or determine real risk; risk classes are user labels. CORE does not import or ship it.

[Back to README](../README.md)
