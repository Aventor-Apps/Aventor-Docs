# AVE-13 — Launch Agent data-first readiness preflight + docs update

> **Execution mode:** TJ local-Codex branch handoff. Use `linear-executor` + `ce:work`. This package is intended to be committed under `docs/linear/AVE-13/` on the AVE-13 branches.

**Linear issue:** AVE-13 — Google and Meta integration during the launch agent and other blockers to launch agent
**Primary repo:** `Aventor-Apps/Aventor-backend` at `/home/openclawsvc/repos/aventor-backend`
**Secondary repos:** `Aventor-Apps/Aventor` at `/home/openclawsvc/repos/aventor`; `Aventor-Apps/Aventor-Docs` at `/home/openclawsvc/repos/Aventor-Docs`
**Branch:** `AVE-13` in every touched repo
**Docs root:** `docs/linear/AVE-13/`

## Problem / goal

Aventor currently asks broad readiness questions around conversion tracking and Google Business Profile (GBP) instead of first checking connected Google/Meta account data. AVE-13 should move readiness into a data-first pre-generation preflight owned by the launch-agent flow.

The implementation must:

1. Require both Google Ads and Meta account connection/selection before campaign parameter generation proceeds.
2. Fetch conversion readiness from connected accounts when possible: Meta Pixel + Google conversion actions/goals.
3. Fetch GBP readiness/selection when possible.
4. Let the launch agent ask only targeted follow-up decisions after data inspection.
5. Include a small Aventor-Docs update so warnings link to accurate docs.

## User story

As a campaign creator, when I ask Aventor to prepare a campaign for review, I want the launch agent to check my connected Google/Meta/GBP account readiness first so I am blocked only for required account setup, warned clearly about conversion/GBP gaps, and can choose whether to switch goals, continue with warnings, or pause to set up tracking.

## Acceptance criteria

