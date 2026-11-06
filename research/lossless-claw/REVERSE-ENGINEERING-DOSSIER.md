# Lossless Claw — Reverse Engineering Dossier
**Case ID:** JRY-REA-LCM-001  
**Observed:** 2026-11-06  
**Target:** `Martian-Engineering/lossless-claw`  
**Target branch:** `main`  
**Package version observed:** `1.2.0`  
**License observed:** MIT  
**Method:** Authorized public-source, read-only static inspection. No target code was executed; no runtime interception or private data was accessed.

## 1. Executive result

Lossless Claw is an OpenClaw context-engine plugin implementing a durable conversation archive plus a hierarchical, DAG-backed summarization projection. It stores message records and structured message parts in SQLite, maintains ordered context references, compacts older contiguous material into leaf summaries, condenses summaries into higher levels, and assembles a bounded model prompt from summaries plus a protected recent tail. Retrieval tools allow an agent to search and expand summaries toward source messages.

**Core design insight:** keep canonical source records separate from lossy semantic summaries. Summaries are navigation/working-context artifacts, not replacements for the original transcript.

**Feasibility for an independent JARYAN capability:** HIGH for the core data model and workflow; MEDIUM for host-specific lifecycle integration; NOT YET VALIDATED for performance, correctness under real workloads, or full parity.

## 2. Scope, authority and limits

- Target is a public GitHub repository under its publisher's account.
- Permitted work: public documentation, source code, manifests, tests and license inspection.
- Excluded: bypassing access controls, secrets, private data, runtime interception, deployment, or copying proprietary assets.
- Clean-room boundary: this dossier describes behavior and contracts. Any JARYAN implementation must be independently authored and must not copy source code or protected UI/branding.
- This is a selected-path audit, not an exhaustive audit of every source file, commit, dependency, issue or release.

## 3. Evidence register

| ID | Artifact | Observed evidence | SHA / locator |
|---|---|---|---|
| E-01 | README.md | Product purpose, install/configuration, CLI, commands, migration, compatibility | blob `ea025124f3e025ec7e09ed333924cd0222723f10` |
| E-02 | package.json | v1.2.0, MIT, Node 22 target, TypeScript, Vitest, build/test/typecheck scripts, OpenClaw peer >=2026.9.2 | blob `002826558e44f0a644f549b55a5eddddc78ba79b` |
| E-03 | docs/architecture.md | Conversation/message/parts model, summary DAG, context_items, ingestion, compaction lifecycle | blob `09220ea278e739c88613ab30a89b9dc064bbef79` |
| E-04 | src/assembler.ts | Context assembly diagnostics, overflow contributors, duplicate clusters, role/content sanitation | blob `14de462982ded6207e68fb78e2ad3515b51ab696` |
| E-05 | src/compaction.ts | Compaction decision/result contracts, protected fresh tail, token budgets, bounded rounds/deadlines | blob `493f29452c0c2491ee15e9ee47b2e146562f2f77` |
| E-06 | src/expansion.ts | Depth and token-capped DAG expansion, query-first expansion, cited IDs, truncation reporting | blob `b11682575efdedec806e19f3cb96bdbf6ccc9543` |
| E-07 | src/large-files.ts | File-block parsing and deterministic exploration of text/code/JSON/CSV/TSV/XML/YAML | blob `e1e522bd7d52ab71892ef6bc14a7fe2c3cd32011` |
| E-08 | SECURITY.md | Private vulnerability reporting and supported-release policy | blob `d4b5873eda9ba8571043068a847332ac3f714e2c` |
| E-09 | LICENSE | MIT license text and attribution condition | blob `f40c7f5f6f7b51f7ff3ffbc363fa7aa52f7c0a76` |
| E-10 | AGENTS.md | Explicit non-destructive data preservation rule; compatibility/config-schema/release requirements | blob `6af653c68ff39694875967361d04e33381b5cb88` |

