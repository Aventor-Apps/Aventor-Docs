# AVE-13 QA Plan

## QA goal

Verify that the launch-agent pre-generation flow is data-first, always opens selected-provider account review before parameter generation, blocks only missing setup for selected Google/Meta providers, keeps conversion/GBP readiness gaps conversational through the Launch Agent, preserves Final Review, avoids new readiness cards/panels for conversion/GBP, and includes the docs update. Only Google/Meta integration/connect/select recovery has AVE-13 UI scope.

## Acceptance-criteria coverage matrix

| AC | Requirement | Primary evidence | Secondary evidence |
|---|---|---|---|
| 1 | Selected-provider account review appears every finalization and missing selected Google/Meta connection/selection blocks before drafts persist; unselected providers do not block | Backend/frontend tests around `start_finalization` and `param_generate`; no draft persistence in blocked cases | Manual chat flow with mocked/seeded Meta-only, Google-only, and both-provider account states |
| 2 | Meta Pixel + Google conversion readiness fetched from account data | Service tests mocking Meta Graph + Google Ads GAQL responses | Code review showing shared services reused by route/preflight |
| 3 | Unknown/unavailable distinct from missing | Tests for API timeout/permission/provider error | Logs show safe reason codes, no token/error body leakage |
| 4 | Conversion gaps warn and ask switch/continue/pause | Tests for conversion-dependent goals; chat response options | Manual flow confirming options and docs links |
| 5 | Structured switch/continue/pause semantics | Tests verifying structured decision payload application, goal mutation, accepted warning, pause/no-draft behavior, the preferred two-turn goal-switch flow, and no backend regex parsing of freeform user text | QA package summary with state transition notes |
| 6 | GBP readiness fetched and non-blocking | Tests for selected/missing/unknown GBP | Manual chat copy if GBP missing |
| 7 | Google/Meta provider integration UI only; conversion/GBP and switch/continue/pause handled by Launch Agent structured decisions; Final Review avoids unapproved redesign/card scope | Workpad scope notes, frontend diff/tests only if provider UI touched | Browser/manual check of existing connect/select recovery and right-side panel style if touched |
| 8 | Docs update included | Docs diff and link validation | Backend constants/tests reference final docs paths |
| 9 | No schema/live mutation unless reviewed | Git diff has no migration or reviewer notes if migration added | Supabase/schema reviewer result if needed |
| 10 | Final QA package exists | Four qa-package files committed | PR/Linear update links package |

## Executor validation checklist

### Backend (`aventor-backend`)

Run exact available commands; if repo test runner syntax differs, record the substitution in `qa-package/validation_results.md`.

```bash
npm run typecheck
npm run build
npm test
```

Focused tests expected to exist/pass after implementation:

```bash
npm test -- test/mastra/launch-readiness-preflight.test.ts
npm test -- test/mastra/validate-account-readiness.test.ts test/routes/google-business-profile.test.ts
npm test -- test/routes/google-launch-parameter-contract.test.ts test/routes/facebook-launch-parameter-contract.test.ts
```

If helper service tests are created under `test/lib/`, include them explicitly in validation results.

### Frontend (`aventor`), only if touched

```bash
npm run typecheck
npm run lint
npm run build
npm run test:unit
```

Focused checks if frontend touched:

- Selected-provider account review opens on every Finalize Campaign entry and account connection/select blocker recovery still works.
- Switch/continue/pause response options render and submit structured decisions using existing option flow; no frontend/backend regex parsing of freeform text is introduced.
- Only selected-provider Google/Meta connect/select recovery has UI scope; conversion/GBP warnings and switch/continue/pause remain Launch Agent conversational/structured-decision behavior, with no new readiness card/panel or Final Review redesign.

### Docs (`Aventor-Docs`)

Use exact repo-supported docs validation if available. Baseline did not show `package.json`, so minimum checks are:

- `docs.json` remains valid JSON if edited.
- Edited `.mdx` files parse enough for local static inspection.
- Internal links match backend docs targets:
  - `/integrations/pixel-setup`
  - `/integrations/google-ads#conversion-tracking-readiness`
  - `/integrations/google-ads#google-business-profile-readiness`
- No future AVE-40 wizard is represented as already shipped.

## Manual / browser QA flows

Browser artifacts are optional by default. Use browser checks only when the implementation PR is ready and a runnable local/preview environment exists.

### Flow 1 — Missing Google blocks

Setup: authenticated user/session with Meta selected but no Google Ads customer/connection.

Steps:

1. Start/continue campaign intake to finalization.
2. Request Finalize Campaign / parameter generation.
3. Observe launch-agent response.

Expected:

- Generation does not proceed.
- No platform parameter drafts are persisted.
- Launch Agent names Google Ads setup as required and the right-side account review panel directs connect/select action.
- Existing account connection/select UI can recover.

### Flow 2 — Missing Meta blocks

Same as Flow 1 but Google ready and Meta missing/unselected.

Expected:

- Generation is blocked for the selected Meta campaign.
- Copy names Meta setup and the right-side account review panel directs connect/select action.
- No drafts persist.

### Flow 2B — Both required platforms missing

Setup: authenticated user/session with neither selected Google Ads customer nor selected Meta ad account/page.

Expected:

- Generation is blocked.
- Launch Agent names both missing selected platforms in one response and the account review panel shows both providers.
- No drafts persist.
- User is not forced through a sequential Google-then-Meta failure loop.

### Flow 3 — Tracking missing with leads/conversions/sales goal

Setup: Google + Meta accounts ready; no Meta Pixel and/or no Google conversion action readiness; campaign goal is `leads`, `conversions`, or `sales`.

