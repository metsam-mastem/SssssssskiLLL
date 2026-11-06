# humanish

- Upstream: https://github.com/thedotmack/humanish
- Snapshot: main @ 84ece3e1c4290e000d4685492182ccab1387c84a
- Tree: 5 blobs; 79,894 bytes; heuristic test paths: 0
- Metadata license: Apache-2.0; root LICENSE present
- Independent repository; updated 2026-10-29; last push 2026-10-03
- Status: L0 PASS; L1 PARTIAL; L2/L3 IN PROGRESS; L4 NOT RUN

## Product category
Repository description: single-use, scoped credential broker for AI chats. Earlier documentation inspection suggested browser-mediated credential handling; exact architecture and cryptographic flow remain to be proven from all five tracked files.

## Tree map
- README.md
- LICENSE
- .gitignore
- plans/ (one tracked file)
- reports/ (one tracked file)

## Security-critical review checklist
- Credential scope and least privilege
- Token entropy, expiry, single-use enforcement and replay prevention
- Binding between user, task, browser session and credential
- Encryption algorithm, nonce generation, key storage/rotation and plaintext lifetime
- Browser isolation, persistence, downloads and profile cleanup
- SSRF, origin allowlists, redirect handling and DNS rebinding
- Audit logs without leaking secrets
- Error paths, retries, race conditions and concurrent redemption

## Test inventory
No test-named paths were identified by the initial heuristic. This does not prove absence of tests; inspect scripts, reports and execution harnesses. No test has been run.

## Feasibility
Small codebase and narrowly scoped security function make it suitable for complete file-by-file review. However, security suitability cannot be inferred from README or an encryption label; it requires threat-model validation and adversarial tests.

## Next source targets
Read all five files; extract design decisions from plans/ and reports/; verify every security checklist item against implementation. Do not make compliance claims without independent evidence.
