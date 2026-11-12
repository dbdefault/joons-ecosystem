# Roadmap

- [x] Phase 1 - Brand and portfolio: this ecosystem repository; Agent Guard 0.1.0 and Allocator 0.1.0 previews.
- [ ] Phase 2 - MCP developer tools: MCP Probe 0.1.0 Preview is in progress. Later ideas include opt-in, allow-listed tool-call checks; resources and prompts inspection; CI-friendly baselines and diffing; and an optional MCP adapter.
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
