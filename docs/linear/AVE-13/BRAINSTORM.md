# AVE-13 Working Brainstorm / Grill Log

Status: auditable brainstorm / grill log for the AVE-13 local-Codex handoff. This preserves the decisions and repo-derived assumptions behind the implementation plan.

## Sources inspected so far

- Hindsight recall for Aventor launch readiness / conversion tracking / GBP.
- Obsidian: `Projects/Aventor/overview.md`, `Projects/Aventor/decisions.md`, `Projects/Aventor/lessons.md`, `System/Assistant/preferences.md`.
- Linear AVE-13 live issue: title `Google and meta integration during the launch agent and also other blockers to launch agent`; state `Backlog`; labels `Migrated`, `Grooming`; description migrated from CTC-128.
- Linear CTC-128 source issue: no additional historical comments beyond migration note; original description only: “Another thing I thought about is handing google and meta integration during the review steps, so we can fetch data we may need prior to generation”.
- Graphify: frontend graph report found at `/home/openclawsvc/repos/aventor/graphify-out/GRAPH_REPORT.md`; backend graph report expected path missing, so fallback to targeted backend source inspection when needed.
- Aventor docs repo: `Aventor-Apps/Aventor-Docs` cloned at `/home/openclawsvc/repos/Aventor-Docs`; Mintlify site `https://docs.aventor.ai`; checked existing `/integrations/pixel-setup`, `/integrations/google-ads`, and `/integrations/overview` docs.

## Captured Q&A / Decisions

### Q1 — Should conversion-tracking / GBP readiness be asked as broad user setup questions or fetched from account data first?

- Planner recommended default: data-first. Aventor should inspect connected Google/Meta/GBP account data where possible and ask only targeted questions for missing/ambiguous/unavailable/API-blocked signals.
- TJ answer: “That's right, and I forgot we can fetch conversion tracking readiness, so we don't need to ask if they have it already, and this includes GBP as well, we can just check from their accounts. So launch agent can own just asking the questions if needed based on the data.”
- Decision: AVE-13 should plan conversion-tracking readiness and GBP readiness as account-data-first checks. The launch/readiness agent owns deciding which targeted follow-up questions are necessary based on fetched readiness data, not a generic pre-launch questionnaire.
- Durable capture completed: patched Aventor marketing/GBP grooming skills, appended Obsidian Aventor decision, retained Hindsight pointer.

### Q2 — Where should the data-first readiness preflight run?

- Planner recommended default: run it both before platform parameter generation/review and again at final launch, with pre-generation shaping the launch agent’s context/questions and final launch acting as stale-state safety recheck.
- TJ answer: “For now let’s just do before.”
- Decision: AVE-13 should scope readiness fetching/checking to the pre-generation / pre-review side for now. Do not add a final-launch revalidation phase in this story unless a narrow existing launch guard must be preserved. Final launch can continue relying on existing launch-time validation/error handling.

### Q3 — How should missing platform connection/readiness affect generation?

- Planner recommended default: missing connection/no selected ad account blocks generation only for the affected platform; ready platforms can still generate, while missing conversion tracking/pixel/conversion action is a warning/setup question and missing GBP is optional enrichment.
- TJ answer: “For blocks, it should block the whole campaign generation, so like they have to have both connected to continue with the generation.”
- Decision: AVE-13 should require both Google and Meta connections/ad-account readiness before campaign generation proceeds. If either required platform connection/selection is missing, block the entire campaign generation flow and ask the user to connect/select the missing platform account(s) before continuing.

### Q4 — After both platforms are connected, do conversion-tracking readiness gaps block generation or become warnings/questions?

- Planner recommended default: conversion tracking / Meta Pixel / Google conversion action gaps should not block generation once Google + Meta are connected and ad accounts are selected. They should surface as strong readiness warnings and targeted setup guidance/questions, especially for lead/conversion goals; GBP remains optional enrichment/warning.
- TJ answer: “That’s right but it should still warn and ask if they want to switch their goal.”
- Decision: conversion-tracking readiness gaps are warning/question-level, not generation blockers, after required platform connections exist. The launch agent should warn clearly and ask whether the user wants to switch away from a goal that depends on conversion tracking.

### Q5 — What happens when the user accepts/declines the goal-switch prompt?

