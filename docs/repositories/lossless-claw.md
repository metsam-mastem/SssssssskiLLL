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


## Source-level findings (confirmed by code inspection)
- src/engine.ts implements the OpenClaw ContextEngine adapter and wires assembler, compaction, SQLite connection/migrations, expansion grants, retrieval, conversation and summary stores.
- src/compaction.ts exposes threshold/manual/none decisions; defaults in its config contract include context threshold 0.75, fresh-tail protection 8 turns, leaf chunk ceiling 20,000 tokens, leaf summary target 600 tokens, condensed target 900 tokens and max 10 rounds.
- src/assembler.ts reconstructs model-ready messages under a token budget and estimates tokens at roughly 4 characters/token; it can add depth-aware system-prompt guidance when summaries exist.
- src/expansion-policy.ts separates query_probe from explicit_expand and chooses answer_directly, expand_shallow or delegate_traversal using depth, candidate count and estimated token-risk thresholds.
- src/retrieval.ts exposes summary/file descriptions with DAG lineage fields (parents, children, message IDs, descendants, source token counts and time bounds).
- Important discrepancy to test: README architecture text describes typical leaf summaries around 800–1200 tokens and condensed summaries around 1500–2000, while compaction source config defaults list targets of 600 and 900. Determine whether those are prompt targets, defaults or a version/documentation drift.
- Package.json declares Vitest test command: vitest run --dir test; test inventory is 20 files.

## Core flow (source-grounded)
OpenClaw lifecycle → engine bootstrap/reconcile → ingest raw messages and message parts → SQLite stores → compaction decision → leaf/condensed summaries → ordered context_items replacement → assembler under token budget → retrieval/expansion tools with authorization/policy.


## Parent delta
Parent current head: Martian-Engineering/lossless-claw @ 51c3c90bfe82fa649e4895dd1a2c4874b5e28f76 (2026-11-05). Fork head: 4fdb53ca9949faf1e7ffaa6abadca7f8c9921130 (2026-03-12). Path/blob comparison: parent 371 files, fork 105; 9 only in fork, 275 only in parent, 71 shared paths with different blobs. This is a stale and materially reduced fork snapshot relative to current parent; see [fork delta](../comparative/fork-delta.md).
