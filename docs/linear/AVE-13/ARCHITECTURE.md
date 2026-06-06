# AVE-13 Architecture Deep Dive — Launch readiness preflight

## Purpose

This document gives the executor the architectural context needed to implement AVE-13 safely. It explains the current flow, target flow, data/control boundaries, sequence examples, payload examples, edge cases, and implementation implications.

## Current architecture

### Current finalization path

At a high level, Aventor campaign finalization works like this:

1. Frontend sends a campaign-phase request after the user asks to finalize/prepare review.
2. Mastra/orchestrator routes the turn to `delegate-campaign-phase.ts`.
3. For `param_generate`, deterministic backend logic evaluates blockers and either:
   - returns a waiting-on-blocker state, or
   - generates/persists platform parameter drafts and moves toward review-ready state.
4. The Launch Agent writes user-facing copy from the validated state.
5. Review Agent may ask existing advisory questions before review-ready presentation.
6. Final launch routes use ApprovalSnapshot and provider APIs to launch campaigns.

Important current files:

- `src/mastra/tools/delegate-campaign-phase.ts`: central deterministic state machine for finalization.
- `src/mastra/prompts/launch.ts`: Launch Agent contract and user-facing behavior.
- `src/mastra/lib/campaign-state.ts`: Zod schemas and persisted campaign state shape.
- `src/mastra/lib/review-advisories.ts`: broad yes/no pixel/GBP advisory questions.
- `src/mastra/lib/specialist-output.ts`: finalization blockers/warnings and output normalization.

### Current account readiness

The Launch Agent already has account tooling:

- `getFacebookConnection`, `getFacebookAccounts`, `saveFacebookSelection`.
- `getGoogleConnection`, `getGoogleAccounts`, `saveGoogleSelection`.
- `validateAccountReadiness`.

`validate-account-readiness.ts` checks provider-account health such as active account/billing/token states. It does not check Meta Pixel, Google conversion actions/goals, or GBP readiness.

Current gap: account readiness is not enforced as TJ wants before generation. The existing prompt also currently says not to ask the user to connect Meta/Google during Finalize Campaign, which conflicts with AVE-13's required account gate.

### Current conversion readiness

Meta:

- `src/routes/facebook-get-pixels.ts` implements `/facebook_get_pixels`.
- It calls Meta Graph `/{adAccountId}/adspixels` and returns safe facts like pixels, `hasPixel`, and `primaryPixelId`.

Google:

- `src/routes/google-launch-campaign.ts` contains private helpers:
  - `GoogleConversionReadiness` type.
  - `queryGoogleConversionReadiness()`.
  - `requireGoogleSearchConversionReadiness()`.
- This logic queries Google Ads conversion actions and customer conversion goals during launch-time validation.

Current gap: both data sources exist, but they are not available as a unified pre-generation readiness signal for the launch-agent flow.

### Current GBP readiness

`src/routes/google-business-profile.ts` supports:

- exchanging/adding GBP OAuth scope,
- listing accounts,
- listing locations,
- getting/saving selected GBP location.

Data persists in `google_business_profile_selections`.

Current gap: there is no Mastra/readiness helper that turns existing GBP selection data into a launch-agent fact.

## Target architecture

AVE-13 should add a **server-side launch readiness preflight** before parameter drafts are generated.

### Design principles

1. **Facts are deterministic.** Provider account/pixel/conversion/GBP facts come from backend services, not LLM guesses.
2. **Conversation is agent-owned.** The Launch Agent explains facts and asks targeted decisions, but blocker truth comes from deterministic state.
3. **Required setup blocks; optimization setup warns.** Missing Google/Meta connection/selection blocks. Missing conversion tracking warns/asks after required accounts exist; GBP is optional and can be `not_applicable` for non-local contexts.
4. **Goal decisions are idempotent and structured.** Switch/continue/pause decisions must survive retries, not duplicate prompts, and arrive as structured Mastra/orchestrator decisions rather than backend regex parsing of freeform text.
5. **No token leakage.** Tokens remain server-side. State, logs, docs, QA artifacts, and Linear comments only contain safe status/identifier data.
6. **No default DB migration.** Store readiness in campaign/session state if needed; avoid new tables unless implementation proves a strong need.
7. **Final Review is stable.** Do not create new conversion/GBP UI cards or redesign review screens.
8. **Show all required blockers together.** If both Google and Meta required setup are missing, return both blockers in one response instead of making the user fix them sequentially.
9. **Mastra owns user-response interpretation.** Deterministic code validates and applies structured decision payloads; it must not infer user intent from raw text with regex/string matching.