- Planner recommended default: if current goal depends on conversion tracking/leads and tracking is missing, the launch agent offers three choices: switch to a traffic/awareness-style goal, continue with the current goal and setup warning, or pause to set up tracking. Switching updates campaign objective in session/profile state and regenerates/replans params from that goal; continuing keeps original goal and carries warnings into review/launch guidance; pausing stops generation and keeps the session resumable.
- TJ answer: “That’s right.”
- Decision: AVE-13 should implement the goal-switch prompt with switch / continue-with-warning / pause-to-setup semantics. A switch mutates the active campaign goal/session state and should drive regenerated/replanned platform params from the new goal; declining preserves the original goal with visible warnings; pausing blocks generation until user returns.

### Q6 — Which account/readiness signals are in scope for AVE-13 v1, and how should setup/install gaps be handled?

- Planner recommended default: AVE-13 v1 covers fetching/using required Google + Meta connection/ad-account readiness, token/permission health enough to fetch signals, conversion tracking readiness when fetchable, and optional GBP presence/location readiness. It does not build conversion tracking setup, pixel installation, GTM, full landing-page/domain readiness, or final-launch revalidation.
- TJ answer: “That’s right, I think for installation we can just direct them to our docs for now, later we will add conversion tracking setup and wizard, we should probably create a issue for that in the backlog.”
- Decision: AVE-13 v1 should direct users to Aventor docs for conversion-tracking/pixel/conversion-action installation/setup instead of building setup flows. A later backlog issue should own a conversion tracking setup/wizard experience.

### Q7 — Create conversion tracking setup/wizard follow-up issue now or defer creation until final plan approval?

- Planner recommended default: create it now as a separate Backlog issue because setup/wizard is clearly outside AVE-13 but important enough not to lose.
- TJ answer: “That works.”
- Decision/action: Created follow-up Linear issue `AVE-40` — `Add conversion tracking setup wizard for Google and Meta` — in Aventor Backlog and related it to `AVE-13` as a non-blocking related issue. No status moves and no comments were posted.

### Q8 — Should readiness show as deterministic UI or conversational agent behavior?

- Planner recommended default: Google Ads + Meta required connection/account-selection block can use existing connection/select UI affordances plus agent messaging; conversion tracking and GBP could optionally appear in a lightweight readiness card if existing surfaces support it, but not as a large new settings/onboarding page.
- TJ answer: “So you are correct with the connection of google ads and meta, for the conversion and gbp, this should happen conversationally, so not through the UI for those warnings because the launch agent will fetch that information after the connections to be able to match those decisions. And then the final review will still remain the same.”
- Decision: Required Google Ads + Meta connection/ad-account selection can use deterministic UI/connect/select affordances before generation. Conversion-tracking and GBP readiness warnings/questions should be owned by the launch agent conversationally after required platform connections exist; do not add deterministic UI warning cards/panels for those in AVE-13. Final Review remains unchanged.

### Q9 — What docs-link targets should the launch agent use for conversion-tracking setup guidance?

- TJ prompt: “Yea, not sure if you have our aventor-docs repo, but it is a docs website that includes information, I'm not sure how much on conversion actions we have in there, maybe its worth a look.”
- Repo inspection:
  - Found and cloned `Aventor-Apps/Aventor-Docs` to `/home/openclawsvc/repos/Aventor-Docs`; branch `main`, HEAD `d353f10`, clean working tree.
  - Repo is a Mintlify docs site (`docs.json`, `*.mdx`) for `https://docs.aventor.ai`.
  - Existing relevant public docs verified by repo + web extraction:
    - `https://docs.aventor.ai/integrations/pixel-setup` exists and covers Meta Pixel setup, event verification, and troubleshooting.
    - `https://docs.aventor.ai/integrations/google-ads` exists but currently says Google Ads is “coming soon” and does not cover Google conversion actions/tracking.
    - No existing docs page found for Google conversion actions/conversion tracking or Google Business Profile readiness.
- Planner recommended default: AVE-13 launch agent can link Meta missing-pixel/conversion-tracking warnings to `/integrations/pixel-setup`; AVE-13 should also include a small docs-site update in `Aventor-Docs` to add/update Google conversion tracking and GBP readiness guidance, or else the agent will have no accurate docs target for Google/GBP warnings.
- TJ answer: “Yea we should also do a docs update as well.”
- Decision: AVE-13 should include the small Aventor-Docs update in the same implementation handoff, not a separate docs follow-up. Treat `Aventor-Docs` as a third touched repo for the story. Keep docs scope limited to accurate setup/readiness guidance and links for Meta Pixel, Google conversion tracking/conversion actions, and GBP readiness; do not expand into the future conversion-tracking setup wizard (`AVE-40`).

