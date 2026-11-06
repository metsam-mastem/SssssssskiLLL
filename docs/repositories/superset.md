# superset

- Upstream fork: https://github.com/thedotmack/superset
- Parent/source: https://github.com/ParadevAI/Paradev
- Snapshot: main @ d4b10385dcd75f3259bb163e53575cd288fac85d
- Tree: 5,344 blobs; 83,927,768 bytes; heuristic test paths: 333
- GitHub license metadata: NOASSERTION; root LICENSE.md exists
- Fork: true
- Status: L0 PASS; L1 PARTIAL; L2/L3 IN PROGRESS; L4 NOT RUN

## Product category
Desktop development environment for parallel AI coding agents. Repository description says Code Editor for the AI Agents Era. Prior README review described concurrent agent workspaces and Git worktrees; implementation details must be validated.

## Tree map
- apps/ (3,905): application surfaces; includes desktop, API, web, mobile, admin and supporting apps
- packages/ (1,288): shared packages including auth, chat, DB, CLI, MCP, UI, workspace and process helpers
- plans/ (47): product/engineering plans
- .github/ (25), .agents/ (12), .superset/ (10), docs/ (8), scripts/ (8), tooling/ (5)
- root manifests: package.json, bun.lock, turbo.jsonc, biome.jsonc, AGENTS.md and app-specific manifests

## Architecture model to validate
Desktop client + API/TRPC + shared packages + workspace/process/PTY services + database and Git worktree operations. Determine whether shell/agent processes run locally, in helper daemons or remote services; do not infer sandbox strength from the UI.

## Licensing blocker
GitHub metadata returns NOASSERTION despite a root LICENSE.md and nested package/sdk/LICENSE files. Do not declare the entire monorepo permissively licensed until root LICENSE.md, package-specific notices, vendored components and parent repository license are examined. Record the exact scope of any license.

## Security focus
Workspace isolation and path traversal, worktree lifecycle, process spawning, terminal/PTY access, IPC, local DB secrets, API auth, MCP bridges, external integrations, update mechanisms and cross-workspace leakage.

## Test inventory
333 heuristic test paths; examples include Slack API route tests, Git operations, PR merge helpers, worktree import, terminal path resolution, preset execution and resource-metrics schema. Not executed.

## Next source targets
README.md; LICENSE.md; package.json; apps/desktop; apps/api; packages/db; packages/local-db; packages/host-service; packages/pty-daemon; packages/mcp; packages/workspace-fs; packages/trpc; AGENTS.md; compare ParadevAI/Paradev.


## Source-level findings (confirmed by code inspection)
- Desktop workspaces resolve their path differently by workspace type: branch workspace uses the project main repository path; worktree workspace resolves its stored worktree path from local DB.
- packages/pty-daemon is a distinct daemon package using node-pty, Node >=20, Bun tests and separate integration tests for control plane, signal recovery, byte fidelity and handoff.
- Desktop change/workspace routers include dedicated Git command and path validation modules, worker/task handlers, status parsing and worktree import tests.
- Root LICENSE.md is Elastic License 2.0; the metadata NOASSERTION should not be interpreted as open-source permissive licensing. The hosted/managed-service restriction is material.
- Fork of ParadevAI/Paradev; parent delta is not yet established.

## Core flow (source-grounded at interface level)
Desktop workspace record → workspace type/path resolution → Git/worktree operations → PTY daemon/process session → desktop/API/TRPC state and diff/status presentation. Validate isolation and process lifecycle before assuming separate workspaces are security sandboxes.