## Target component map

```text
Frontend repo (aventor)
  Existing finalize action / chat stream
  Existing response option rendering
  Existing LaunchConnectionConfirmationModal
  Optional API/type updates only if backend exposes new frontend contract

Backend repo (aventor-backend)
  src/mastra/tools/delegate-campaign-phase.ts
    -> calls launch-readiness preflight before draft generation

  src/mastra/lib/launch-readiness.ts (new)
    -> combines required account, conversion, and GBP facts
    -> emits readiness snapshot + finalization blockers/warnings
    -> applies structured switch/continue/pause decisions emitted by Mastra/orchestrator, or delegates to existing finalization decision handling

  src/lib/facebook|meta/pixels.ts (new extraction)
    -> shared Meta Pixel readiness service

  src/lib/google-ads/conversion-readiness.ts (new extraction)
    -> shared Google conversion action/goal readiness service

  src/lib/google-business-profile/readiness.ts (new helper)
    -> shared GBP selection/readiness helper

  src/mastra/prompts/launch.ts
    -> Launch Agent writes from backend-provided readiness facts

Docs repo (Aventor-Docs)
  integrations/pixel-setup.mdx (existing Meta target)
  integrations/google-ads.mdx (Google conversion/GBP readiness update)
```

## Data/control boundaries

### Backend-only provider boundary

Provider APIs must only be called from backend/server-side code:

- Meta token -> Meta Graph Pixel query -> safe readiness fact.
- Google refresh/access token -> Google Ads conversion readiness query -> safe readiness fact.
- Google OAuth/GBP connection -> Supabase GBP selection + optional provider fetch -> safe readiness fact.

Never send raw tokens, refresh tokens, provider API errors containing secrets, or OAuth scopes as unredacted user-facing output.

### Deterministic state-machine boundary

`delegate-campaign-phase.ts` should own whether generation can proceed. The LLM should not decide from scratch whether an account is connected or whether parameter drafts should persist.

Expected deterministic outputs:

- blocking finalization blockers for missing required Google/Meta account setup;
- non-blocking/warning decision groups for conversion-dependent goal risk;
- optional warning facts for GBP missing/unknown;
- a readiness snapshot safe enough to persist in campaign state.

### Agent conversation boundary

Natural-language user response interpretation must stay Mastra-owned. Deterministic/backend code must not parse freeform user text with regex/string matching to decide `switch`, `continue`, `pause`, or similar intent. The orchestrator/Launch Agent should convert user language or button selections into structured `finalizationDecision` / `finalizationDecisions` payloads; deterministic code may validate, persist, and apply only those structured decisions by stable IDs/actions.

The Launch Agent should:

- explain blocker/warning facts in short copy;
- ask one clear question/action at a time;
- include docs links only for setup guidance;
- preserve backend-provided blocker truth;
- not invent readiness facts.

### Frontend boundary

Frontend should:

- render existing chat and response options;
- reuse existing connection/select modal flows for account blockers;
- not inspect tokens or provider APIs directly;
- avoid new conversion/GBP readiness UI surfaces;
- submit structured response-option payloads through existing Mastra/orchestrator decision handling rather than adding client/server regex parsing of freeform text.

## Suggested readiness status model

Exact naming can change to match repo conventions, but the model needs these distinctions:

```ts
type ReadinessStatus =
  | "ready"
  | "missing"
  | "unknown"
  | "unavailable"
  | "not_applicable";

type LaunchReadinessIssue = {
  id: string;
  source: "meta" | "google_search" | "google_pmax" | "input";
  severity: "critical" | "warning";
  message: string;
  docsUrl?: string;
  blocking: boolean;
  reasonCode?: string;
};

type LaunchReadinessSnapshot = {
  version: "launch_readiness_v1";
  checkedAt: string;
  requiredConnections: {
    metaAds: {
      status: ReadinessStatus;
      adAccountId?: string;
      pageId?: string;
      issues: LaunchReadinessIssue[];
    };
    googleAds: {
      status: ReadinessStatus;
      customerId?: string;
      issues: LaunchReadinessIssue[];
    };
  };
  conversionTracking: {
    metaPixel: {
      status: ReadinessStatus;
      primaryPixelId?: string;
      docsUrl: "/integrations/pixel-setup";
      reasonCode?: string;
    };
    googleConversions: {
      status: ReadinessStatus;
      conversionActionCount?: number;
      docsUrl: "/integrations/google-ads#conversion-tracking-readiness";
      reasonCode?: string;
    };
  };
  googleBusinessProfile: {
    status: ReadinessStatus;
    locationTitle?: string;
    docsUrl: "/integrations/google-ads#google-business-profile-readiness";
    reasonCode?: string;
  };
  blockers: LaunchReadinessIssue[];
  warnings: LaunchReadinessIssue[];
};
```

