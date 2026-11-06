# humanish

- Upstream: https://github.com/thedotmack/humanish
- Snapshot: main @ 84ece3e1c4290e000d4685492182ccab1387c84a
- Tree: 5 blobs; 79,894 bytes; heuristic test paths: 0
- Metadata license: Apache-2.0; root LICENSE present
- Independent repository; last push 2026-10-03; metadata updated 2026-10-29
- Status: L0 PASS; L1 PASS for inventory; L2 design-level only; implementation verification BLOCKED by absence of implementation source; L4 NOT RUN

## Product category
Single-use, scoped credential broker for AI chats. README describes a two-tier design: Cloudflare Worker link broker plus Fly.io container hosting neko browser isolation and a Playwright action runner.

## Critical finding: documentation-only snapshot
The complete tree contains exactly five blobs:
- .gitignore
- LICENSE
- README.md
- plans/2026-05-14-humanish.md
- reports/2026-05-14-humanish-server.md

There is no src/, package.json, worker source, Dockerfile, deployment manifest, test directory or executable implementation in this snapshot. Therefore this repository is a design/plan artifact, not an auditable implementation at this revision. The previous phrase “small codebase” is incorrect and is superseded by this finding.

## Architecture described by documents (not code-verified)
- Tier 1: Cloudflare Worker, Durable Object-backed slug claim/single-use handling, magic-link dashboard authentication, short-lived bearer delivery.
- Tier 2: Fly.io VM/container, neko Chromium browser for human login, Node/Playwright action runner, encrypted browser storageState, first planned action via notebooklm-py.
- User-mediated trust transition: the user opens the slug page, copies the generated skill markdown and pastes it into the AI chat; the design explicitly rejects relying on AI WebFetch to import instructions.
- README claims AES-GCM at-rest encryption and one-action/profile/TTL scoping. These are design claims only until source and deployed behavior exist.

## Security-critical review checklist for a future implementation
- Credential scope and least privilege
- Token entropy, expiry, single-use enforcement and replay prevention
- Atomic slug claim under concurrent requests
- Binding between user, task, browser session, profile and credential
- AES-GCM nonce uniqueness, key storage/rotation and plaintext lifetime
- Browser isolation, persistence, downloads and profile cleanup
- SSRF, origin allowlists, redirect handling and DNS rebinding
- Audit logs without leaking secrets
- Error paths, retries, race conditions and concurrent redemption
- Fly private network authentication and Worker-to-runner bearer rotation

## Test inventory
No test-named paths and no implementation test harness in this snapshot. No test has been run. Any claim of security or functional behavior remains unverified.

## Feasibility
The design can be reviewed completely at document level. Implementation feasibility and security assurance are BLOCKED until the actual Worker, Durable Object, container, action runner, infrastructure manifests and tests are published or supplied.

## Source targets when implementation appears
Worker entrypoint; Durable Object slug state machine; dashboard auth; encryption/decryption; Playwright runner; container/VM deployment config; action registry; tests for replay, race, scope escalation and cleanup.