Expected:

- Launch Agent warns; does not ask a generic “do you have tracking?” if data shows missing.
- Response options allow switch goal, continue with warning, pause for setup, and submit stable structured decision payloads.
- Meta warning links `/integrations/pixel-setup`.
- Google warning links the updated Google docs target.

Sub-checks:

- Switch goal: active goal changes; next deterministic generation turn proceeds from the safer goal without duplicate drafts.
- Continue: original goal remains; warning appears in summary/context; generation proceeds.
- Pause: generation remains paused/resumable; no drafts persist.

### Flow 4 — Tracking unknown/API unavailable

Setup: mock/provider failure for conversion readiness.

Expected:

- Copy says Aventor could not confirm readiness, not that tracking is missing.
- User can continue if they know tracking is ready or pause for setup; Mastra owns natural-language interpretation if the user types instead of clicking an option.
- No token/provider error body shown.

### Flow 5 — GBP selected/missing

Setup A: saved GBP location exists.

Expected:

- No broad GBP yes/no question.
- Selected location can be used as readiness fact where applicable.

Setup B: no GBP selection or missing scope.

Expected:

- Optional conversational guidance only.
- No hard blocker.
- GBP guidance appears conversationally through the Launch Agent only; no new GBP readiness card/panel appears in the right panel or Final Review.

### Flow 6 — Docs links

Steps:

1. Open Meta Pixel docs link from warning.
2. Open Google conversion/GBP docs link from warning.

Expected:

- Links resolve to correct docs pages/anchors.
- Docs explain setup/readiness accurately and do not claim the future wizard is shipped.

## Mastra response-interpretation QA guard

- Deterministic/backend code must not regex-parse freeform user text for switch/continue/pause intent.
- Mastra/orchestrator must own natural-language interpretation and emit structured `finalizationDecision` / `finalizationDecisions` payloads.
- Tests/reviewer checks should inspect the diff for regex/string intent parsing and verify only structured decision IDs/actions are applied by deterministic code.

## Data/API verification

- Provider calls are server-side only.
- Tokens/access tokens/refresh tokens never appear in state, logs, test fixtures, docs, Linear comments, or QA package.
- Fetch failures classified safely.
- Google conversion readiness service still supports launch-time guard behavior.
- Meta Pixel service returns consistent shape for route and preflight and does not log/expose access tokens.
- GBP readiness reads existing `google_business_profile_selections` data safely.

## UI / accessibility checks

Required only if AVE-13 touches visible provider integration UI. Conversion tracking, Meta Pixel, GBP, and switch/continue/pause decisions are Launch Agent conversation/structured decisions and are not right-panel readiness cards.

If visible Google/Meta provider UI is touched:

- Response options are keyboard accessible through existing components.
- Button labels are concise and not ambiguous.
- Warning copy is readable, not alarmist.
- Existing account connection/select recovery remains usable.
- Any right-side panel provider information matches the current media/info panel style in `MainStudio.tsx` and `DesignPanel.tsx`: light `bg-gray-50` shell, white/zinc-50 cards, gray borders, rounded panels, and gray/black text.
- Do not apply the dark Open Design artifact background to the right-side panel.
- No unapproved conversion/GBP readiness card, right-panel warning surface, or Final Review redesign is introduced.
- If substantive provider-integration UI design is needed beyond existing patterns, route it through local Open Design per `linear-executor`; if Open Design MCP/artifact fetch/artifact creation is unavailable, mark `BLOCKED_DESIGN` and stop for that UI phase.

## Regression areas

- Finalization budget/media/learning blockers still fire correctly.
- Google website URL blocker still works.
- Review Agent advisory flow does not ask stale broad pixel/GBP questions when data facts exist.
- Existing Google launch-time conversion readiness behavior remains intact.
- Existing Meta/Google launch parameter contract tests still pass.
- Existing OAuth connection/selection flows still work.

## Required evidence

Executor must include in `qa-package/validation_results.md`:

- exact commands run;
- workdir for each command;
- pass/fail status;
- focused test list and results;
- reviewer gates and outcomes;
- any command substitutions/caveats;
- docs validation performed.

Executor must create the canonical QA package in the backend repo. Executor must include in `qa-package/checkpoint_manifest.md`:

- repos, branches, PR URLs, commit SHAs;
- scope summary;
- acceptance criteria status;
- artifact index;
- next gate.

Executor must include in `qa-package/qa_readiness_result.json` a machine-readable verdict:

- `READY` if all code/docs validation and reviewers pass;
- `READY_WITH_SETUP_CHECKPOINTS` if browser QA needs provider sandbox/manual account setup;
- `BLOCKED_SETUP` if QA cannot run due missing env/provider setup;
- `BLOCKED_IMPLEMENTATION` if code/reviewer/validation blockers remain.

## Browser QA prompt requirement

`docs/linear/AVE-13/qa-package/browser_qa_prompt.md` must be paste-ready for TJ/Review Companion and include:

- report-only mode: no code patches, commits, pushes, DB/Supabase mutations, Linear Done transition, or secrets in reports;
- setup/readiness gate before browser steps;
- exact flows listed above;
- expected behavior and stop/fail conditions;
- evidence required;
- statement that screenshots/video are optional unless needed for a finding.

## Final readiness gate

Do not claim AVE-13 implementation is complete until:

- required tests pass;
- relevant reviewer gates pass or blocking findings are fixed;
- docs update is complete and links match backend copy;
- QA package exists with all four files;
- PR(s) are open and linked;
- Linear update includes repo/branch/PR/docs/QA package references.
