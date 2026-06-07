# AVE-13 Codex Workpad

## Scope
- Issue: AVE-13 Launch Agent data-first readiness preflight + docs update.
- Backend worktree: C:\Users\kktho\code\aventor-workspaces\doc-rule-updates\AVE-13\Aventor-backend on branch AVE-13.
- Frontend worktree: C:\Users\kktho\code\aventor-workspaces\doc-rule-updates\AVE-13\Aventor on branch AVE-13.
- Docs worktree: C:\Users\kktho\code\aventor-workspaces\doc-rule-updates\AVE-13\Aventor-Docs on branch AVE-13.

## Loaded skills / obligations
- linear-executor: docs-first, workpad, TDD, Open Design only for substantive provider-integration UI beyond existing app patterns, QA package, reviewer gates.
- linear-reviewer: report-only QA artifacts and local browser QA target unless blocked.
- agent-browser: verify CLI and attach to CDP for browser QA; no ad hoc replacement.
- Open Design MCP: use for AVE-13 UI only if required Google/Meta integration/connect/select recovery needs substantive design beyond existing app patterns. If MCP/artifact creation/fetch is unavailable, block that UI phase.
- ce:review / ECC review routing: run code/spec/API/security/testing/docs review before completion.
- Superpowers/TDD: write focused tests before implementation and run verification before claiming done.

## Graphify
- Backend graphify-out/GRAPH_REPORT.md: missing. Recorded; continuing with targeted source inspection.
- Frontend graphify-out/GRAPH_REPORT.md: missing. Recorded; continuing with targeted source inspection.

## Open Design provenance and scope correction
- Existing artifact fetched: project a923a56a-c59f-44b6-9aa5-e476fbb1f066, "Aventor Design System (1)", entry admin/Admin Wireframes.html. Useful for Aventor tokens/admin primitives but not a launch-readiness chat artifact.
- Earlier Open Design run c47b02d4-66f3-44e4-9686-efd030b7ef2b failed with RATE_LIMITED.
- New project: AVE-13-launch-readiness-ui, "AVE-13 Launch Readiness UI Design".
- New completed run: 7a9edbe2-d8ba-4825-b284-8da7a7321b6f.
- Studio: http://127.0.0.1:17573/projects/AVE-13-launch-readiness-ui/conversations/f053da60-3da4-47fb-8304-5cb43ed2f8c9/files/launch-readiness.html
- Preview: http://127.0.0.1:17456/api/projects/AVE-13-launch-readiness-ui/raw/launch-readiness.html
- Artifact fetched with `get_artifact(project="AVE-13-launch-readiness-ui", entry="launch-readiness.html", include="all")`.
- Scope correction from TJ: only Meta and Google integration has an interface. Conversion tracking readiness, Meta Pixel readiness, GBP readiness, and switch/continue/pause questions are handled through the Launch Agent conversation and structured decisions, not the right panel.
- The generated artifact includes dark readiness-card concepts and is not directly applicable as-is. It may be used only as secondary reference for provider integration copy/structure if needed.
- Right-side panel visual source of truth is existing app code: `MainStudio.tsx` uses `bg-gray-50` for the right panel; `DesignPanel.tsx` uses a light `bg-gray-50` shell with `bg-white`/`bg-zinc-50` cards, gray borders, rounded cards/panels, and gray/black text. Any provider setup panel must match that media/info panel pattern.

## Functional vs UI/Design Ownership Ledger
| Item | Owner/status | Notes |
| --- | --- | --- |
| Backend dual Google+Meta account readiness gate | Codex functional / implemented | Runs before launch parameter draft persistence. |
| Backend conversion readiness warnings and structured decisions | Codex functional / implemented | No deterministic regex/freeform parsing for switch/continue/pause. |
| Backend GBP optional readiness facts | Codex functional / implemented | No hard blocker; Launch Agent handles optional guidance conversationally. |
| Google/Meta provider integration UI | Existing app patterns + Open Design only if needed / scoped | Only interface scope for AVE-13. Use existing connect/select recovery and match existing right-side light panel if touched. |
| Conversion/Meta Pixel/GBP readiness UI | Launch Agent conversation / not UI scope | Do not build right-panel readiness cards or Final Review UI. |
| Frontend structured response-option plumbing | Codex deterministic integration / unchanged unless needed | Response options must submit structured decisions; no regex parsing of freeform user text. |
| Aventor-Docs Google conversion + GBP guidance | Codex docs / implemented | Narrow docs update only; no AVE-40 wizard claims. |

