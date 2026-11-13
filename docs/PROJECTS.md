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
| JOONS MCP Mock Server | JavaScript ESM | Node.js 20+ | Snapshot and optional JSON fixture inputs; in-memory call recording and optional JSON report | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Contract Test Runner | JavaScript ESM | Node.js 20+ | JSON test suites; terminal, JSON and Markdown reports | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Report Hub | JavaScript ESM | Node.js 20+ | JSON reports in; summary, offline HTML dashboard and SVG badges out | Apache-2.0 | Public, v0.1.0 Preview |

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

## JOONS MCP Mock Server

- Category: MCP Development Tools: mock server. Core question: Can I test my agent against this server without touching anything real?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-mock-server/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, official MCP SDK. [Repository](https://github.com/dbdefault/joons-mcp-mock-server).
- Capabilities: Create realistic mock MCP servers from tool snapshots for safe agent development and testing. Reads a `joons-mcp-diff` schemaVersion 1 snapshot and optional JSON fixtures; serves a MOCK MCP server over stdio with initialize, ping, tools/list and tools/call. Fixture results support defaults, argument matches and named scenarios; built-in scenarios are default, empty, error, slow and server-error. Includes `--fail-tool`, JSON Schema subset argument validation, in-memory call recording with masking of secret-looking arguments and an optional JSON report. Streamable HTTP, resources and prompts are not in 0.1.0.
- Install: `npm install -g github:dbdefault/joons-mcp-mock-server#v0.1.0`.
- Boundary: This is a MOCK; it performs no real actions, network requests, file changes except the optional report, child processes or forwarding, and executes nothing from snapshots or fixtures. Probe diagnoses a real server; Diff compares two snapshots; Policy Gate checks one snapshot against a policy; Mock Server runs a mock server from one snapshot. It is installed and run independently, needs none of the others at runtime, and Diff is an easy way to make a snapshot. CORE does not import or ship it.

## JOONS MCP Contract Test Runner

- Category: MCP Development Tools: contract testing. Core question: Do my tools still behave the way my agents expect?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-contract-runner/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, official MCP SDK. [Repository](https://github.com/dbdefault/joons-mcp-contract-runner).
- Capabilities: Define, execute, and validate behavioral contracts for MCP tools. Strictly validated JSON test suites with assertions for outcome, isError, text, contentCount, structuredContent (equals or subset match), fields, maxDurationMs, errorCode and errorMessageContains; captures between tests; per-test and total timeouts; concurrency; PASS/FAIL/SKIP; terminal, JSON and Markdown reports; CI exit codes; captured values and secret-looking keys redacted in reports.
- Install: `npm install -g github:dbdefault/joons-mcp-contract-runner#v0.1.0` and the Mock Server `npm install -g github:dbdefault/joons-mcp-mock-server#v0.1.0`.
- Boundary: 0.1.0 runs only against a JOONS MCP Mock Server, identity-checked before any tool call, so no real tool is called; real-server execution is planned with explicit opt-in and an exact per-tool allowlist. Probe diagnoses a real server; Diff compares two snapshots; Policy Gate checks one snapshot against a policy; Mock Server runs a mock server; Contract Test Runner runs behavioral contract tests against the mock. It is installed and run independently and uses the Mock Server only as its test target. CORE does not import or ship it.

## JOONS MCP Report Hub

- Category: MCP Development Tools: report aggregation. Core question: Where can I see all my MCP reports in one place?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-report-hub/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, zero runtime dependencies. [Repository](https://github.com/dbdefault/joons-mcp-report-hub).
- Capabilities: Reads the JSON reports of Probe, Diff, Policy Gate, Mock Server and Contract Test Runner and produces one normalized summary, a self-contained offline HTML dashboard and SVG badges. Report content is treated as untrusted input; default output is an allowlist and drops tool names, server names, messages, paths, URLs and error text.
- Install: `npm install -g github:dbdefault/joons-mcp-report-hub#v0.1.0`
- Boundary: It only reads JSON files you point it at and writes its own output files. It never connects to an MCP server, never runs a tool or program, and makes no network request. It needs none of the other products installed. CORE does not import or ship it.
[Back to README](../README.md)
