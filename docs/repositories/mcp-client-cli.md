# mcp-client-cli

- Upstream: https://github.com/thedotmack/mcp-client-cli
- Snapshot: main @ a9e7795a1dd14a21040cc260557cc20a91927edc
- Tree: 15 blobs; 41,429 bytes; heuristic test paths: 0
- Metadata license: MIT; root LICENSE present
- Package: mcp-client-cli 0.1.0; Node >=18; ESM
- Status: L0 PASS; L1 PARTIAL; L2 IN PROGRESS; L3/L4 NOT RUN

## Product category
Human-facing CLI client for MCP servers. README describes dynamic discovery of server tools via tools/list and generation of Commander.js commands/options from tool input schemas.

## Tree map
- src/ (5): client, CLI generator, config loader, protocol types and entry code
- examples/ (2): sample server configurations
- bin/ (1): executable entry
- root: README.md, SUMMARY.md, package.json, package-lock.json, tsconfig.json, LICENSE

## Architecture from README/SUMMARY (documented claim)
User CLI command → server process over stdio → MCP initialize/JSON-RPC handshake → tools/list → schema-to-Commander mapping → dynamic help/options → tools/call → formatted output. Named server configuration is loaded from local config paths; tool schemas are discovered dynamically rather than duplicated in config.

## Important discrepancy
package.json declares a test script using Node's test runner, but the tree heuristic found zero test-named files. This means test script exists, not that test coverage exists or that no tests can be invoked. Inspect test discovery behavior and npm test result before concluding.

## Quality/provenance notes
package.json repository URL still contains YOUR_USERNAME placeholders. SUMMARY.md calls it a working prototype and documents a Chroma MCP demo with 13 tools; that is historical project documentation, not a current live test result.

## Security questions
Audit command/argument parsing, environment-variable injection, server process spawning, config file permissions, secret exposure, JSON schema edge cases, output serialization and untrusted MCP server responses.

## Next source targets
src/client/MCPClient.ts; src/cli/generator.ts; src/cli/index.ts; src/config/loader.ts; src/types/mcp.ts; bin/mcp-cli.js; examples/.


## Source-level findings (confirmed by code inspection)
- src/client/MCPClient.ts starts a child process via Node child_process.spawn, tracks numeric/string request IDs and pending promises, parses a stream buffer and maintains initialization/capability state.
- src/cli/generator.ts maps JSON Schema types to Commander flags: integer/number → number, array/object → JSON, boolean → boolean, otherwise string; required fields become requiredOption and optional fields become optional options.
- src/config/loader.ts searches project-local .mcp-cli.json, home .mcp-cli.json and ~/.config/mcp-cli/config.json; config defines process launch command/args/env, while tool schemas are discovered at runtime.
- Security review must inspect spawn options, stdio framing, pending request timeout/cleanup, malformed JSON, server crash/restart, env inheritance, and JSON schema constructs not covered by the simple generator.
- package.json declares node --test, but no test-named paths exist in the snapshot; no test execution evidence.

## Core flow (source-grounded)
Load named server config → spawn configured command with args/env → MCP initialize handshake → tools/list → generate Commander command options → tools/call → serialize/formatted result → graceful process shutdown.