1. **Required dual-platform gate:** before parameter drafts persist, backend verifies required Google Ads + Meta connections/selections. Missing/invalid/unselected Google or Meta blocks the whole generation flow, not just one platform. **Product decision:** this applies even if the user appears to be pursuing a single-platform campaign; Aventor's current generation model assumes Google + Meta readiness as the baseline.
2. **Data-first conversion checks:** after required accounts exist, backend fetches Meta Pixel and Google conversion action/goal readiness. Do not ask “do you already have tracking?” when data answers it.
3. **Unknown is not missing:** provider/API/permission errors produce `unknown`/`unavailable` readiness and targeted fallback questions, not false “missing” facts.
4. **Conversion gaps warn, not block:** after required accounts exist, missing/unknown tracking does not hard-block generation. For conversion-dependent goals (`conversions`, `leads`, `sales`, or the repo's normalized campaign-goal/objective equivalents), launch agent warns and asks whether to switch goal, continue with warning, or pause for setup.
5. **Goal-switch semantics:** structured switch/continue/pause decisions mutate or preserve state idempotently. Mastra owns natural-language interpretation and emits structured decisions; deterministic/backend code must not regex-parse freeform user responses to decide intent.
6. **GBP is optional enrichment:** fetch saved GBP readiness/selection when the campaign/business context is local-relevant or an existing GBP selection exists; selected GBP becomes a positive fact. Missing/unknown/missing-scope GBP is conversational optional guidance, not a blocker. For clearly non-local campaigns with no GBP signal, mark GBP `not_applicable` and avoid irrelevant chatter.
7. **Provider integration UI only:** AVE-13 only has an interface for the required Google Ads and Meta integration/connect/select recovery. Conversion tracking readiness, GBP readiness, and switch/continue/pause decisions are Launch Agent conversational/structured-decision behavior, not new right-panel readiness cards. If the right-side panel is touched for provider setup information, match the existing light media/info panel design in `MainStudio`/`DesignPanel`; Open Design is secondary reference and must not override that existing panel background/style.
8. **Docs update:** Aventor-Docs gets narrow Google conversion tracking + GBP readiness guidance. Meta warnings link to `/integrations/pixel-setup`. Do not implement the AVE-40 setup wizard.
9. **Safety:** no live Supabase mutation by Planner. Prefer no schema migration. If implementation requires schema/RPC/policy changes, run schema/security review and do not apply live migration without approval.
10. **QA package:** executor must create `docs/linear/AVE-13/qa-package/{checkpoint_manifest.md,browser_qa_prompt.md,validation_results.md,qa_readiness_result.json}` before final PR/Linear closeout.

## Decisions from grill log

- Data first: inspect connected account data before asking user readiness questions.
- Run preflight before generation/review; no final-launch revalidation added in AVE-13.
- Missing Google or Meta required connection/ad-account readiness blocks the whole generation flow.
- Product decision: both Google + Meta required setup is mandatory before this pre-generation flow, even if a selected-platform subset appears single-platform. If single-platform generation becomes a product path, capture it as separate scope.
- Missing conversion tracking after both platforms are connected warns and asks whether to switch goal.
- Switch / continue-with-warning / pause semantics are in scope.
- Setup/install wizard is out of scope and tracked by AVE-40.
- Conversion/GBP warnings happen conversationally; no new readiness UI card/panel.
- GBP scope strategy: if GBP OAuth scope is not granted, treat GBP readiness as `unknown`/`unavailable` or `not_applicable` depending on local relevance, not as a hard missing setup fact. Do not proactively implement a GBP scope prompt/wizard in AVE-13.
- AVE-13 includes a small Aventor-Docs update.

## Context inspected

### Linear and memory

- AVE-13 live issue in Aventor Backlog, labels `Migrated` + `Grooming`.
- Source CTC-128 only contained the original migrated note about fetching Google/Meta data before generation.
- AVE-40 exists as non-blocking follow-up for conversion tracking setup wizard.
- Hindsight/Obsidian/project notes and working `BRAINSTORM.md` were used.

### Git / repo baseline

- Frontend `aventor`: remote `https://github.com/Aventor-Apps/Aventor.git`, `origin/main` `936d119a2d4c06c1deb8f63cf960e00ab8d07bed`; current checkout is unrelated branch with untracked graph/docs artifacts.
- Backend `aventor-backend`: remote `https://github.com/Aventor-Apps/Aventor-backend.git`, `origin/main` `c66afd27dc37861dc8294bb00ce9e5157b1110fb`; current checkout is unrelated AVE-32 branch.
- Docs `Aventor-Docs`: remote `https://github.com/Aventor-Apps/Aventor-Docs.git`, `origin/main` `d353f10201668caff2e5011c3259769b9a2fd599`; current checkout detached.
- Final branch handoff should use `AVE-13` branches from `origin/main`, not the unrelated current worktrees.

### Backend source anchors

- Launch agent: `src/mastra/agents/launch-agent.ts`, `src/mastra/prompts/launch.ts`.
- Deterministic finalization: `src/mastra/tools/delegate-campaign-phase.ts`.
- Current account readiness: `src/mastra/tools/validate-account-readiness.ts`, `get-google-connection.ts`, `get-facebook-connection.ts`.
- Current broad advisories: `src/mastra/lib/review-advisories.ts`, `src/mastra/lib/specialist-output.ts`, `src/mastra/lib/campaign-state.ts`.
- Existing Meta Pixel route: `src/routes/facebook-get-pixels.ts` (`/facebook_get_pixels`).
- Existing Google conversion readiness: private helpers in `src/routes/google-launch-campaign.ts` (`queryGoogleConversionReadiness`, `requireGoogleSearchConversionReadiness`).
- Existing GBP REST flow: `src/routes/google-business-profile.ts` registered in `src/app.ts`.
- Relevant tests: `test/mastra/check-readiness.test.ts`, `test/mastra/validate-account-readiness.test.ts`, `test/routes/google-business-profile.test.ts`, Google/Facebook parameter and launch tests.

### Frontend source anchors

- API client: `src/utils/api.ts`.
- Existing launch account UI: `src/components/dashboard/CampaignReview.tsx`, `src/components/modals/LaunchConnectionConfirmationModal.tsx`.
- Relevant tests: campaign review, Google launch pipeline, GBP, Facebook OAuth/config, final review tests.

### Docs anchors

- `Aventor-Docs/docs.json`.
- `integrations/pixel-setup.mdx` exists and is the Meta docs target.
- `integrations/google-ads.mdx` currently needs Google conversion/GBP readiness content.

## Recommended approach

### Mastra-owned response interpretation

Natural-language user response interpretation must stay Mastra-owned. Deterministic/backend code must not parse freeform user text with regex/string matching to decide `switch`, `continue`, `pause`, or similar intent. The orchestrator/Launch Agent should convert user language or button selections into structured `finalizationDecision` / `finalizationDecisions` payloads; deterministic code may validate, persist, and apply only those structured decisions by stable IDs/actions.

Build a deterministic backend launch-readiness domain and integrate it into `param_generate` before platform draft generation. Keep provider facts server-side; store only safe readiness statuses/docs URLs in campaign/session state. Use launch-agent copy for the user-facing conversation.

Recommended new/refactored pieces:

- `src/lib/facebook/pixels.ts` or `src/lib/meta/pixels.ts`: shared Meta Pixel readiness service used by `/facebook_get_pixels` and preflight.
- `src/lib/google-ads/conversion-readiness.ts`: extract/reuse Google Ads conversion action/goal GAQL logic from `google-launch-campaign.ts`.
- `src/lib/google-business-profile/readiness.ts`: server-side helper to read saved GBP selection/readiness from existing data.
- `src/mastra/lib/launch-readiness.ts`: combines required connection checks, conversion readiness, GBP readiness, docs links, warning/blocker classification, and decision application. Note: `src/mastra/lib/readiness.ts` already exists for intake/business-context readiness; keep the AVE-13 launch-readiness domain separate to avoid import/name confusion.
- Optional `src/mastra/tools/get-launch-readiness.ts`: only if a Mastra tool boundary improves specialist context; deterministic code should still own blocker truth.
- Updates to `campaign-state.ts` for an optional `launchReadiness` snapshot and any needed question/decision identifiers.
- Updates to `delegate-campaign-phase.ts` so readiness blockers/warnings happen before draft generation.
- Updates to `launch.ts` prompt so the launch agent writes from backend-provided readiness facts and can explain required Google/Meta blockers.

## Implementation phases

### Phase 0 — Branch/handoff repair

**Owner:** Local Codex.
**Repos:** all three.

- Fetch latest `origin/main` in backend, frontend, docs repos; create/reuse `AVE-13` branch from `origin/main` unless TJ explicitly says to reuse another implementation branch.
- Re-verify key source anchors from this plan before editing, because planning-time baseline SHAs may be stale.
- Read handoff docs and repo `AGENTS.md`.
- Repair branch-local `AGENTS.md` files so they reference AVE-13. Current backend/frontend baseline `AGENTS.md` references old CTC-138 and must not remain stale on AVE-13 branches.
- Create a small docs-repo `AGENTS.md` if absent.

### Phase 1 — Tests-first readiness contract

**Owner:** Local Codex backend.

Add tests before implementation, e.g. `test/mastra/launch-readiness-preflight.test.ts`, focused provider service tests under `test/lib/`, and any route tests needed for extracted services. Cover:

- missing Google connection/customer blocks whole generation;
- missing Meta connection/ad account blocks whole generation;
- both accounts ready + missing Meta Pixel warns for conversion-dependent goals;
- missing Google conversion action/goal warns with Google docs link;
- Meta Pixel + Google conversion ready yields no conversion warning;
- API unavailable/permission-limited results are `unknown`/`unavailable`;
- GBP selected vs missing/unknown behavior;
- structured switch goal / continue / pause decision semantics, with Mastra owning natural-language response interpretation;
- existing launch-time Google conversion readiness still passes;
- both Google + Meta missing are surfaced together, not sequentially;
- single-platform-selected state still follows TJ's dual-platform readiness decision.

### Phase 2 — Extract shared provider readiness services

**Owner:** Local Codex backend.

- Extract Meta Pixel fetch/readiness from `facebook-get-pixels.ts` into shared service; route delegates to service. Improve/maintain token safety during extraction: do not log tokens, and prefer `Authorization: Bearer` over query-string access tokens where the Meta API contract allows it.
- Extract Google conversion readiness from `google-launch-campaign.ts` into shared service; launch route preserves current launch behavior by delegating to service and tests pin `queryGoogleConversionReadiness` / `requireGoogleSearchConversionReadiness` semantics.
- Add GBP readiness helper that reads saved selection and distinguishes selected, missing, missing scope, unknown/fetch failed.
- Ensure services never log tokens and expose only safe IDs/statuses.

### Phase 3 — Launch-readiness domain and state

**Owner:** Local Codex backend.

- Implement `launch-readiness` evaluator returning safe statuses for required connections, conversion tracking, GBP, blockers, warnings, docs links, and checked-at/version metadata.
- Reuse existing `finalizationBlockerSchema` / `finalizationDecisionsSchema` where possible. Use a clear `launch_readiness.*` blocker/warning ID prefix. Add new state/question IDs only where necessary.
- Keep readiness facts separate from launch-agent copy.
- Prefer no DB migration; persist in campaign/session state only if turn-to-turn durability is needed.

### Phase 4 — Pre-generation integration

**Owner:** Local Codex backend.

- Run launch-readiness preflight in `runDeterministicParamGeneration()` before parameter draft generation/persistence.
- If required Google/Meta account readiness fails, return blocking finalization state.
- If conversion readiness is missing/unknown for conversion-dependent goals, return a warning decision group with switch / continue / pause options.
- Do not add deterministic regex/string parsing of user utterances for those options. Mastra/orchestrator must interpret natural language or button selections into structured `finalizationDecision` / `finalizationDecisions` payloads; deterministic code only validates and applies those stable decision IDs/actions.
- Apply structured decisions idempotently:
  - switch: mutate campaign goal, clear/supersede the conversion warning, and let the next deterministic turn generate from the updated goal rather than building a fragile in-turn recursive rerun;
  - continue: mark warning accepted and proceed;
  - pause: stay waiting and persist no drafts.
- Update launch prompt to align with new backend truth. Replace the current "do not ask the user to connect Meta or Google during Finalize Campaign" instruction with: "Required Google Ads and Meta account connection/selection is enforced by backend preflight before parameter generation. If a required account blocker is active, explain which account setup is needed and direct the user to the existing connect/select UI. Do not proceed with generation until required account blockers are resolved."
- Replace/bypass broad advisory questions when readiness facts already answer them. If required account blockers are active, suppress marketing-advisory pixel/GBP questions for that turn so the user sees one coherent setup blocker set.

### Phase 5 - Meta/Google integration UI and frontend contract plumbing

**Owner:** Codex for deterministic contract/state/tests; Open Design only for provider integration UI if existing patterns are insufficient.

- Scope the frontend work to required Google Ads + Meta integration/connect/select recovery only.
- Do not add deterministic readiness cards, right-panel warning cards, or Final Review redesign for conversion tracking, Meta Pixel, GBP, or switch/continue/pause questions. Those topics are handled by the Launch Agent conversation using backend facts and structured decisions.
- Inspect existing chat response-option plumbing and the existing account connect/select flows before editing. Verify options submit structured `finalizationDecision` / `finalizationDecisions` payloads; do not implement frontend/backend regex parsing of freeform user text.
- If provider setup information needs to appear in the right-side panel, use the existing media/info panel as the visual source of truth: `MainStudio.tsx` wraps the right panel in `bg-gray-50`; `DesignPanel.tsx` uses a light `bg-gray-50` shell with `bg-white`/`bg-zinc-50` cards, gray borders, rounded panels, and gray/black text. Do not apply the dark Open Design artifact background to this panel.
- Use Open Design MCP only when substantive provider-integration UI design is needed beyond existing app patterns. Fetch an existing relevant artifact with `get_artifact()` when available; otherwise create/commission one (`create_project` if needed, `start_run`, poll `get_run`, then `get_artifact`). If the MCP is unavailable or a required artifact cannot be created/fetched, mark `BLOCKED_DESIGN` and stop for that UI phase.
- Record Open Design artifact/run provenance, inspected existing panel/component files, integration scope, and blocker status in the workpad before editing visible UI.
- Update `src/utils/api.ts` types only if a frontend-visible route/response is introduced.
- Final Review remains unchanged.

### Phase 6 — Aventor-Docs update

**Owner:** Local Codex docs phase.

Recommended minimal docs change:

- Update `integrations/google-ads.mdx` in-place with sections for Google Ads connection/customer selection, conversion tracking/conversion actions readiness, and GBP readiness.
- Create exact anchors `#conversion-tracking-readiness` and `#google-business-profile-readiness`, or update backend constants/tests if the final anchors intentionally differ.
- Keep Meta warning link to existing `integrations/pixel-setup.mdx`.
- Do not add a new docs page for AVE-13; `docs.json` should not need changes because `integrations/google-ads` is already registered.
- Clarify setup wizard is future work; do not promise AVE-40 behavior as shipped.

### Phase 7 — Validation, reviewers, QA package, PRs

**Owner:** Local Codex orchestration.

Backend validation:

```bash
npm run typecheck
npm run build
npm test
```

Frontend validation if frontend touched:

```bash
npm run typecheck
npm run lint
npm run build
npm run test:unit
```

Docs validation:

- Use repo-supported Mintlify/docs command if available after branch setup.
- At minimum verify `docs.json` JSON if touched, MDX/Markdown syntax for edited docs, and internal docs links/anchors. Cross-check backend docs-link constants against the edited docs anchors.

Run independent reviewers (see `REVIEWERS.md`), repair blocking findings, then create final QA package before PR/Linear closeout.

## Likely files touched

### Backend

- `AGENTS.md`
- `docs/linear/AVE-13/*`
- `src/mastra/agents/launch-agent.ts`
- `src/mastra/prompts/launch.ts`
- `src/mastra/tools/delegate-campaign-phase.ts`
- `src/mastra/tools/validate-account-readiness.ts`
- `src/mastra/lib/launch-readiness.ts` (new)
- `src/mastra/lib/campaign-state.ts`
- `src/mastra/lib/review-advisories.ts`
- `src/mastra/lib/specialist-output.ts`
- `src/lib/facebook/pixels.ts` or `src/lib/meta/pixels.ts` (new extraction)
- `src/lib/google-ads/conversion-readiness.ts` (new extraction)
- `src/lib/google-business-profile/readiness.ts` (new/helper, optional path)
- `src/routes/facebook-get-pixels.ts`
- `src/routes/google-launch-campaign.ts`
- Tests in `test/mastra/`, `test/lib/`, `test/routes/`.

### Frontend

- `AGENTS.md`
- `docs/linear/AVE-13/*`
- `src/utils/api.ts` only if contract exposed.
- `src/components/dashboard/CampaignReview.tsx` only if existing response option flow needs non-visual structured-decision alignment.
- `src/components/modals/LaunchConnectionConfirmationModal.tsx` only if existing blocker recovery needs non-visual plumbing.
- Focused `test/*` updates if frontend touched.

### Docs

- `AGENTS.md` if absent/stale.
- `docs/linear/AVE-13/*`
- `integrations/google-ads.mdx`
- Optional small cross-link in `integrations/pixel-setup.mdx`.
- `docs.json` only if executor intentionally adds a new docs page despite the recommended in-place update.

## Data / API / schema considerations

- Do not expose tokens or sensitive account credentials to frontend, Linear, docs, logs, or QA package.
- External provider calls must have safe error handling and status classification.
- No new schema expected. If executor proposes schema, trigger schema/security review first.
- Preserve launch-time Google conversion safety in `google-launch-campaign.ts` while adding pre-generation warning behavior.
- Adding a new readiness status such as `not_applicable` is a TypeScript/Zod contract change; keep it internal where possible and include schema/API-contract reviewer attention if it becomes persisted/frontend-visible.
- Accepted warning decisions must be idempotent across retries and not re-prompt every turn.
- In-flight sessions with old `reviewAdvisoryState` should not crash.

## UI / UX considerations

- Only required Google Ads and Meta integration/connect/select recovery has an interface in AVE-13.
- Conversion readiness, Meta Pixel readiness, GBP readiness, and switch/continue/pause questions are Launch Agent conversational behavior backed by structured Mastra decisions. They should not be implemented as right-panel readiness cards or Final Review UI.
- Account setup blockers: clear, short Launch Agent copy plus existing connection/select UI.
- If provider setup information is surfaced in the right-side panel, match the existing media/info panel design: light `bg-gray-50` right-panel shell, `bg-white` or `bg-zinc-50` information cards, gray borders, rounded panels, and gray/black typography from `MainStudio.tsx` and `DesignPanel.tsx`.
- The generated Open Design artifact can be used only as secondary provider-integration reference; it is not authoritative for the right-side panel background because the app already has a light panel pattern.
- Warning option copy should be short in the Launch Agent: switch goal, continue with warning, pause for setup.
- Missing GBP copy should explain optional benefit for local campaigns conversationally without blocking.
- Final Review remains unchanged unless TJ explicitly expands scope.
- Browser screenshots/video are optional by default.

## Risks / unknowns

- Google conversion readiness extraction could accidentally change launch-time behavior; tests must pin existing semantics.
- Prompt currently says not to ask for Meta/Google connection during Finalize Campaign; use the exact replacement direction in Phase 4 to avoid contradiction.
- External APIs can be flaky/rate-limited; unknown-state handling is important.
- Current root `AGENTS.md` files are stale; branch handoff should repair them.
- Docs repo lacks visible package scripts; validation may require static checks.

## Suggested internal subtasks

Do not create Linear subtasks unless TJ explicitly says `create the subtasks`.

1. Backend services/tests.
2. Backend pre-generation integration/state/prompt.
3. Frontend contract plumbing if needed.
4. Docs update.
5. Validation + reviewers + QA package.

## Source docs included in final handoff

- `docs/linear/AVE-13/PLAN.md`
- `docs/linear/AVE-13/BRAINSTORM.md`
- `docs/linear/AVE-13/ARCHITECTURE.md`
- `docs/linear/AVE-13/QA.md`
- `docs/linear/AVE-13/EXECUTOR.md`
- `docs/linear/AVE-13/REVIEWERS.md`
- `docs/linear/AVE-13/LINEAR_UPDATE.md`
- `docs/linear/AVE-13/source_bundle/context_manifest.md`

## Planner review notes

Planner review gates were run in CEO/product, engineering, and DevEx modes. Verdict: `CLEAR_WITH_CHANGES`; required doc clarifications were incorporated before branch handoff. Design review/Open Design is required only if AVE-13 changes the visible Google/Meta provider integration interface beyond existing app patterns. The plan remains high enough risk to warrant implementation-time engineering/security/API-contract/testing/docs reviewers because it touches OAuth-backed external integrations and campaign generation state. It should avoid DB schema changes, avoid deterministic freeform response parsing, and avoid unapproved conversion/GBP/Final Review card or panel redesign scope.
