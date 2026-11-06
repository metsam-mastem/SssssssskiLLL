# lossless-claw

- Upstream: https://github.com/thedotmack/lossless-claw
- Parent/source: https://github.com/Martian-Engineering/lossless-claw
- Snapshot: main @ 4fdb53ca9949faf1e7ffaa6abadca7f8c9921130
- Tree: 105 blobs; 1,761,693 bytes; heuristic test paths: 20
- Metadata license: MIT; root LICENSE present
- Fork: true
- Status: L0 PASS; L1 PARTIAL; L2/L3 IN PROGRESS; L4 NOT RUN

## Product category
OpenClaw plugin for Lossless Context Management (LCM). README and docs position it as a structured conversation-history and context retrieval mechanism.

## Tree map
- src/ (30): implementation
- tui/ (25): terminal interface; separate Go module via tui/go.mod
- test/ (20): tests
- specs/ (6): specifications/contracts
- docs/ (5): architecture and usage docs
- root: index.ts, openclaw.plugin.json, package.json, package-lock.json, tsconfig.json, CHANGELOG.md, RELEASING.md

## README/documentation claims to validate
- Conversation data is retained in SQLite.
- Older conversation segments are summarized and organized as a DAG, with expansion/retrieval paths back toward source messages.
- Tooling names previously surfaced include lcm_grep, lcm_describe and lcm_expand; verify exact registration and schemas in source before treating as confirmed.

## Architecture model
OpenClaw plugin entry → message ingestion/indexing → SQLite + full-text search → summarization/DAG generation → query tools → policy-controlled expansion to original messages. Treat as hypothesis until src and architecture.md are inspected.

## Provenance and licensing
This is a fork of Martian-Engineering/lossless-claw. Do not assume fork changes are upstream or vice versa. Compare fork tree/commit history with parent before reuse. MIT metadata and LICENSE are present; confirm vendored dependencies and notices.

## Test inventory
20 heuristic test paths under test/. Prior path inventory includes config, engine, expansion, FTS fallback/sanitization, indexing, secrets/auth profiles, migration and integration suites. Execution status NOT RUN.

## Next source targets
README.md; docs/architecture.md; index.ts; src/; openclaw.plugin.json; package.json; test/expansion-policy.test.ts; test/migration.test.ts; compare upstream parent.
