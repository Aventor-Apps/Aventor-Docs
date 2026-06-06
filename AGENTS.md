# AGENTS.md — AVE-13 Planner Support local-Codex handoff

This branch contains Planner Support handoff docs for Linear `AVE-13`. Aventor-Docs is required for the docs-site update that gives the launch-agent warnings accurate setup/readiness targets.

## Read these first

- `docs/linear/AVE-13/PLAN.md`
- `docs/linear/AVE-13/ARCHITECTURE.md`
- `docs/linear/AVE-13/BRAINSTORM.md`
- `docs/linear/AVE-13/QA.md`
- `docs/linear/AVE-13/EXECUTOR.md`
- `docs/linear/AVE-13/REVIEWERS.md`
- `docs/linear/AVE-13/LINEAR_UPDATE.md`
- `docs/linear/AVE-13/source_bundle/context_manifest.md`

## Critical scope boundaries for AVE-13 docs work

Active docs scope includes:

- update `integrations/google-ads.mdx` in-place with Google Ads connection/customer selection, conversion tracking/conversion actions readiness, and GBP readiness guidance;
- create/verify anchors `#conversion-tracking-readiness` and `#google-business-profile-readiness`;
- preserve Meta warning target `/integrations/pixel-setup`;
- avoid claiming the future AVE-40 setup wizard is already shipped;
- validate docs with repo-supported tooling if available, or static JSON/MDX/link/anchor checks if no docs CLI/package scripts exist.

Out of scope:

- new docs page unless implementation intentionally changes the docs-link contract and updates backend constants/tests;
- AVE-40 setup wizard docs as shipped behavior;
- planning-docs-only PR.

## Graphify / docs-first inspection

If `graphify-out/GRAPH_REPORT.md` exists, read it before raw source search. For relationship questions, prefer Graphify query/path/explain when available, then verify against current source. Backend graph report was missing at planning time, so use docs-first fallback plus targeted source inspection there.

## Local plugin expectations

Use local Linear and local Supabase/plugin access where available. Use local Codex `linear-executor` for orchestration and `linear-reviewer` for independent review. Do not depend on Planner Support's Hermes-only credentials, MCP tool names, or filesystem assumptions.

No live Supabase data/schema mutation is allowed without explicit TJ approval unless the approved implementation itself creates repo migration files for review. Do not apply live migrations from Planner docs.

## UI routing

Substantive visible UI/design/styling/layout/component work is not expected for AVE-13. If it becomes necessary, follow the local `linear-executor` Open Design route instead of direct Claude/Kimi/plain-Codex UI work:

1. Inspect the repo design system first.
2. Fetch an existing Open Design artifact/design bundle with `get_artifact()` when available.
3. If no artifact exists, create/commission a new Open Design artifact with `create_project` if needed, `start_run`, `get_run`, then `get_artifact`, requiring reuse of repo primitives/tokens.
4. Record the Open Design source/run and integration scope in the workpad.
5. If Open Design MCP/artifact fetch/artifact creation is unavailable or blocked, mark the UI phase `blocked`/`BLOCKED_DESIGN` and stop. Do not fall back to plain Codex, Claude Code, Kimi, or raw Ollama for substantive UI/design work.

Codex may still own backend contracts, tests, validation, docs, and non-visual frontend API/type plumbing.

## Mastra-owned user response interpretation

Natural-language user response interpretation must stay Mastra-owned. Deterministic/backend code must not parse freeform user text with regex/string matching to decide `switch`, `continue`, `pause`, or similar intent. The orchestrator/Launch Agent should convert user language or button selections into structured `finalizationDecision` / `finalizationDecisions` payloads; deterministic code may validate, persist, and apply only those structured decisions by stable IDs/actions.

## QA package and reviewer artifacts

The executor must create/update the final QA package before Human Review:

- `docs/linear/AVE-13/qa-package/checkpoint_manifest.md`
- `docs/linear/AVE-13/qa-package/browser_qa_prompt.md`
- `docs/linear/AVE-13/qa-package/validation_results.md`
- `docs/linear/AVE-13/qa-package/qa_readiness_result.json`

The canonical QA package lives in the backend repo unless implementation findings justify otherwise. Independent reviewer findings should live under `docs/linear/AVE-13/qa-artifacts/`.

Browser screenshots/video are optional by default and should only be captured when useful to prove/debug a visible finding or if TJ asks.

## Linear safety

Do not move Linear status automatically during execution. Do not create subtasks unless TJ explicitly says `create the subtasks`. Do not update Linear description/comments unless TJ explicitly approves that mutation.
