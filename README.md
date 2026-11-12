# JOONS

MCP-Based AI Infrastructure Ecosystem

JOONS is an MCP-centered AI infrastructure ecosystem that connects AI agents with tools, computers, knowledge, and reliable execution systems.

![CORE Private](https://img.shields.io/badge/JOONS%20CORE-Private-lightgrey?labelColor=4F46E5)
![Agent Guard Preview](https://img.shields.io/badge/Agent%20Guard-Preview-orange?labelColor=4F46E5)
![Allocator Preview](https://img.shields.io/badge/Allocator-Preview-orange?labelColor=4F46E5)
![MCP Toolkit Public](https://img.shields.io/badge/MCP%20Toolkit-Public-brightgreen?labelColor=4F46E5)
![MCP Probe Preview](https://img.shields.io/badge/MCP%20Probe-Preview-orange?labelColor=4F46E5)
![MCP Diff Preview](https://img.shields.io/badge/MCP%20Diff-Preview-orange?labelColor=4F46E5)
![MCP Policy Gate Preview](https://img.shields.io/badge/MCP%20Policy%20Gate-Preview-orange?labelColor=4F46E5)

[한국어](README.ko.md)

## Why JOONS

Connecting an agent to tools is only one part of an MCP system. Developers also need reliable execution, task allocation, diagnostics, and tool connectivity. JOONS brings these concerns together around MCP: JOONS CORE is the central server, while libraries and tools complement it or stand independently.

## Who it is for

- Developers connecting AI agents to tools and data through MCP.
- Python developers looking for independent reliability or allocation libraries.
- MCP developers exploring reference patterns or protocol diagnostics.
- Operators who need visibility into product status and boundaries.

## Ecosystem map

```mermaid
flowchart TD
    Agent["AI agents"] --> CORE["JOONS CORE MCP server"]
    CORE --> External["External tools and data sources"]
    Allocator["JOONS Allocator"] -. "optional hooks" .-> Guard["JOONS Agent Guard"]
    Toolkit["JOONS MCP Toolkit"]
    Probe["JOONS MCP Probe Preview"]
    Diff["JOONS MCP Diff Preview"]
    PolicyGate["JOONS MCP Policy Gate Preview"]
    classDef private fill:#d3d3d3,stroke:#4F46E5,color:#000
    classDef public fill:#90ee90,stroke:#4F46E5,color:#000
    classDef preview fill:#ffa500,stroke:#4F46E5,color:#000
    classDef planned fill:#add8e6,stroke:#4F46E5,color:#000
    class CORE private
    class Toolkit public
    class Guard,Allocator preview
    class Probe,Diff,PolicyGate preview
```

This conceptual map does not imply that independent products are embedded in CORE. Allocator's optional Guard hooks are the only implemented library-to-library integration.

| Product | Core question | Role | Status | Link |
|---|---|---|---|---|
| JOONS CORE | What can the AI do? | MCP server and tool execution | Private, in operation; source not published | None |
| JOONS Agent Guard | Does it run safely? | Reliability and protection | Public, v0.1.0 Preview | [Repository](https://github.com/dbdefault/joons-agent-guard) |
| JOONS Allocator | Who does which job? | Task allocation | Public, v0.1.0 Preview | [Repository](https://github.com/dbdefault/joons-allocator) |
| JOONS MCP Toolkit | How should we build it? | Development reference | Public reference implementation, early | [Repository](https://github.com/dbdefault/joons-mcp-toolkit) |
| JOONS MCP Probe | Does the MCP server work correctly? | Diagnostics and validation | Public, v0.1.0 Preview | [Repository](https://github.com/dbdefault/joons-mcp-probe) |
| JOONS MCP Diff | What changed, and will it break my agents? | MCP Development Tools: contract comparison | Public, v0.1.0 Preview | [Repository](https://github.com/dbdefault/joons-mcp-diff) |
| JOONS MCP Policy Gate | Does this tool list follow our rules? | MCP Development Tools: contract policy checks | Public, v0.1.0 Preview | [Repository](https://github.com/dbdefault/joons-mcp-policy-gate) |

## What you can do today

### Install JOONS Agent Guard

Use the public preview for durable exclusive locks, a circuit breaker, and at-most-once handling of retried requests. It uses Python 3.9+ and SQLite. Its locks are advisory and do not expire on their own; its scope is one machine.

```sh
pip install git+https://github.com/dbdefault/joons-agent-guard@v0.1.0
```

### Install JOONS Allocator

Use the public preview for idempotent task submission, priority and FIFO leasing, exclusive named resources, and an append-only audit. It uses Python 3.9-3.13 and SQLite. It operates on one machine and has no lease expiry today.

```sh
pip install git+https://github.com/dbdefault/joons-allocator@v0.1.0
```

Allocator works without Guard. Optional Guard hooks are described in [Integration](docs/INTEGRATION.md). Consult each product repository for contribution and security guidance.

Allocator also offers:

- One running task per worker and lease-token fencing.
- Approval, cancellation, pause, resume, retry and recovery operations.
- A worker registry with heartbeat, an asyncio facade, and an operator CLI.

Agent Guard also offers:

- A per-actor single run slot through CircuitBreaker.
- Asyncio adapters and an operator CLI.

Both libraries use the Python standard library and SQLite.
Review their single-machine limitations before choosing a design.
Guard's locks never expire on their own; Allocator has no lease expiry.
Planned lease expiry and timeouts are not current features.

### Explore JOONS MCP Toolkit

The early public reference implementation presents patterns for request coordination, host routing, hybrid retrieval, and a durable request ledger. It is an educational reference, not production JOONS CORE.

### Probe an MCP server

Install the public preview from GitHub (not npm):

```sh
npm install -g github:dbdefault/joons-mcp-probe#v0.1.0
```

Probe inspects MCP servers over stdio or Streamable HTTP without calling tools. See the [repository](https://github.com/dbdefault/joons-mcp-probe) for CLI usage.

### Compare MCP tool contracts

JOONS MCP Diff: Detect MCP tool and schema changes before they break your AI agents. Install the [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-diff/releases/tag/v0.1.0):

```sh
npm install -g github:dbdefault/joons-mcp-diff#v0.1.0
```

Diff captures read-only `tools/list` snapshots over stdio or Streamable HTTP as versioned canonical JSON, then compares two points in time. Snapshots never store commands, paths, URLs, headers, environment values or tokens. Changes are BREAKING, NON_BREAKING or UNKNOWN; unprovable changes are UNKNOWN and rename hints are suggestions only. It provides terminal, Markdown and JSON reports, `--fail-on` for CI, and exit codes 0–4. It is Apache-2.0, uses JavaScript ESM, Node.js 20+ and the official MCP SDK.

Probe inspects one MCP server's current protocol behaviour and tool contract; Diff compares the tool contract at two points in time. They are independent and complementary: Diff does not need Probe. For example, run Probe for health and Diff in CI for contract drift.

### Check a tool list against a policy

JOONS MCP Policy Gate: Static policy checks for MCP tool contracts, built for CI. Install the [v0.1.0 Preview](https://github.com/dbdefault/joons-mcp-policy-gate/releases/tag/v0.1.0):

```sh
npm install -g github:dbdefault/joons-mcp-policy-gate#v0.1.0
```

Policy Gate reads a `joons-mcp-diff` schemaVersion 1 snapshot and a versioned JSON policy. It checks allow and deny name globs, required tools, maximum tool count, descriptions, input schemas, required annotation keys, and user-defined risk classes with dated approvals. Deny wins and its level cannot be lowered by an allow violation. Strict validation rejects unknown keys, conflicts and policies that can never produce a finding. It provides PASS/WARN/FAIL, terminal, JSON and Markdown reports, `--fail-on failure|warning|none`, exit codes 0-4, and a `validate-policy` command. It uses JavaScript ESM, Node.js 20+, zero runtime dependencies and Apache-2.0; it is not on npm.

Probe inspects one server's current behaviour; Diff compares two points in time; Policy Gate checks one snapshot against a written policy. Policy Gate is installed and run independently and needs neither Probe nor Diff at runtime; Diff is an easy way to make a snapshot. This static check does not block tool calls at runtime or determine real risk: risk classes are user labels.

## Choosing a product

1. Need the private MCP server and its tool execution? Consider JOONS CORE.
2. Need reliability primitives for a single machine? Start with JOONS Agent Guard.
3. Need to allocate tasks to workers? Start with JOONS Allocator.
4. Need MCP implementation patterns to study? Explore JOONS MCP Toolkit.
5. Need protocol and tool-contract diagnostics? Use JOONS MCP Probe.
6. Need to detect contract drift and compatibility risks? Use JOONS MCP Diff.
7. Need to check whether a tool list follows your rules? Use JOONS MCP Policy Gate.

The libraries can be used without CORE. See the [project comparison](docs/PROJECTS.md) for scope and limitations.

## Design principles

- MCP-centered: organize tool connectivity around MCP.
- Independent and composable: libraries can be used without CORE.
- Read-only and least-surprise defaults: Probe is designed to inspect without calling tools.
- Durable, auditable state: durable locks and ledgers are available where described; Allocator records an append-only audit.
- Honest status labels: distinguish public previews, private operation, and planned work.
- Privacy by default: keep public and private boundaries explicit.

## FAQ

### Is JOONS CORE open source?

No. JOONS CORE is private, in operation, and its source is not published.

### Do I need CORE to use the libraries?

No. JOONS Agent Guard and JOONS Allocator are independent libraries. Allocator's Guard hooks are optional.

### Are the public products bundled into CORE?

No. CORE does not import or ship Guard, Allocator, Toolkit, Probe, Diff, or Policy Gate today. Future integration is a plan.

### Is MCP Toolkit the production CORE server?

No. It is an early public reference implementation. It does not expose or require CORE.

### Can MCP Probe call tools?

The v0.1.0 Preview is read-only and does not call tools. Opt-in, allow-listed tool-call checks are a later idea.

## Contributing

This repository accepts documentation fixes. Send product issues and pull requests to the relevant product repository and consult its CONTRIBUTING.md and SECURITY.md. Use GitHub private vulnerability reporting on the affected repository for security reports.

## Documentation

- [Architecture](docs/ARCHITECTURE.md)
- [Projects](docs/PROJECTS.md)
- [Integration](docs/INTEGRATION.md)
- [Roadmap](docs/ROADMAP.md)
- [Brand](docs/BRAND.md)
- [Korean README](README.ko.md)
- [License](LICENSE)