Canonical source URLs:
- https://github.com/Martian-Engineering/lossless-claw
- https://github.com/Martian-Engineering/lossless-claw/blob/main/docs/architecture.md
- https://github.com/Martian-Engineering/lossless-claw/blob/main/src/compaction.ts
- https://github.com/Martian-Engineering/lossless-claw/blob/main/src/assembler.ts
- https://github.com/Martian-Engineering/lossless-claw/blob/main/src/expansion.ts
- https://github.com/Martian-Engineering/lossless-claw/blob/main/src/large-files.ts
- https://github.com/Martian-Engineering/lossless-claw/blob/main/SECURITY.md
- https://github.com/Martian-Engineering/lossless-claw/blob/main/LICENSE

## 4. Architecture reconstruction

### 4.1 Logical entities

- Conversation: one OpenClaw session, keyed by runtime session identity.
- Message: monotonically sequenced record with role, content, token estimate and timestamp.
- Message part: typed content blocks retaining structured message shape (text, tool calls/results, reasoning, file content, etc.).
- Summary: content, conversation scope, depth, time interval, token estimate, file references and lineage.
- Summary-message relation: leaf summary to raw source messages.
- Summary-parent relation: condensed summary to lower-level summaries.
- Context item: ordered reference to either a raw message or a summary.
- Transcript cursor/watermark and replay/idempotency records: incremental ingestion and duplicate protection.
- Maintenance/debt state: deferred preparation/publication work and diagnostics.

### 4.2 Summary DAG and context projection

Leaf summaries represent contiguous ranges of raw messages. Condensed summaries combine summaries at a common depth into a more abstract level. Context items provide the ordered model-facing projection; compaction replaces a range of context references with a summary reference while retaining source records and lineage.

The DAG supports traversal from a summary to child summaries and, at leaf level, to original messages. Expansion is bounded by maximum depth and token budget and returns cited IDs plus truncation status.

### 4.3 Ingestion lifecycle

The inspected architecture describes a host-owned visible-transcript delta reader and a per-conversation opaque cursor. Ingestion commits message rows and cursor advancement in one SQLite transaction. It also describes idempotency receipts, transcript replay keys, resynchronization after cursor invalidation, and gap markers when previously unseen historical material cannot safely be inserted in order.

Important design rule: on uncertainty, retain degraded coverage and diagnostics rather than deleting or rewriting persisted user data.

### 4.4 Compaction lifecycle

1. Determine whether context pressure crosses a configured threshold.
2. Preserve a recent fresh tail.
3. Select the oldest eligible contiguous source range.
4. Summarize through the host LLM capability.
5. Escalate from normal to aggressive prompting if output is unsuitable.
6. Use deterministic bounded fallback when model output is unavailable or invalid.
7. Persist summary and lineage, then atomically update the context projection.
8. Condense same-depth summaries when fanout and budget rules permit.
9. Bound each pass by rounds, deadlines, token limits and spend/cooldown policies.
10. Keep pending work/debt visible for later maintenance.

### 4.5 Context assembly

The assembler resolves ordered context references, reconstructs structured message content, accounts for token estimates and runtime-observed pressure, protects recent content, and emits diagnostics for overflow contributors and duplicate clusters. This is a key lesson: budget decisions should be based on the serialized prompt projection where possible, not only stored estimates.

### 4.6 Large-file handling

The inspected large-file module detects embedded file blocks, parses attributes and selects deterministic structural exploration for common structured formats and code. For long prose it can prepare a bounded head/middle/tail sample for summarization, with a deterministic fallback. This is a retrieval aid, not a substitute for preserving the complete original file.

## 5. Capability matrix

| Capability | Evidence status | JARYAN feasibility | Priority |
|---|---|---:|---:|
| Immutable raw event/message archive | PARTIAL: architecture + repository invariants inspected | High | P0 |
| Structured content-part preservation | VERIFIED in architecture | High | P0 |
| Hierarchical summary DAG | VERIFIED in architecture and expansion code | High | P0 |
| Ordered context projection | VERIFIED in architecture | High | P0 |
| Fresh-tail protection | VERIFIED in compaction contract | High | P0 |
| Token/depth bounded expansion | VERIFIED in expansion code | High | P0 |
| Incremental transcript cursor/idempotency | VERIFIED in architecture | Medium (host adapter needed) | P1 |
| Deferred maintenance/debt tracking | VERIFIED in architecture | Medium | P1 |
| Large-file structural exploration | VERIFIED in selected source | High | P2 |
| CLI/TUI/control UI parity | PARTIAL from README/package | Medium | P3 |
| OpenClaw lifecycle parity | Host-specific | Low without adapter | P3 |

