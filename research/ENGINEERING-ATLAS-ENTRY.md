# Engineering Atlas Entry — Lossless Context Management
**Atlas ID:** META-MEM-LCM-001  
**Case:** JRY-REA-LCM-001  
**Date:** 2026-11-06  
**Status:** Research record created; independent implementation not started.

## Taxonomy
- META / GOV / AGENT: agent memory and context governance
- DATA: event store, relational persistence, projections, lineage
- ARC: DAG, bounded traversal, provider adapters
- OFF: local-first SQLite persistence; model summarization may still require a provider
- OPS / SEC: backups, idempotency, bounded maintenance, diagnostics
- DOC / EVD: source registry, evidence status, decision and test records

## Atomic claims
| Claim ID | Claim | Status | Evidence |
|---|---|---|---|
| C-01 | Lossless Claw separates persisted messages from summary artifacts | EVIDENCE | E-03 |
| C-02 | Summary hierarchy is represented as a DAG with source/parent relations | EVIDENCE | E-03, E-06 |
| C-03 | Ordered context items reference either messages or summaries | EVIDENCE | E-03 |
| C-04 | Compaction protects a fresh tail and is bounded by token/round/time controls | EVIDENCE | E-05 |
| C-05 | Expansion is bounded by depth and token caps and reports truncation | EVIDENCE | E-06 |
| C-06 | Ingestion cursor and message persistence are transactionally coordinated | EVIDENCE | E-03 |
| C-07 | Summary generation itself is lossless | REJECTED | Summarization is semantic and lossy; source retention is the lossless boundary |
| C-08 | Target has been independently built and tested in this investigation | NOT RUN | No local execution performed |

## Reusable recipe: Durable Context Projection
**Recipe ID:** RCP-META-DCX-001  
**Version:** 0.1.0  
**Purpose:** Keep a durable source archive while assembling compact, bounded working context.

### Inputs
- Canonical append-only events/messages
- Structured content parts
- Context budget and tokenizer/estimator
- Fresh-tail policy
- Optional summarization adapter
- Stable source and replay identifiers

### Outputs
- Durable canonical records
- Derived summary DAG with lineage
- Ordered context projection
- Retrieval results with citations and truncation status
- Maintenance/health diagnostics

### Preconditions
- Stable conversation identity
- Transactional local storage
- Explicit retention and data sensitivity policy
- Bounded summarization and retrieval budgets

### Decision points
1. Is the new source event already seen? If yes, idempotently acknowledge.
2. Is context pressure above threshold? If no, preserve current projection.
3. Is there a contiguous eligible prefix outside the fresh tail? If no, report protected-tail pressure.
4. Is a summarizer available? If no, defer or use explicitly labeled deterministic fallback.
5. Does candidate summary pass size and provenance validation? If no, reject candidate.
6. Can context references be atomically replaced? If no, preserve old projection and retry.

### Failure paths
- Cursor invalidation: resync without deleting canonical rows.
- Model timeout: retain source and retry later.
- Invalid summary: reject and record failure.
- Token estimate mismatch: remeasure serialized assembly and apply bounded fallback.
- Broken lineage: report integrity issue; never silently prune source.

### Acceptance
- AT-01 through AT-16 in the REA dossier.
- No raw event mutation during compaction.
- All derived records trace to source IDs.
- Export/import preserves canonical hashes.
- Expansion enforces depth and token caps.

### Risks and limitations
- Summary content is lossy.
- Token estimates may diverge from provider serialization.
- Host transcript APIs are adapter-specific.
- SQLite concurrency and migration behavior require executable tests.
- No performance, security or parity benchmark has yet been run.

### Source
See [Reverse Engineering Dossier](lossless-claw/REVERSE-ENGINEERING-DOSSIER.md) and the upstream repository: https://github.com/Martian-Engineering/lossless-claw

## Decision
Adopt the *pattern*, not the implementation: canonical append-only source records + derived summary DAG + rebuildable ordered context projection + bounded cited retrieval. Build a small local SQLite vertical slice before any host plugin integration.