## Repo / Linear baseline refreshed for plan drafting

### Linear baseline

- Live Aventor issue: `AVE-13`, state `Backlog`, labels `Migrated` + `Grooming`, no assignee, no project, no attachments, one migration comment only.
- Source issue `CTC-128` is canceled/migrated and has no additional historical planning comments beyond the migration note.
- Related issue `AVE-40` exists in Aventor Backlog as non-blocking related work: `Add conversion tracking setup wizard for Google and Meta`.
- Aventor team workflow has `Todo` as the unstarted state for approved closeout, but no status move should happen until TJ approves posting the final handoff.

### Git / repo baseline

- Frontend repo `/home/openclawsvc/repos/aventor`: remote `https://github.com/Aventor-Apps/Aventor.git`, current checkout on unrelated `fix/ave-31-uploaded-image-edit-clean`, HEAD `292c2aa`, `origin/main` `936d119a2d4c06c1deb8f63cf960e00ab8d07bed`; untracked `.hermes/`, `docs/linear/CTC-102/qa-artifacts/`, and `graphify-out/`. Use an AVE-13 branch/worktree from `origin/main` for final docs; do not disturb the active unrelated branch.
- Backend repo `/home/openclawsvc/repos/aventor-backend`: remote `https://github.com/Aventor-Apps/Aventor-backend.git`, current checkout on unrelated `fix/ave-32-review-agent-fallback`, HEAD `76ab02d`, `origin/main` `c66afd27dc37861dc8294bb00ce9e5157b1110fb`; worktree clean. Use an AVE-13 branch/worktree from `origin/main` for final docs.
- Docs repo `/home/openclawsvc/repos/Aventor-Docs`: remote `https://github.com/Aventor-Apps/Aventor-Docs.git`, detached HEAD/main at `d353f10201668caff2e5011c3259769b9a2fd599`; use an AVE-13 branch from `origin/main` for final docs.
- Package scripts:
  - Frontend: `npm run typecheck`, `npm run lint`, `npm run build`, `npm run test:unit`.
  - Backend: `npm run typecheck`/`npm run lint` (both `tsc --noEmit`), `npm run build`, `npm test` / `npm run test:unit`.
  - Docs repo: no `package.json` found in current baseline; plan should validate docs with repository-supported Mintlify/docs commands only if local tooling exists after branch setup, otherwise require markdown/Mintlify config static checks.

### Backend/Mastra source findings

- `src/mastra/agents/launch-agent.ts` tools currently include `getFacebookConnection`, `getFacebookAccounts`, `saveFacebookSelection`, `getGoogleConnection`, `getGoogleAccounts`, `saveGoogleSelection`, `validateAccountReadiness`, `allocateCampaignBudget`, `getReachEstimate`, and `pollCampaignStatus`.
- `src/mastra/tools/validate-account-readiness.ts` checks selected Meta/Google account active/billing/token readiness. It does **not** check Meta Pixel, Google conversion actions/goals, or GBP readiness.
- `src/mastra/prompts/launch.ts` currently says not to ask the user to connect Meta or Google during `Finalize Campaign`; AVE-13 must intentionally update this contract because TJ wants required Google + Meta connection/ad-account selection to block the pre-generation flow.
- `src/mastra/lib/review-advisories.ts` currently asks broad yes/no questions: `Do you already have pixel or conversion tracking set up for this goal?` and `Do you have a Google Business Profile for this business?`. AVE-13 should replace/extend this with data-first readiness from account APIs, then ask only targeted questions based on `unknown`/`missing`/`unavailable` signals.
- `src/mastra/tools/delegate-campaign-phase.ts` runs deterministic parameter generation blockers before persisting platform drafts, then projects current review advisory questions after review-ready state. AVE-13 should introduce pre-generation launch-readiness evaluation before parameter drafts persist, so required platform connection blockers happen before generation while conversion/GBP warnings carry into Launch Agent conversational decisions.
- Existing Google conversion readiness logic exists only as a private launch-time helper in `src/routes/google-launch-campaign.ts` (`queryGoogleConversionReadiness` + `requireGoogleSearchConversionReadiness`). It queries `conversion_action` and `customer_conversion_goal` and can be extracted/reused for pre-generation readiness instead of duplicating GAQL logic. There is no public REST/Mastra tool for Google conversion readiness today.
- Existing Meta Pixel fetch route `src/routes/facebook-get-pixels.ts` calls `/{adAccountId}/adspixels` and returns `{ pixels, hasPixel, primaryPixelId }`; route is registered as `/facebook_get_pixels`. For AVE-13, prefer extracting/reusing this logic server-side for launch-agent readiness rather than making the agent/front-end pass raw tokens around.
- Existing GBP REST flow `src/routes/google-business-profile.ts` supports scope exchange, accounts, locations, saved selection, and selection persistence on `google_business_profile_selections`; routes are registered in `src/app.ts`. There is no Mastra launch-readiness tool that surfaces GBP status/selection to the launch agent today.
- Existing backend tests relevant to AVE-13 include `test/mastra/check-readiness.test.ts`, `test/mastra/validate-account-readiness.test.ts`, `test/routes/google-business-profile.test.ts`, Google launch/parameter tests, and Facebook launch/parameter tests. No dedicated tests currently cover data-first conversion/GBP readiness, goal-switch prompt semantics, or required dual-platform generation blocking.

