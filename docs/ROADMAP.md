# Roadmap

- [x] Phase 1 - Brand and portfolio: this ecosystem repository; Agent Guard 0.1.0 and Allocator 0.1.0 previews.
- [x] Phase 2 - MCP developer tools: MCP Probe 0.1.0 Preview, MCP Diff 0.1.0 Preview, MCP Policy Gate 0.1.0 Preview, MCP Mock Server 0.1.0 Preview and MCP Contract Test Runner 0.1.0 Preview. Later Probe ideas include opt-in, allow-listed tool-call checks; resources and prompts inspection; CI-friendly baselines and diffing; and an optional MCP adapter. Diff separately provides snapshot comparison and CI thresholds today. Policy Gate independently checks one snapshot against a written policy. Mock Server independently runs a mock server from one snapshot. Contract Test Runner runs behavioral contract tests against that mock; real-server execution with explicit opt-in and an exact per-tool allowlist is planned.
- [ ] Phase 3 - Independent MCP extension modules: extract general-purpose tools that CORE needs as standalone MCP packages.
- [ ] Phase 4 - Ecosystem integration: connect Guard, Allocator and Probe to CORE where useful.

## Planned library next steps

- [ ] Allocator lease expiry and timeouts, worker capacity greater than one, and scheduled tasks.
- [ ] Guard and Allocator 1.0 after the API stabilises.

## Principles for what ships next

- Keep products independent and composable, including without CORE.
- Label work in progress and future integrations honestly.
- Keep diagnostics read-only in Probe 0.1.0; tool-call checks are a later opt-in idea.
- Stabilise library APIs before Guard and Allocator 1.0.
- Extract general-purpose tools as standalone MCP packages and connect products to CORE where useful.

[Back to README](../README.md)