## Phase checklist
- [x] Read issue docs and root instructions.
- [x] Fetch existing Open Design artifact and commission/fetch AVE-13-specific artifact.
- [x] Apply TJ scope correction: only Meta/Google integration has interface; conversion/GBP readiness remains Launch Agent conversational behavior.
- [x] Add focused failing backend tests.
- [x] Implement provider readiness services/domain.
- [x] Integrate pre-generation readiness before draft persistence.
- [x] Update Aventor-Docs.
- [x] Run validation, reviewers, agent-browser QA, QA package for backend/docs scope.
- [ ] If future provider integration UI edits are required, verify against existing right-side panel style and rerun browser QA.

## Backend/docs implementation update
- Added launch-readiness domain at src/mastra/lib/launch-readiness.ts with dual Google+Meta account gate, conversion warnings, GBP optional snapshot, docs links, and structured switch/continue/pause decision application.
- Added shared Meta Pixel service at src/lib/meta/pixels.ts and route delegation using Authorization bearer headers instead of query-string access tokens.
- Added shared Google conversion readiness service at src/lib/google-ads/conversion-readiness.ts for AVE-13 preflight reuse.
- Integrated launch readiness into param_generate before budget allocation, parameter draft generation, and persistence.
- Added launchReadiness snapshot schema to campaign state and state deltas.
- Suppressed broad review advisory questions when launchReadiness facts exist.
- Updated launch prompt to align with backend-enforced Google+Meta blockers and conversational conversion/GBP decisions.
- Updated Aventor-Docs integrations/google-ads.mdx with conversion tracking readiness and GBP readiness anchors plus Meta Pixel link.

## Validation update
- npx tsx --test test/mastra/launch-readiness-preflight.test.ts: PASS (5 tests).
- npx tsx --test test/mastra/launch-readiness-delegate.test.ts: PASS (1 test).
- npm run typecheck: PASS.
- npm run build: PASS.
- npm test: PASS (1049 tests).
- Aventor-Docs python -m json.tool docs.json: PASS.

## Current blockers / open UI note
- No current `BLOCKED_DESIGN` remains for conversion/GBP readiness because TJ clarified those are not UI scope.
- If the implementation later requires substantive Google/Meta provider integration UI beyond existing app patterns, use Open Design MCP per `linear-executor`; if the MCP/artifact cannot be used, mark only that provider-UI phase `BLOCKED_DESIGN`.
- Do not claim a provider UI change complete without browser QA against the local target.


## 2026-06-06 UI scope correction review
- Updated AVE-13 docs after TJ clarified only Meta/Google integration has an interface.
- Conversion tracking, Meta Pixel readiness, GBP readiness, and switch/continue/pause questions remain Launch Agent conversational/structured-decision behavior and are not right-panel readiness cards.
- Right-side provider setup information, if touched later, must match the existing light `MainStudio`/`DesignPanel` media/info panel style.
- Docs consistency review: no stale "launch-readiness UI design must use Open Design" wording remains; remaining Open Design references are conditional provider-UI-only instructions.
- Validation: `git diff --check` passed in Aventor, Aventor-backend, and Aventor-Docs; mirrored docs match across all three worktrees; `python -m json.tool docs.json` passed in Aventor-Docs.

## 2026-06-06 Closeout Update

- Validation and review artifacts were generated under `qa-package/` and `qa-artifacts/`.
- Independent code/security/TypeScript review checklist completed; see `qa-artifacts/code-review.md`.
- agent-browser was used for local QA evidence.
- Backend/docs functional scope is green. UI scope is narrowed to existing Google/Meta provider integration recovery; conversion/GBP readiness is Launch Agent conversational behavior and should not be implemented as right-panel readiness cards.
