# ODS

- Upstream fork: https://github.com/thedotmack/ODS
- Parent/source: https://github.com/Osmantic/ODS
- Snapshot: main @ fce1a2281343cd9de1ca0622b4bbf28f3469523e
- Tree: 5,818 blobs; 78,071,722 bytes; heuristic test paths: 2,214
- Metadata license: Apache-2.0; root LICENSE, SECURITY.md, SECURITY_AUDIT.md and ARCHITECTURE.md present
- Fork: true; upstream description explicitly says V3 Pre-Release
- Status: L0 PASS; L1 PARTIAL; L2/L3 IN PROGRESS; L4 NOT RUN

## Product category
Local/private AI server and deployment stack for PC, Mac and Linux. The source description labels V3 as pre-release; do not treat as production-ready without deployment validation.

## Tree map
- ods/ (5,719): main system, services, extensions, configuration, docs, agents and runtime components
- .github/ (74): workflows and repository automation
- .claude/ (9), root installers install.sh/install.ps1
- root architecture/security/contribution docs and license

## Architecture model to validate
Host bootstrap/installers → orchestration and service definitions → container/runtime services → model and inference stack → UI/API/extensions → storage, observability and governance. This is a working decomposition, not a confirmed execution graph.

## Security and licensing
Root docs include ARCHITECTURE.md, SECURITY_AUDIT.md and SECURITY.md. Read remediation notes and current security workflows before relying on any audit claim. Apache-2.0 metadata/root LICENSE do not automatically grant the same license to every upstream extension, model, container image or bundled service. Build a component-level license inventory.

## Test inventory
2,214 heuristic test paths, with mixed Python/JS tests, fixtures and benchmark helpers. This unusually large count must be decomposed by actual test framework and package; it is not a count of runnable tests. Examples include approval expiration, audit boundaries, governance contracts and service configuration checks.

## Next source targets
ARCHITECTURE.md; SECURITY_AUDIT.md; SECURITY.md; install.sh; install.ps1; ods/README.md; ods/docs/SECURITY_REMEDIATION_2026-09-28.md; ods/extensions/README.md; services and compose manifests; test configs and CI workflows; compare Osmantic/ODS.
