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
