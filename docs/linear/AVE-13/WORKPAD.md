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
| Backend selected-provider account readiness gate | Codex functional / implemented | Must run before launch parameter draft persistence; Meta-only requires Meta only, Google-only requires Google only, both selected requires both. |
| Backend conversion readiness warnings and structured decisions | Codex functional / implemented | No deterministic regex/freeform parsing for switch/continue/pause. |
| Backend GBP optional readiness facts | Codex functional / implemented | No hard blocker; Launch Agent handles optional guidance conversationally. |
| Google/Meta provider integration UI | Existing app patterns + Open Design secondary reference / implemented with browser advisory | Account review panel must appear every finalization and show selected providers only. Use existing connect/select recovery and match existing right-side light panel. |
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
- [x] Implement selected-provider account review panel, verify against existing right-side panel style, and capture agent-browser screenshots.

## Backend/docs implementation update
- Added launch-readiness domain at src/mastra/lib/launch-readiness.ts; revising it to selected-provider account gating plus conversion warnings, GBP optional snapshot, docs links, and structured switch/continue/pause decision application.
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


## 2026-06-07 Executor continuation
- Re-entered `linear-executor` flow after TJ requested implementation continuation.
- Re-read workpad, PLAN, EXECUTOR, QA, and current QA package.
- Found stale QA package state from pre-clarification `BLOCKED_DESIGN`; refreshed canonical backend QA package to `READY_WITH_SETUP_CHECKPOINTS`.
- Validation rerun:
  - `npx tsx --test test/mastra/launch-readiness-preflight.test.ts test/mastra/launch-readiness-delegate.test.ts`: PASS, 6 tests.
  - `npm run typecheck`: PASS.
  - `npm test`: PASS, 1049 tests.
  - `npm run build`: PASS.
  - `python -m json.tool docs.json > $null` in Aventor-Docs: PASS.
  - `git diff --check` in Aventor, Aventor-backend, and Aventor-Docs: PASS with LF-to-CRLF warnings only.
  - `agent-browser --version`: PASS, `agent-browser 0.26.0`.
- Browser QA setup checkpoint: both local frontend/backend AVE-13 worktrees have `.env.example` only and no `.env`; full authenticated/provider-state browser QA needs local env/auth and safe seeded Google/Meta/GBP states. This is recorded as setup checkpoint, not implementation blocker.
- Next action: commit/push refreshed backend QA package and updated code-review/workpad artifacts, then proceed to PR/Linear closeout if requested/available.


## 2026-06-07 PR closeout metadata
- Opened backend PR: https://github.com/Aventor-Apps/Aventor-backend/pull/54
- Opened frontend docs PR: https://github.com/Aventor-Apps/Aventor/pull/60
- Opened docs PR: https://github.com/Aventor-Apps/Aventor-Docs/pull/1
- Updated canonical QA checkpoint manifest with PR URLs.

