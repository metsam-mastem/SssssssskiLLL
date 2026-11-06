# hermes-agent

- Upstream: https://github.com/thedotmack/hermes-agent
- Parent/source: https://github.com/NousResearch/hermes-agent
- Snapshot: main @ 73d0b083510367adec42746e90c41ace16c0afb2
- Tree: 2,133 blobs; 37,027,305 bytes; heuristic test paths: 665
- Metadata license: MIT; root LICENSE and SECURITY.md present
- Fork: true
- Status: L0 PASS; L1 PARTIAL; L2/L3 IN PROGRESS; L4 NOT RUN

## Product category
General-purpose agent/runtime ecosystem described by upstream as an agent that grows with the user. Tree indicates Python core plus CLI, gateway, toolsets, skills, plugins, environments, web UI/TUI and documentation.

## Tree map
- tests/ (650): automated test suites and fixtures
- skills/ (437): reusable agent skills
- ui-tui/ (229): terminal UI and packages
- optional-skills/ (187): optional capability bundles
- website/ (148), web/ (65): documentation/site and web components
- tools/ (78): tool implementations
- hermes_cli/ (50), gateway/ (49), agent/ (34): CLI, gateway and agent runtime areas
- environments/ (43), plugins/ (36), scripts/ (17), acp_adapter/ (9), tui_gateway/ (5)
- root: run_agent.py, mcp_serve.py, pyproject.toml, requirements.txt, Dockerfile, setup-hermes.sh, toolsets.py, model_tools.py, trajectory_compressor.py and hermes_state.py

## Architecture model to validate
CLI/config entry → agent loop and provider adapters → tool registry/toolsets and skills → context/memory and trajectory compression → gateway/MCP/ACP interfaces → TUI/web clients and environment runners. Exact control flow, state ownership and plugin loading must be confirmed from code.

## Provenance and fork delta
Fork of NousResearch/hermes-agent. Analyze local fork as its own snapshot, then compare commits and changed files against parent. MIT metadata/root LICENSE are present; check vendored plugins/skills and bundled assets for separate terms.

## Test inventory
665 heuristic test paths. Examples include ACP auth/events/MCP E2E/server/session/tools and numerous agent/provider adapter tests. Inventory is not execution evidence.

## Security focus
Tool execution boundaries, shell/process invocation, provider credential storage, gateway auth, MCP permissions, plugin trust, skill prompt injection, session isolation, environment sandboxes and telemetry.

## Next source targets
run_agent.py; agent/; tools/registry.py or equivalent current registry; trajectory_compressor.py; hermes_state.py; gateway/; hermes_cli/; plugins/memory/; pyproject.toml; SECURITY.md; tests/agent and tests/acp; compare NousResearch parent.


## Source-level findings (confirmed by code inspection)
- agent/context_engine.py defines a pluggable context-engine interface selected by config; only one context engine is active. Lifecycle includes session start, usage updates, should_compress, compress and session end.
- agent/memory_manager.py centralizes built-in memory plus at most one external memory provider; this is an explicit constraint to avoid tool-schema bloat/conflicting backends.
- toolsets.py composes reusable named toolsets from core tool names; tools include terminal/process, file operations, web, skills and browser automation.
- README claims self-improving skills, periodic memory nudges, FTS5 session search, LLM summaries, multiple messaging platforms and terminal backends. These capabilities need per-module validation and should not be treated as one monolithic agent feature.
- run_agent.py is a very large entry/runtime file (~621 KB in this snapshot); this raises maintainability and review-cost concerns and suggests decomposing analysis by class/function and test ownership.
- Fork of NousResearch/hermes-agent: parent delta remains unmeasured because cross-repository compare endpoint returned 404 in this pass.

## Core flow (source-grounded at interface level)
CLI/gateway entry → provider/model adapter → agent loop → context engine + memory manager → tool registry/toolsets → tool execution → session and channel delivery. Exact orchestration order still requires deeper function-level trace.