Implementation should prefer existing Zod schema patterns in `campaign-state.ts` and avoid over-modeling beyond AVE-13.

## Sequence examples

### Sequence A — Google not connected

```text
User clicks Finalize Campaign
  -> frontend sends existing phase/stream request
  -> delegate-campaign-phase enters param_generate
  -> launch-readiness checks required connections
  -> googleAds.status = missing, metaAds.status = ready
  -> finalizationBlocker: critical, blocking, source google_search/google_pmax
  -> no parameter drafts generated
  -> Launch Agent: “I need a connected Google Ads customer and Meta ad account before I can prepare this campaign. Connect/select Google Ads, then I’ll continue.”
  -> existing account connect/select UI handles recovery
```

Key invariant: **no draft persistence** while required connection blocker is active.

### Sequence B — Both accounts ready, tracking missing for leads goal

```text
User clicks Finalize Campaign
  -> required connections ready
  -> Meta Pixel service returns missing
  -> Google conversion service returns missing
  -> campaignGoal = leads
  -> preflight creates conversion warning decision group
  -> no drafts generated until user chooses switch/continue/pause
  -> Launch Agent asks:
     “I can prepare this, but I don’t see conversion tracking for your lead goal. Do you want to switch to a traffic goal, continue with a tracking warning, or pause to set up tracking?”
```

If user chooses switch:

```text
User: switch to traffic
  -> deterministic decision handler updates campaignGoal = traffic
  -> warning resolved/superseded
  -> current turn returns a confirmation / waiting state with the warning cleared
  -> next deterministic param_generate turn sees campaignGoal = traffic
  -> drafts persist for traffic objective without a recursive in-turn rerun
```

If user chooses continue:

```text
User: continue with warning
  -> accepted warning decision persisted
  -> original leads goal remains
  -> drafts generate
  -> review summary includes notable tracking warning
```

If user chooses pause:

```text
User: pause for setup
  -> state remains waiting/resumable
  -> docs links returned
  -> no drafts generated
```

### Sequence C — Meta Pixel ready, Google conversion unknown

```text
Google Ads conversion API returns permission/rate/API unavailable
  -> googleConversions.status = unknown or unavailable
  -> Launch Agent does not claim tracking is missing
  -> If goal depends on conversion tracking, ask a targeted uncertainty question or offer continue/pause:
     “I couldn’t confirm Google conversion actions from the connected account. If you know tracking is ready, you can continue; otherwise pause and check setup.”
```

Key invariant: provider fetch errors do not become false readiness facts.

### Sequence D — GBP selected

```text
Preflight reads google_business_profile_selections
  -> gbp.status = ready, locationTitle = selected location
  -> readiness facts include selected GBP
  -> no GBP advisory question is asked
  -> parameter generation can include GBP context where existing generation context supports it
```

### Sequence E — GBP missing / unknown / not applicable

```text
No saved GBP selection / missing scope / fetch unknown
  -> if local-relevant: gbp.status = unknown/unavailable/missing with optional guidance
  -> if clearly non-local and no GBP signal: gbp.status = not_applicable
  -> no generation blocker
  -> Launch Agent may explain optional local campaign benefit and link docs only when relevant
  -> Final Review layout unchanged
```

### Sequence F — Both Google and Meta required setup missing

```text
Preflight checks both required connections
  -> googleAds.status = missing, metaAds.status = missing
  -> two critical finalization blockers returned in one response
  -> Launch Agent names both missing platforms in one message
  -> user can address either or both before retrying
  -> no parameter drafts generated
```

Key invariant: present all missing required account blockers together, not one-at-a-time across multiple failed attempts.

## Request/response and payload examples

### Internal readiness input

```ts
type EvaluateLaunchReadinessInput = {
  state: CampaignState;
  supabase: SupabaseClientLike;
  now?: () => Date;
  fetch?: typeof globalThis.fetch;
};
```

The evaluator should derive identity/session/user context from existing `CampaignState` fields and existing helper patterns. It should not require frontend to pass tokens.

### Blocking finalization example

