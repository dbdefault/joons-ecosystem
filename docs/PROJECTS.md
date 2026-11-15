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
| JOONS MCP Schema Toolkit | JavaScript ESM | Node.js 20+ | Offline CLI | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Tool Catalog | JavaScript ESM | Node.js 20+ | Offline CLI | Apache-2.0 | Public, v0.1.1 Preview |
| JOONS MCP Test Data Generator | JavaScript ESM | Node.js 20+ | Offline CLI | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Context Budget | JavaScript ESM | Node.js 20+ | Offline CLI | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS Search Engine | JavaScript ESM | Node.js 20+ | Offline CLI and library | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Audit Trail | JavaScript ESM | Node.js 20+ | Offline CLI and library | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS MCP Config Doctor | JavaScript ESM | Node.js 20+ | Offline CLI and library | Apache-2.0 | Public, v0.1.0 Preview |
| JOONS Listen MCP | Python | Python 3.10+, NVIDIA GPU | Local service, CLI and stdio MCP server | Apache-2.0 | Public, v0.1.0 Preview |

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
## JOONS MCP Test Data Generator

- Category: MCP Development Tools: test data generation. Core question: How do I get valid and invalid test inputs for my MCP tools?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-test-data-generator/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+. [Repository](https://github.com/dbdefault/joons-mcp-test-data-generator).
- Capabilities: Deterministic, offline generation of schema-valid and invalid test inputs (fixtures, suite and dataset outputs) from a tool snapshot; refuses credential-like tool and property names.
- Install: `npm install -g github:dbdefault/joons-mcp-test-data-generator#v0.1.0`
- Boundary: Works offline on files you point it at and writes only its own output files. CORE does not import or ship it.

## JOONS MCP Schema Toolkit

- Category: MCP Development Tools: schema inspection. Core question: Are my MCP tool schemas well formed, and what do they accept?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-schema-toolkit/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+. [Repository](https://github.com/dbdefault/joons-mcp-schema-toolkit).
- Capabilities: Offline inspection of a supported subset of MCP tool schemas, with example and fixture generation; refuses credential-like identifiers.
- Install: `npm install -g github:dbdefault/joons-mcp-schema-toolkit#v0.1.0`
- Boundary: Works offline on files you point it at and writes only its own output files. CORE does not import or ship it.

## JOONS MCP Tool Catalog

- Category: MCP Development Tools: tool catalog. Core question: Which tools do my MCP servers expose together, and do any names collide?
- Status: Public, [v0.1.1 Preview](https://github.com/dbdefault/joons-mcp-tool-catalog/releases/tag/v0.1.1), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, zero runtime dependencies. [Repository](https://github.com/dbdefault/joons-mcp-tool-catalog).
- Capabilities: Offline command-line tool that merges several MCP tool-list snapshots into one normalized catalog, finds exact and weak name collisions across servers, searches tool names and descriptions with a deterministic ranking, and writes deterministic JSON, Markdown and terminal reports. Sanitizing of free text is best-effort pattern matching, not an anonymization guarantee.
- Install: `npm install -g github:dbdefault/joons-mcp-tool-catalog#v0.1.1`
- Boundary: Command-line tool only; the JavaScript modules are internal and not a supported library. It only reads the snapshot files you point it at and writes its own output files; it never connects to an MCP server, never runs a tool or program, and makes no network request. CORE does not import or ship it.

## JOONS MCP Context Budget

- Category: MCP Development Tools: context budgeting. Core question: How much context do my MCP tool definitions take up?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-context-budget/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, zero runtime dependencies. [Repository](https://github.com/dbdefault/joons-mcp-context-budget).
- Capabilities: Offline command-line tool that reads MCP tool-list snapshots, estimates the context size of the tool definitions, ranks the largest tools, checks the estimate against a budget, and writes JSON and Markdown reports. Estimates are based on UTF-8 bytes and a configurable formula, not tokenizer counts.
- Install: `npm install -g github:dbdefault/joons-mcp-context-budget#v0.1.0`
- Boundary: Command-line tool only; the JavaScript modules are internal and not a supported library. It only reads the snapshot files you point it at and writes its own output files; it never connects to an MCP server and makes no network request. CORE does not import or ship it.

## JOONS Search Engine

- Category: Standalone tool: local document search. Core question: How do I index my local documents offline and search them with explainable ranking?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-search-engine/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, zero runtime dependencies. [Repository](https://github.com/dbdefault/joons-search-engine).
- Capabilities: Offline command-line tool and library that index Markdown, TXT, JSON and CSV files, search them with multilingual tokenization (Korean, Chinese, Japanese, English), user synonym dictionaries and configurable path and metadata rules, return deterministic results with a score explanation, and measure retrieval quality with a built-in evaluation toolkit and public synthetic datasets.
- Install: `npm install -g github:dbdefault/joons-search-engine#v0.1.0`
- Boundary: A standalone product. It only reads the folders you point it at and writes its own index files; it makes no network request and does not depend on CORE. CORE does not import or ship it; an adapter interface is documented for future optional use.

## JOONS MCP Audit Trail

- Category: MCP Development Tools: tamper-evident audit log. Core question: How do I show that the record of my AI agent and MCP tool activity was not altered afterwards?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-audit-trail/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, zero runtime dependencies. [Repository](https://github.com/dbdefault/joons-mcp-audit-trail).
- Capabilities: Offline command-line tool and library that append events to a JSON-Lines log as a hash chain (SHA-256, or HMAC-SHA256 with a key file), verify the chain and report edits, deletions, reordering and insertions, detect removal of the newest records against an anchor file, and write show, summary and export reports (JSON, CSV, Markdown). It detects modification; it does not prevent it.
- Install: `npm install -g github:dbdefault/joons-mcp-audit-trail#v0.1.0`
- Boundary: A standalone product. It only reads and writes the log, key and anchor files you point it at; it makes no network request and does not depend on CORE. CORE does not import or ship it.

## JOONS MCP Config Doctor

- Category: MCP Development Tools: config linting and conversion. Core question: Is my MCP client config file correct, safe and portable?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-config-doctor/releases/tag/v0.1.0), not on npm. Apache-2.0, JavaScript ESM, Node.js 20+, zero runtime dependencies. [Repository](https://github.com/dbdefault/joons-mcp-config-doctor).
- Capabilities: Offline command-line tool and library that read MCP client config files (Claude Desktop, Claude Code, Cursor, VS Code), report problems (missing commands, unpinned packages, shell wrappers, insecure URLs, secrets by credential shape and key name), list servers without revealing values, convert between the four formats with explicit loss warnings, and write a redacted copy that is safe to share. It never starts a server or opens a network connection.
- Install: `npm install -g github:dbdefault/joons-mcp-config-doctor#v0.1.0`
- Boundary: A standalone product. It only reads and writes the config files you point it at; it makes no network request and does not depend on CORE. CORE does not import or ship it. Credential detection is a heuristic list, not a guarantee.

[Back to README](../README.md)

## JOONS Listen MCP

- Category: Standalone tool: local meeting transcription. Core question: How do I transcribe meeting recordings locally, with speaker turns and flagged numbers, and drive it from an MCP client?
- Status: Public, [v0.1.0 Preview](https://github.com/dbdefault/joons-listen-mcp/releases/tag/v0.1.0), not on PyPI. Apache-2.0, Python 3.10+, needs an NVIDIA GPU of about 8 GB for the speech models. [Repository](https://github.com/dbdefault/joons-listen-mcp).
- Capabilities: A loopback-only service and a stdio MCP server (six tools) that turn a recording into a transcript with speaker turns, timestamps and "verify" flags on numbers, prices, dates and codes, using Whisper large-v3 and Qwen3-ASR-1.7B, plus optional Korean translation and minutes through a local LLM. Meeting audio and text stay on the machine.
- Install: `pip install "joons-listen-mcp[gpu] @ git+https://github.com/dbdefault/joons-listen-mcp@v0.1.0"` (the `gpu` extra pulls the speech packages)
- Boundary: A standalone product. It reads audio only from folders you allow and sends text only to a local LLM. JOONS CORE has a separate private adapter that talks to the same local HTTP API; CORE does not import or ship this repository. Known weaknesses (far-field audio, numbers, speaker count) are listed in its LIMITATIONS.md.