Legend: VERIFIED means direct selected-source/documentation evidence exists, not that an independent implementation has been tested.

## 6. Important engineering findings and risks

1. **Semantic summaries are lossy.** The system's defensible lossless property is source retention plus lineage and retrieval, not lossless summarization.
2. **Token counts are estimates.** Actual provider serialization and hidden overhead can differ; assembly should observe rendered pressure and preserve safety margins.
3. **DAG lineage must be transactional.** Summary persistence and context-reference replacement must not diverge after a crash.
4. **Idempotent ingestion is mandatory.** Cursor advance, message insertions and replay receipts must commit atomically.
5. **Fresh-tail protection needs a token cap.** A fixed number of turns can still exceed the model budget.
6. **Fallback summaries need provenance.** Mark whether output came from a model, deterministic fallback, or operator repair.
7. **Maintenance must be bounded.** Time, rounds, fanout, concurrency and model spend need explicit limits.
8. **Data deletion is a hard safety boundary.** Automatic repair/compaction/migration must never silently delete source records.
9. **Large-file summaries are not complete file representations.** Keep content-addressed originals and byte hashes where retention is permitted.
10. **Provider coupling must remain at adapters.** Domain logic should not import provider SDKs.

## 7. License and supply-chain notes

The inspected LICENSE is MIT and requires preservation of the copyright and permission notice in copies/substantial portions. A clean-room JARYAN implementation should implement independently from this behavior dossier rather than copy source. Re-check the target license and dependency licenses at the time of any distribution. The package uses Node.js/TypeScript and has OpenClaw as an optional peer dependency with a declared minimum version. No full dependency-license audit or vulnerability scan was performed.

## 8. Independent JARYAN architecture proposal

### 8.1 Module boundaries

- `core/events`: canonical append-only event and message records.
- `core/content-parts`: typed structured payloads and references.
- `core/graph`: summary DAG, lineage and graph validation.
- `core/projection`: ordered context-item projection.
- `core/compaction`: deterministic selection and state transitions; model-independent.
- `core/budget`: token estimates, hard caps and observed usage.
- `core/retrieval`: search, describe and bounded expansion.
- `core/provenance`: source IDs, content hashes, derivation records and audit events.
- `adapters/llm`: provider-neutral summarize interface and local/remote implementations.
- `adapters/host`: OpenClaw or other host transcript/lifecycle adapter.
- `storage/sqlite`: local persistence and migrations.
- `ui/cli`: inspect, backup, export, doctor and status.
- `tests/fixtures`: deterministic synthetic conversations and fault injection.

### 8.2 Canonical data rule

Raw records are canonical. Summary records are derived artifacts with explicit source edges. A context projection is a rebuildable view. Never overwrite raw content with a summary. Every derived artifact should record:
- stable ID and schema version
- source event/message IDs or parent summary IDs
- source range and timestamps
- algorithm/prompt version
- model/provider identifier where applicable
- created time and token/byte counts
- output hash
- fallback/repair status
- validation status

### 8.3 Minimal relational schema

- conversations(id, external_key, created_at, updated_at, status)
- events(id, conversation_id, seq, role, created_at, payload_hash, payload_ref, schema_version)
- event_parts(id, event_id, ordinal, part_type, payload_ref, metadata_json)
- summaries(id, conversation_id, depth, kind, content_ref, content_hash, token_estimate, start_seq, end_seq, created_at, method, method_version)
- summary_sources(summary_id, source_kind, source_id, ordinal)
- context_items(conversation_id, ordinal, item_kind, item_id)
- ingestion_cursors(conversation_id, source_name, cursor, generation, updated_at)
- replay_keys(conversation_id, source_key, payload_digest, event_id)
- maintenance_jobs(id, conversation_id, state, reason, attempts, not_before, lease_until, last_error_code)
- provenance_events(id, subject_kind, subject_id, action, actor, timestamp, details_hash)

Enforce foreign keys, uniqueness of (conversation_id, seq), uniqueness of replay keys within source scope, DAG acyclicity, and transaction boundaries.