```json
{
  "phase": "param_generate",
  "finalizationBlockers": [
    {
      "id": "launch_readiness.google_ads_connection_required",
      "source": "google_search",
      "severity": "critical",
      "message": "Connect and select a Google Ads customer before I prepare campaign parameters.",
      "blocking": true,
      "question": "Connect/select Google Ads, then continue finalization.",
      "questionType": "finite_options",
      "options": ["Connect Google Ads", "I'll do this later"],
      "renderStyle": "chat",
      "decisionKind": "required_input"
    }
  ]
}
```

Exact option labels should match existing frontend response option conventions. These examples are conversational; actual state changes must be driven by structured `finalizationDecision` / `finalizationDecisions` payloads emitted by Mastra/orchestrator, not deterministic regex parsing of raw user text.

### Conversion warning example

```json
{
  "id": "launch_readiness.conversion_tracking_missing.leads",
  "source": "meta",
  "severity": "warning",
  "message": "I do not see conversion tracking ready for this lead-generation goal.",
  "blocking": true,
  "question": "Do you want to switch to a traffic goal, continue with the warning, or pause to set up tracking?",
  "questionType": "finite_options",
  "options": ["Switch to traffic goal", "Continue with warning", "Pause for setup"],
  "renderStyle": "chat",
  "decisionKind": "warning_confirmation"
}
```

Note: `blocking: true` here means “requires a user decision before generation,” not “tracking is a hard account blocker.” Continue should resolve/suppress the warning and proceed.

### Readiness snapshot example

```json
{
  "version": "launch_readiness_v1",
  "checkedAt": "2026-06-05T00:00:00.000Z",
  "requiredConnections": {
    "metaAds": { "status": "ready", "adAccountId": "act_..." },
    "googleAds": { "status": "ready", "customerId": "1234567890" }
  },
  "conversionTracking": {
    "metaPixel": { "status": "missing", "docsUrl": "/integrations/pixel-setup" },
    "googleConversions": { "status": "unknown", "docsUrl": "/integrations/google-ads#conversion-tracking-readiness", "reasonCode": "API_UNAVAILABLE" }
  },
  "googleBusinessProfile": {
    "status": "ready",
    "locationTitle": "Example Business",
    "docsUrl": "/integrations/google-ads#google-business-profile-readiness"
  }
}
```

Do not include access tokens, refresh tokens, or sensitive provider error bodies.

## Integration points in `delegate-campaign-phase.ts`

The likely insertion point is inside `runDeterministicParamGeneration()` after pending finalization answers / selected generation context are resolved and before budget allocation or parameter draft generation.

Target order:

1. Apply prior pending finalization answers.
2. Attach selected generation context to state.
3. Evaluate existing status output if needed.
4. Build selected platforms/context.
5. Existing deterministic blockers: website, vague geography, budget, media, marketing, learning.
6. **New launch-readiness preflight before parameter generation.**
7. If active blocker/decision required exists, return waiting-on-blocker.
8. If safe to proceed, allocate budget and generate/persist drafts.

Implementation can place the preflight slightly earlier/later if tests prove behavior, but the invariant is: readiness blockers/warnings must be resolved before draft persistence.

### Marketing-advisory conflict resolution

Existing `review-advisories.ts` / `specialist-output.ts` can already produce broad pixel and GBP advisory questions. AVE-13 should not show those broad questions when deterministic launch-readiness facts already answer the same topic.

Recommended rules:

- Use `launch_readiness.*` IDs for new account blockers and conversion/GBP warning decisions.
- If required Google/Meta account blockers are active, suppress marketing-advisory pixel/GBP questions for that turn; account setup must be fixed before advisory nuance matters.
- If conversion/GBP readiness facts are `ready`, `missing`, or `not_applicable`, bypass the old broad yes/no advisory for that signal.
- If provider/API state is `unknown`/`unavailable`, ask a targeted uncertainty question instead of the generic "do you have tracking/GBP?" advisory.
- Reuse the existing accepted/suppressed finalization-decision pattern so `continue with warning` does not reopen the same warning on every retry.

## Goal-switch implementation implications

A goal switch is not a cosmetic response. It must update the source of truth used for platform parameter generation:

- normalize the selected goal via existing `campaignGoalSchema` / `normalizeCampaignGoalLike` helpers;
- write updated goal into `CampaignState` fields used by generation (`campaignGoal`, possibly goal aliases if present);
- clear or supersede the conversion-dependent warning;
- avoid a recursive in-turn generation rerun unless the existing state machine already supports it cleanly; the preferred implementation is a two-turn pattern: turn 1 applies the goal switch and returns confirmation/waiting state, turn 2 enters `param_generate` with the updated goal and proceeds;
- ensure budget/platform/media context remains intact;
- avoid duplicating drafts from the previous goal.

