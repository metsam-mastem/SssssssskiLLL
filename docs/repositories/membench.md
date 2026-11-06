# membench

- Upstream: https://github.com/thedotmack/membench
- Snapshot: main @ fe61cf0a484c3e7932888e4f366b5aaf5aad938c
- Tree: 83 blobs; 847,584 bytes; heuristic test paths: 25
- Metadata license: MIT; root LICENSE, NOTICE and SECURITY.md present
- Language metadata: TypeScript
- Status: L0 PASS; L1 PARTIAL; L2/L3 IN PROGRESS; L4 NOT RUN

## Product category
Evaluation/benchmark harness for memory systems in AI coding agents; not itself assumed to be a production memory engine.

## Tree map
- src/ (26): benchmark/application logic
- test/ (25): tests
- scripts/ (7): runners and automation
- docs/ (6): methodology and supporting documentation
- schemas/ (4): structured contracts
- root: package.json, bun.lock, bunfig.toml, tsconfig.json, SECURITY.md, CONTRIBUTING.md

## README-level evaluation model
Prior documentation review identified comparative conditions such as candidate memory, no-memory baseline, shuffled memory and reference condition. The purpose is to measure whether memory improves outcomes rather than merely adding context. Exact scoring, judge protocol and dataset boundaries still require source-level verification.

## Security / data handling
SECURITY.md explicitly says the repository must remain private and prohibits credentials, real user data, model transcripts, local filesystem details and production run artifacts in issues, PRs, commits, fixtures or logs. Release gate is defense-in-depth and does not prove arbitrary runtime behavior. Keep all reproduced experiments synthetic and scrubbed.

## Test inventory
25 heuristic test paths, including command tests, phase 2 corpus/metrics/scheduling/spec tests, phase 3 end-to-end/judge calibration/observer/orchestrator/sandbox tests, phase 4 CI scanners/report bundles, privacy scan and release gates. Not executed.

## Feasibility
Useful as a reference for evidence-driven memory evaluation and regression gates. Reuse should preserve dataset privacy and distinguish offline static checks from model-judge evaluations.

## Next source targets
README.md; SECURITY.md; package.json; src/; docs/methodology.md; schemas/; test/phase3-e2e.test.ts; release-gate and privacy-scan implementations.


## Source-level findings (confirmed by code inspection)
- src/artifacts.ts freezes protocol descriptors for observer, executor, reference and judges; harness descriptor includes version 0.2.0, artifact schema 1, schedule algorithm, item bootstrap algorithm and PRNG algorithm identifiers.
- src/orchestrator.ts validates exact schedule-entry shapes, bounds repetitions, checks duplicate attempt IDs and duplicate item/lane/candidate/arm/repetition coordinates, canonicalizes schedule bytes and hashes the manifest.
- src/observer.ts defines a strict JSON Schema for memory observations, disallows additional properties and bounds IDs, text and metadata; it records parse states such as valid, invalid_json, invalid_schema, invalid_semantics and not_run_budget.
- src/calibration.ts binds a reference control to the exact corpus item and lane, hashes the binding, freezes the object and uses an issuance registry to reject caller-forged references.
- src/metrics.ts models attempt outcome, attribution, drift avoidance, schema validity, token/time/cost and effective provider/model as distinct fields.
- docs/methodology.md confirms paired item-level inference, seeded bootstrap over items (not attempts), Bonferroni adjustment across the declared comparison family, explicit unknown/missingness handling and independent reassessment.
- This is unusually strict experiment governance: schedule and artifacts are derived/committed; public comparisons are not supposed to accept caller-supplied score arrays.

## Core flow (source-grounded)
Validated corpus/events + frozen spec → canonical hashes and schedule → independent calibration → observer emits candidate memory → candidate/none/shuffled/reference arms → isolated executor → mechanical/blinded outcome + attribution/drift judges → item-level paired inference → independent reassessment → report bundle.
