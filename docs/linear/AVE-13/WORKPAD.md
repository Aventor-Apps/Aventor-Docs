# AVE-13 Codex Workpad

## Scope
- Issue: AVE-13 Launch Agent data-first readiness preflight + docs update.
- Backend worktree: C:\Users\kktho\code\aventor-workspaces\doc-rule-updates\AVE-13\Aventor-backend on branch AVE-13.
- Frontend worktree: C:\Users\kktho\code\aventor-workspaces\doc-rule-updates\AVE-13\Aventor on branch AVE-13.
- Docs worktree: C:\Users\kktho\code\aventor-workspaces\doc-rule-updates\AVE-13\Aventor-Docs on branch AVE-13.

## Loaded skills / obligations
- linear-executor: docs-first, workpad, TDD, Open Design for substantive UI, QA package, reviewer gates.
- linear-reviewer: report-only QA artifacts and local browser QA target unless blocked.
- agent-browser: verify CLI and attach to CDP for browser QA; no ad hoc replacement.
- Open Design MCP: required for AVE-13 UI design. Existing Aventor artifact fetched before UI edits; new AVE-13 launch-readiness artifact commissioned because existing artifact is admin wireframes/design-system only.
- ce:review / ECC review routing: run code/spec/API/security/testing/docs review before completion.
- Superpowers/TDD: write focused tests before implementation and run verification before claiming done.

## Graphify
- Backend graphify-out/GRAPH_REPORT.md: missing. Recorded; continuing with targeted source inspection.
- Frontend graphify-out/GRAPH_REPORT.md: missing. Recorded; continuing with targeted source inspection.

## Open Design provenance
- Existing artifact fetched: project a923a56a-c59f-44b6-9aa5-e476fbb1f066, "Aventor Design System (1)", entry admin/Admin Wireframes.html. Useful for Aventor tokens/admin primitives but not a launch-readiness chat artifact.
- New project: AVE-13-launch-readiness-ui, "AVE-13 Launch Readiness UI Design".
- New run: c47b02d4-66f3-44e4-9686-efd030b7ef2b.
- Studio: http://127.0.0.1:17573/projects/AVE-13-launch-readiness-ui/conversations/f053da60-3da4-47fb-8304-5cb43ed2f8c9
- UI phase status: Open Design generation running. Frontend substantive UI integration must remain blocked until artifact succeeds.

## Functional vs UI/Design Ownership Ledger
| Item | Owner/status | Notes |
| --- | --- | --- |
| Backend dual Google+Meta account readiness gate | Codex functional / in_progress | Must run before launch parameter draft persistence. |
| Backend conversion readiness warnings and structured decisions | Codex functional / in_progress | No deterministic regex/freeform parsing for switch/continue/pause. |
| Backend GBP optional readiness facts | Codex functional / pending | No hard blocker. |
| Launch-readiness chat/connect/select UI | Open Design artifact fetch/generation / running | Block as BLOCKED_DESIGN if OD run fails/unavailable. |
| Frontend structured response-option plumbing | Codex deterministic integration / pending | Only integrate Open Design-sourced labels/patterns; no Final Review redesign. |
| Aventor-Docs Google conversion + GBP guidance | Codex docs / pending | Narrow docs update only; no AVE-40 wizard claims. |

## Phase checklist
- [x] Read issue docs and root instructions.
- [x] Fetch existing Open Design artifact and commission AVE-13-specific artifact.
- [ ] Add focused failing backend tests.
- [ ] Implement provider readiness services/domain.
- [ ] Integrate pre-generation readiness before draft persistence.
- [ ] Poll OD artifact and integrate frontend only after success.
- [ ] Update Aventor-Docs.
- [ ] Run validation, reviewers, agent-browser QA, QA package.

## Validation log
- Pending AVE-13 tests/validation.

## Open Design update - BLOCKED_DESIGN for UI phase
- Run c47b02d4-66f3-44e4-9686-efd030b7ef2b failed with RATE_LIMITED: "You've hit your limit - resets 7:40pm (America/New_York)".
- Per linear-executor/TJ UI rules, substantive AVE-13 launch-readiness UI design/integration is blocked until Open Design generation is available or an approved artifact is provided.
- Backend functional work and docs update may continue; final AVE-13 issue cannot be called fully READY while UI phase is BLOCKED_DESIGN.

## Backend/docs implementation update
- Added launch-readiness domain at src/mastra/lib/launch-readiness.ts with dual Google+Meta account gate, conversion warnings, GBP optional snapshot, docs links, and structured switch/continue/pause decision application.
- Added shared Meta Pixel service at src/lib/meta/pixels.ts and route delegation using Authorization bearer headers instead of query-string access tokens.
- Added shared Google conversion readiness service at src/lib/google-ads/conversion-readiness.ts for AVE-13 preflight reuse.
- Integrated launch readiness into param_generate before budget allocation, parameter draft generation, and persistence.
- Added launchReadiness snapshot schema to campaign state and state deltas.
- Suppressed broad review advisory questions when launchReadiness facts exist.
- Updated launch prompt to align with backend-enforced Google+Meta blockers.
- Updated Aventor-Docs integrations/google-ads.mdx with conversion tracking readiness and GBP readiness anchors plus Meta Pixel link.

## Validation update
- npx tsx --test test/mastra/launch-readiness-preflight.test.ts: PASS (5 tests).
- npx tsx --test test/mastra/launch-readiness-delegate.test.ts: PASS (1 test).
- npm run typecheck: PASS.
- npm run build: PASS.
- npm test: PASS (1049 tests).
- Aventor-Docs python -m json.tool docs.json: PASS.

## Current blockers
- AVE-13 substantive UI remains BLOCKED_DESIGN because Open Design run c47b02d4-66f3-44e4-9686-efd030b7ef2b failed with RATE_LIMITED. Do not claim full READY until OD artifact can be created or supplied.

## 2026-06-06 Closeout Update

- Validation and review artifacts were generated under `qa-package/` and `qa-artifacts/`.
- Independent code/security/TypeScript review checklist completed; see `qa-artifacts/code-review.md`.
- agent-browser was used for local QA evidence.
- Backend/docs functional scope is green; UI and full browser QA remain `BLOCKED_DESIGN` until Open Design can generate/provide the launch-readiness artifact.