## 2026-06-07 Selected-provider account review correction
- TJ clarified the account review gate is selected-provider scoped, not dual-platform mandatory: Meta-only requires Meta only, Google-only requires Google only, and both selected requires both.
- The right-side account review panel must appear on every Finalize Campaign entry, including when selected accounts are already connected, and requires explicit user confirmation before parameter generation.
- The account review panel temporarily replaces the media/design panel during launch prep, then the normal final review flow resumes after parameter generation.
- Conversion tracking, Meta Pixel readiness, GBP readiness, and switch/continue/pause questions remain Launch Agent conversational/structured-decision behavior, not right-panel readiness cards.
- Open Design artifact `AVE-13-launch-readiness-ui` was rechecked; it remains secondary reference only because it uses dark styling and stale dual-platform copy. Implementation must match the existing light `MainStudio`/`DesignPanel` right-panel pattern.
## 2026-06-08 Implementation closeout update
- Frontend implemented the selected-provider launch account review panel at `src/components/studio/LaunchAccountReviewPanel.tsx`, using the existing light right-side panel treatment and existing Google/Meta OAuth + account-selection modals.
- Frontend state now stores `launchAccountReview`; pending review takes precedence over generic finalization loading, suppresses final-review refresh while pending, restores on session reload, and sends structured `launchAccountReviewConfirmed: true` on `Continue with these accounts`.
- Backend direct `start_finalization` now evaluates selected-platform account readiness at `depth: "accounts"`, persists `phase: "account_connect"`, and emits `mode: "account_review_start"` before any parameter generation.
- Backend account-review confirmation now advances to `mode: "parameter_generation_start"` instead of reopening the account review panel, then runs full launch readiness/parameter generation.
- Backend stream keepalive now uses a safe controller wrapper so keepalive enqueue after stream close does not throw `ERR_INVALID_STATE`.
- Conversion tracking, Meta Pixel, GBP, and switch/continue/pause questions remain Launch Agent conversation/structured decision scope; no right-panel readiness cards were added.
- TDD additions: frontend account-review, typing, and campaign-restore tests; backend direct finalization/account review and safe-stream tests.
- Validation: frontend `npm run typecheck`, `npm run test:unit` (392 tests), `npm run lint`, and `npm run build` all PASS. Backend `npm run typecheck`, `npm test` (1055 tests), and `npm run build` all PASS. Docs `python -m json.tool docs.json > $null` PASS. `git diff --check` PASS in all three worktrees with LF-to-CRLF warnings only.
- Browser QA: local backend `http://127.0.0.1:3003/health` returned `{"ok":true}` and frontend `http://127.0.0.1:5175/` returned `200`; agent-browser captured Meta-only and Google-only account-review panel screenshots. Advisory: the full natural Finalize Campaign button click was not rerun after final non-visual fixes because the active browser profile was no longer authenticated; automated tests cover the direct request-context/state transition.
- QA artifacts refreshed under backend `docs/linear/AVE-13/qa-package/` and frontend `docs/linear/AVE-13/qa-artifacts/browser-qa.md`.

## 2026-06-08 Reviewer repair closeout
- Independent ECC prompt-backed reviewers were run via agent orchestration:
  - Bacon / code+TypeScript found P1 selected-media bypass; fixed in backend direct finalization routing and covered by `selected media request context does not bypass start_finalization account review`.
  - Parfit / security found P2 Meta Graph token-in-query transport; fixed by moving changed Graph helpers to `Authorization: Bearer` and covered by `test/metaGraphTokenTransport.test.ts`.
  - Carver / focused code+TypeScript re-review found a HIGH frontend precedence bug where pending account review could be hidden when `CampaignReview` was already visible; fixed by giving `LaunchAccountReviewPanel` unconditional pending-review precedence and covered by `test/launchAccountReview.test.ts`.
  - Singer / focused security re-review APPROVED the token fix and found no CRITICAL/HIGH issues.
  - Carver focused rerun APPROVED the panel precedence fix.
- Final validation after repairs:
  - Backend targeted `npx tsx --test test/metaGraphTokenTransport.test.ts test/routes/aventor-stream.test.ts test/mastra/launch-readiness-preflight.test.ts`: PASS, 35 tests.
  - Backend `npm run typecheck`: PASS; `npm test`: PASS, 1056 tests; `npm run build`: PASS.
  - Frontend targeted `npx tsx --test test/assistantTyping.test.ts test/launchAccountReview.test.ts test/campaignRestore.test.ts`: PASS, 34 tests.
  - Frontend `npm run typecheck`: PASS; `npm run test:unit`: PASS, 392 tests; `npm run lint`: PASS; `npm run build`: PASS with existing Vite warnings only.
  - Docs `python -m json.tool docs.json > $null`: PASS.
  - `git diff --check` in all three worktrees: PASS with LF-to-CRLF warnings only.
- QA package refreshed under backend `docs/linear/AVE-13/qa-package/` and code review artifact refreshed at `docs/linear/AVE-13/qa-artifacts/code-review.md`.
- Browser QA advisory remains: agent-browser captured selected-provider panel rendering and local server health, but full natural Finalize button-to-panel browser click was not rerun after final reviewer repairs because the active browser profile was no longer authenticated.
