# Integration

None of JOONS Agent Guard, JOONS Allocator, JOONS MCP Toolkit, JOONS MCP Probe, JOONS MCP Diff, JOONS MCP Policy Gate, JOONS MCP Mock Server or JOONS MCP Contract Test Runner is bundled into or shipped with JOONS CORE today. CORE does not import them.

| State | Integration |
|---|---|
| Implemented today | Allocator has optional Agent Guard hooks. This is the only implemented library-to-library integration. The core Allocator works without Guard. |
| Planned | CORE could use Guard and Allocator internally where useful. |
| Planned | Probe could gain an optional MCP adapter. |
| Implemented today | Public, v0.1.0 Preview: Probe is an external read-only tool for inspecting MCP servers without depending on CORE. An operator could inspect CORE in the future. |
| Implemented today | Diff is an independent external snapshot and comparison tool; it does not need Probe and has no library integration with ecosystem products today. Probe can be used for server health and Diff in CI for contract drift. |
| Implemented today | Policy Gate is an independent public v0.1.0 Preview for checking one schemaVersion 1 snapshot against a versioned JSON policy. It needs neither Probe nor Diff at runtime; Diff is an easy way to make a snapshot. It does not block tool calls at runtime or determine real risk; classes are user labels. |
| Implemented today | Mock Server is an independent public v0.1.0 Preview that runs a MOCK MCP server over stdio from one schemaVersion 1 snapshot and optional JSON fixtures. It needs none of the other products at runtime; Diff is an easy way to make a snapshot. Fixture responses do not perform real actions; there are no network requests, file changes except an optional report, child processes, forwarding, or execution of snapshot or fixture content. CORE does not import or ship it. |
| Implemented today | Contract Test Runner is an independent public v0.1.0 Preview that runs behavioral contract test suites against a JOONS MCP Mock Server it starts itself. It checks the target is a JOONS MCP Mock Server before any tool call; no real tool is called. It uses the Mock Server only as its test target and has no runtime dependency on it. Real-server execution is planned with explicit opt-in and an exact per-tool allowlist. CORE does not import or ship it. |

Install the optional hooks with `pip install "joons-allocator[guard]"`. `joons_allocator.integrations.guard.GuardHooks` provides the hooks. `before_lease` can veto inside the lease transaction; `after_*` hooks run after commit.

## Combining Guard and Allocator today

Install the optional Guard extra above, then use the Allocator and GuardHooks interfaces in an application:

```python
from joons_allocator import Allocator
from joons_allocator.integrations.guard import GuardHooks
```

Configure the hooks according to the Allocator repository's documentation. The hook construction and attachment details are not specified here. The `before_lease` veto participates in the lease transaction; post-commit hooks run only after the transaction commits. Allocator remains usable without the extra.

```mermaid
sequenceDiagram
    participant Allocator as Allocator
    participant Hooks as GuardHooks
    Allocator->>Allocator: Begin lease transaction
    Allocator->>Hooks: before_lease
    Hooks-->>Allocator: Allow or veto
    alt Lease allowed
        Allocator->>Allocator: Commit lease transaction
        Allocator->>Hooks: after hooks
    else Lease vetoed
        Allocator->>Allocator: Do not lease
    end
```

## Mock Server and Contract Test Runner together

Install both from their v0.1.0 tags (`npm install -g github:dbdefault/joons-mcp-contract-runner#v0.1.0` and `npm install -g github:dbdefault/joons-mcp-mock-server#v0.1.0`), make a snapshot with JOONS MCP Diff, write Mock Server fixtures and a test suite, then run:

```sh
joons-mcp-contract-test run shop.suite.json --snapshot snapshot.json --fixtures fixtures.json
```

The runner starts the Mock Server itself, checks that it is a JOONS MCP Mock Server before any tool call, runs every test and reports PASS or FAIL. No real tool is called.

## Planned adapters and connections

CORE could use Guard and Allocator internally where useful. Probe could gain an optional MCP adapter. The public v0.1.0 Preview is an external diagnostic tool that can inspect MCP servers, including CORE in the future when operated by its operator, without depending on CORE. These possibilities do not represent a present CORE integration. Probe's later ideas also include opt-in, allow-listed tool-call checks, resources and prompts inspection, and CI-friendly baselines and diffing.

[Back to README](../README.md)