If existing finalization warning decision actions are too limited, add the smallest compatible extension rather than building a separate decision engine.

## Docs-link contract

Backend copy should be able to reference stable docs targets:

- Meta Pixel: `/integrations/pixel-setup`.
- Google conversion tracking: `/integrations/google-ads#conversion-tracking-readiness`.
- GBP readiness: `/integrations/google-ads#google-business-profile-readiness`.

The docs update should create those exact anchors in `integrations/google-ads.mdx`. If executor intentionally changes docs paths/anchors, update backend constants/tests, `QA.md`, and `LINEAR_UPDATE.md` accordingly.

## Edge cases

1. **Selected platforms do not include both Google and Meta.** TJ decision says both Google and Meta connections/ad accounts are required for this pre-generation flow. If existing product supports single-platform selection, document how AVE-13 reconciles that. Default plan assumption: AVE-13 requires both required account setups before generation regardless of selected platform subset.
2. **Google Search vs PMax.** Google connection/customer readiness applies to both. Launch-time conversion enforcement may differ by campaign type; preserve existing launch behavior.
3. **Meta page missing.** If existing Meta launch flow requires page selection, include page readiness in required Meta setup. Do not invent a new requirement if existing launch code does not require it.
4. **OAuth token expired.** Treat required account readiness as blocking if token refresh/readiness cannot support provider fetches. Provide concise reconnect guidance.
5. **Provider API timeout.** Classify as `unknown`/`unavailable`; do not claim the user lacks tracking.
6. **Multiple pixels/conversion actions.** For AVE-13 v1, presence/readiness is enough unless existing launch logic requires a primary selection. Prefer `primaryPixelId` from existing service if available.
7. **In-flight old advisory state.** Old sessions may have `reviewAdvisoryState.pixelTracking` or `googleBusinessProfile`. Code should tolerate/migrate/ignore safely.
8. **Repeated continue message.** Accepted warnings should not re-open on every retry.
9. **Docs unavailable locally.** If docs repo lacks package scripts, validate with JSON/MDX static checks and record caveat.
10. **No live credentials in test.** Unit tests should mock fetch/Supabase/provider responses.
11. **GBP OAuth scope not granted.** Treat as `unknown`/`unavailable` for local-relevant campaigns, not as a hard missing setup fact. Do not implement a GBP scope wizard/prompt in AVE-13.
12. **Non-local campaign with no GBP signal.** Skip GBP readiness chatter and set status to `not_applicable` if that status is added internally.
13. **Both required platforms missing.** Return both required blockers in one response.
14. **No deterministic freeform parsing.** Tests/review must prove backend code applies structured decisions and does not regex-parse user utterances for switch/continue/pause intent.
15. **New `not_applicable` status.** If persisted or exposed to frontend, update Zod/API tests and include API-contract/schema reviewer attention.

## Security and privacy implications

- Redact tokens and provider error bodies from logs.
- During Meta Pixel service extraction, prefer bearer authorization over query-string access tokens where compatible, and never log constructed URLs that contain tokens.
- Avoid logging full account objects; log stable non-sensitive status and reason codes.
- No live Supabase schema/data mutation from Planner docs.
- If new persistence is introduced, review RLS/policies/indexes with security/schema reviewers.
- QA package must not include real customer account identifiers beyond safe masked IDs already exposed by UI.

## Implementation implications by repo

### Backend

Most work belongs here. Codex can implement backend services, tests, state machine, prompt, and docs under `docs/linear/AVE-13/`.

### Frontend

Expected work is small. Codex may handle non-visual API/type plumbing. If a real visible component/interaction change emerges, route it through local Open Design per `linear-executor`: fetch an existing artifact/design bundle with `get_artifact()` or create/commission a new artifact and pull it with `get_artifact()`. If Open Design is unavailable or cannot create/fetch the artifact, mark `BLOCKED_DESIGN` and stop.

### Aventor-Docs

Docs update should be merged with implementation, not deferred. Keep scope limited to readiness/setup guidance; avoid promising the future AVE-40 wizard as shipped.

## Verification implications

- Backend tests are the main proof.
- Frontend tests only if frontend touched.
- Docs validation should prove edited docs parse and links match backend constants.
- Reviewers should include engineering/correctness, API-contract/external integration, security, testing, and documentation/readability.