### Frontend source findings

- Existing `CampaignReview.tsx` and `LaunchConnectionConfirmationModal.tsx` already support launch-time account confirmation/change flows for Meta/Google connections and selected accounts/pages/customers.
- Frontend API client already exposes `/facebook_get_connection`, `/facebook_get_pixels`, `/google_get_connection`, Google Ads account selection, and Google Business Profile REST methods.
- Per TJ Q8, AVE-13 should not add deterministic conversion/GBP warning cards or change Final Review UX. Frontend work should be limited to any needed contract plumbing for required platform connection/select flows and chat/agent response option handling.

### Supabase / data baseline

- Live read-only schema inspection confirmed relevant columns exist in `google_connections` (tokens, scopes, `customer_id`, `manager_customer_id`, `customer_name`, `is_expired`, preferred account/currency/timezone), `facebook_connections` (tokens, `ad_account_id`, `page_id`, preferred account/currency/timezone), `google_business_profile_selections`, `campaign_sets`, and `sessions`.
- Supabase security advisors returned existing project-wide warnings (many Mastra/service tables with RLS enabled/no policies, public security-definer RPC execute grants, mutable search paths, permissive service-role policies). AVE-13 should not mutate live Supabase from Planner; executor should treat any new/changed table/RPC/function work as security-reviewed. Current plan can likely avoid new schema by storing readiness in campaign/session state unless implementation finds a durable persistence need.

### Docs baseline

- `Aventor-Docs/docs.json` navigation currently has Meta Ads pages `integrations/meta-ads` and `integrations/pixel-setup`, plus Google Ads page `integrations/google-ads` only.
- `integrations/pixel-setup.mdx` exists and is the confirmed Meta warning docs target (`/integrations/pixel-setup`).
- `integrations/google-ads.mdx` currently says Google Ads integration is “coming soon”; AVE-13 docs work should update it or add narrowly-scoped Google conversion tracking / GBP readiness guidance so Launch Agent warnings have accurate docs targets.

## Review-gate synthesis before handoff

- Product/CEO review: `CLEAR_WITH_CHANGES`. Incorporated explicit product ownership for the dual-platform gate, GBP scope/non-local handling, docs-anchor contract, prompt-replacement specificity, and both-missing blocker UX.
- Engineering review: `CLEAR_WITH_CHANGES` after Planner verified two false subagent findings against `origin/main` (Google conversion helpers and readiness tests do exist). Incorporated valid engineering risks: suppress/dedupe old broad advisories when deterministic readiness facts exist, prefer a two-turn goal-switch pattern, preserve launch-time Google conversion guard, and harden Meta token handling during service extraction.
- DevEx review: `CLEAR_WITH_CHANGES`. Incorporated branch/AGENTS repair, re-fetch/re-verify before branching, docs validation fallback for the docs repo, backend-owned QA package, local-plugin fallback language, and no-new-page docs guidance.
- Design review: not applicable by default because AVE-13 plans no new visible UI/design surface. If implementation expands into substantive UI, route to Claude Code with `frontend-design` and run design review.

## Unresolved / next grill queue

- None recorded after review gates. Remaining actions are side-effect approvals: commit/push AVE-13 branch docs and post/verify the Linear handoff.