### 8.4 Provider-neutral contract

`summarize(input, options) -> SummaryCandidate`

Input: source references, bounded content view, prior context, target token range, language, prompt/version ID, cancellation/deadline.

Output: candidate text, provider/model metadata, usage if known, finish reason, warnings.

The core validates output size, non-empty content, lineage, and budget before publication. Provider errors must not mutate canonical source records.

### 8.5 Retrieval contract

- `grep(query, scope, filters, limit)`
- `describe(target_id, depth, token_cap)`
- `expand(summary_ids, max_depth, token_cap, include_raw)`
- `export(conversation_id, format)`
- `verify(conversation_id)`

Every retrieval response includes source IDs, truncation status, total budget consumed and provenance links.

## 9. Acceptance test plan

| ID | Test | Acceptance |
|---|---|---|
| AT-01 | Ingest same source page twice | No duplicate canonical event; replay key stable |
| AT-02 | Crash between ingestion and cursor advance | Transaction rolls back both or commits both |
| AT-03 | Create leaf summary | Exact source range links to summary |
| AT-04 | Condense summaries | Parent edges valid; DAG acyclic |
| AT-05 | Replace context range | Projection order preserved; raw records unchanged |
| AT-06 | Expand summary | Correct descendants; obey depth/token caps |
| AT-07 | Exceed expansion cap | Truncation=true; no unbounded traversal |
| AT-08 | Provider timeout | Source records unchanged; retryable job recorded |
| AT-09 | Invalid/oversized model output | Rejected or bounded fallback explicitly marked |
| AT-10 | Huge fresh tail | Fresh-tail token cap and assembly safety policy enforced |
| AT-11 | Rebuild projection | Same ordered context from canonical records |
| AT-12 | Corrupt/missing lineage edge | Doctor reports integrity failure; no automatic source deletion |
| AT-13 | Export/import round trip | Canonical event payload hashes preserved |
| AT-14 | Large file | Full original hash retained; exploration summary clearly marked derived |
| AT-15 | Concurrent compaction | Per-conversation serialization; no duplicate publication |
| AT-16 | Maintenance deadline | Stops within configured budget and leaves retryable debt |

## 10. WBS and gates

- W0 (P0): finalize data contract and threat model.
- W1 (P0): SQLite schema, migrations, append-only event store.
- W2 (P0): deterministic context projection and integrity verifier.
- W3 (P0): summary DAG and lineage.
- W4 (P0): bounded expansion/retrieval.
- W5 (P1): provider adapter + deterministic fallback.
- W6 (P1): compaction state machine and fresh-tail policy.
- W7 (P1): maintenance queue, retries, cancellation and deadlines.
- W8 (P2): CLI, export/import, backup and doctor.
- W9 (P2): host adapters and optional UI.

Gates:
- G0 rights/scope: PASS for public-source study; implementation attribution review required before distribution.
- G1 evidence: PARTIAL; selected paths inspected, not exhaustive.
- G2 feasibility: PASS for local SQLite core; provider/host integration needs adapter.
- G3 architecture: PROPOSED; not implemented.
- G4 build: NOT RUN.
- G5 parity: NOT RUN.
- G6 handoff: PASS for this dossier only; no executable module delivered.

## 11. Status and explicit non-claims

- Public source inspection: EXECUTED for listed artifacts.
- Architecture reconstruction: EXECUTED.
- Clean-room architecture proposal: EXECUTED as specification.
- Target build/tests: NOT RUN.
- Independent JARYAN implementation: NOT RUN.
- Performance, offline and resource benchmarks: NOT RUN.
- Full repository/history/dependency/security audit: NOT RUN.
- Full behavioral parity: NOT CLAIMED.

## 12. Next validation actions

1. Inspect the complete storage schema, migrations, retrieval implementation and test suites.
2. Review tests for ingestion idempotency, compaction transactions, graph expansion and failure recovery.
3. Run the target's typecheck/build/tests only in an isolated environment after obtaining a local source snapshot; do not run repository scripts blindly.
4. Implement a small provider-independent SQLite vertical slice in a separate JARYAN module.
5. Run AT-01 through AT-16 against synthetic fixtures and publish exact test output.
