# claude-mem

- Upstream: https://github.com/thedotmack/claude-mem
- Snapshot: main @ 8954effe93546e9cd55b793a878f4bf855c69b22
- Tree: 3,030 blobs; 59,485,747 bytes; heuristic test paths: 1,543
- Metadata license: Apache-2.0; root LICENSE and NOTICE present
- Language metadata: TypeScript; package version in package.json: 15.10.0
- Status: L0 PASS; L1 PARTIAL; L2/L3 IN PROGRESS; L4 NOT RUN

## Product category
Persistent agent memory and context compression layer, presented as a multi-runtime integration system. README claims capture of tool observations, semantic summaries, retrieval across sessions, progressive disclosure, citations, privacy controls, local viewer and optional cloud sync.

## Tree map (top-level)
- tests/ (1,346): test suites, fixtures and evaluation assets
- src/ (883): core source and UI/service modules
- plugin/ (199): plugin hooks, skills, modes and packaged runtime assets
- docs/ (147): product, integration and multilingual documentation
- services/ (81): service components
- openclaw/ (63), cowork/ (62), claude-mem-cursor/ (58), claude-mem-grok-bot/ (58): runtime/client integrations
- scripts/ (57): build, release and maintenance tooling
- ods/, docker/, dsh/, hermes/, pi/: additional integration/deployment packages

## Confirmed entry and packaging evidence
- package.json declares ESM package, CLI bin at dist/npx-cli/index.js, Node >=20.19.0 or >=22.12.0 and Bun >=1.1.31.
- Scripts expose build, worker lifecycle, typecheck, fast/integration/all test lanes, SQLite tests, agent/search/context/server suites, Docker E2E, sync matrix E2E and eval read-gate.
- README explicitly distinguishes npm SDK install from npx install flow that registers hooks and worker service.
- README lists Claude Code, OpenClaw, Codex, Gemini, Hermes, Copilot, OpenCode and other integrations; each must be validated against the integration code.

## Architecture hypothesis to validate
Capture hooks/integrations → worker/service queue → observation processing and LLM summarization → SQLite/storage and indexing → search/context assembly → agent-specific injection and viewer/API. This is an analysis model, not yet a code-proven data-flow.

## Risks / questions
- Identify exact data schemas, redaction/private-tag behavior, retention/deletion semantics and cloud-sync boundaries.
- Separate local observer from hosted observer/provider flows.
- Audit process spawning, hook I/O discipline, native binaries, image conversion and postinstall allowlist.
- Clarify 1,543 test-path count: includes large tests subtree and may include fixtures or non-test artifacts.

## Verification
- Source read: README.md, package.json (blob SHA captured during fetch; add exact SHAs in evidence register).
- Tree inventory: top-level counts recorded in repository inventory workflow.
- Tests: INVENTORIED only; not run in this environment.
- License: metadata Apache-2.0 and root LICENSE present; NOTICE and vendored subtrees need separate review.

## Next source targets
src/services/worker-service.ts; src/services/context/; src/services/search/; plugin/hooks/; plugin/skills/; tests/sqlite/; tests/worker/; scripts/test-lanes.ts; docs/configuration and privacy/sync guides.


## Source-level findings (confirmed by code inspection)
- src/cli/handlers/observation.ts documents a pure hook handler: it packages observation events into a spool and delegates diagnostics; it explicitly avoids direct stdout/stderr writes and process exit in the handler.
- The observation path bounds payloads, filters project/tool events, redacts log output and attempts server dispatch with a 2-second timeout before falling back to local spool.
- src/services/worker-service.ts imports Bun SQLite, Node child_process spawn, MCP stdio client transport, hook spool/drainer, retention pruning, worker supervisor, environment sanitizer, Chroma sync and telemetry. This is a long-running worker/supervisor, not merely a prompt file.
- src/cli/handlers/memory-search.ts bounds query length/UTF-8 bytes, resolves memory roots and restricts memory path handling to configured/native memory roots.
- Package scripts include hook I/O and spawn-environment discipline checks, clean-room smoke, postinstall allowlist check, worker test lanes and integration/E2E lanes.
- Tree has multiple integration packages (OpenClaw, OMP, Pi, DSH, Hermes, ODS, Cursor, Grok Bot) alongside core; separate runtime contracts and release artifacts must be audited per adapter.

## Core flow (source-grounded)
Agent hook → normalize/filter/bound event → server dispatch or local spool → worker drain → worker-side processing/storage/indexing → smart search/context assembly → runtime-specific hook/MCP context delivery.
