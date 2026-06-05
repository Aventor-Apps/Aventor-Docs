# AVE-13 Source Bundle Context Manifest

No GPT Pro/source attachments were provided by TJ for AVE-13. The plan is grounded in the following inspected sources and should be kept with the branch docs so local Codex does not depend on Discord/Hermes-only context.

## Planning docs in this package

- `docs/linear/AVE-13/PLAN.md`
- `docs/linear/AVE-13/BRAINSTORM.md`
- `docs/linear/AVE-13/ARCHITECTURE.md`
- `docs/linear/AVE-13/QA.md`
- `docs/linear/AVE-13/EXECUTOR.md`
- `docs/linear/AVE-13/REVIEWERS.md`
- `docs/linear/AVE-13/LINEAR_UPDATE.md`
- `docs/linear/AVE-13/source_bundle/context_manifest.md`

## Linear context

- AVE-13: `Google and meta integration during the launch agent and also other blockers to launch agent`.
- Source issue CTC-128 had only the migrated note about handling Google/Meta integration during review steps and fetching needed data prior to generation.
- AVE-40 exists as follow-up backlog issue for conversion tracking setup wizard.

## Repo context inspected

### Backend: `Aventor-Apps/Aventor-backend`

- Baseline `origin/main`: `c66afd27dc37861dc8294bb00ce9e5157b1110fb` at planning time.
- Important files inspected:
  - `src/mastra/agents/launch-agent.ts`
  - `src/mastra/prompts/launch.ts`
  - `src/mastra/tools/delegate-campaign-phase.ts`
  - `src/mastra/tools/validate-account-readiness.ts`
  - `src/mastra/tools/get-google-connection.ts`
  - `src/mastra/tools/get-facebook-connection.ts`
  - `src/mastra/lib/campaign-state.ts`
  - `src/mastra/lib/readiness.ts`
  - `src/mastra/lib/review-advisories.ts`
  - `src/mastra/lib/specialist-output.ts`
  - `src/routes/facebook-get-pixels.ts`
  - `src/routes/google-launch-campaign.ts`
  - `src/routes/google-business-profile.ts`
  - `src/app.ts`
- Important tests inspected/found:
  - `test/mastra/check-readiness.test.ts`
  - `test/mastra/validate-account-readiness.test.ts`
  - `test/routes/google-business-profile.test.ts`
  - Google/Facebook launch and parameter tests.

### Frontend: `Aventor-Git/Aventor`

- Baseline `origin/main`: `936d119a2d4c06c1deb8f63cf960e00ab8d07bed` at planning time.
- Graphify report inspected: `graphify-out/GRAPH_REPORT.md`.
- Important files inspected:
  - `src/utils/api.ts`
  - `src/components/dashboard/CampaignReview.tsx`
  - `src/components/modals/LaunchConnectionConfirmationModal.tsx`

### Docs: `Aventor-Apps/Aventor-Docs`

- Baseline `origin/main`: `d353f10201668caff2e5011c3259769b9a2fd599` at planning time.
- Important files inspected:
  - `docs.json`
  - `integrations/pixel-setup.mdx`
  - `integrations/google-ads.mdx`
  - `integrations/meta-ads.mdx`

## Supabase read-only context

Read-only schema inspection confirmed relevant columns/tables:

- `google_connections`
- `facebook_connections`
- `google_business_profile_selections`
- `campaign_sets`
- `sessions`

Security advisors returned existing project-wide warnings. AVE-13 should avoid new schema unless necessary and route any schema work through security/schema review.

## Durable decisions imported from brainstorm

- Required Google + Meta account readiness blocks generation.
- Conversion tracking gaps warn and offer switch/continue/pause.
- GBP readiness is data-first and conversational.
- Docs update is part of AVE-13.
- Setup wizard is out of scope and tracked by AVE-40.
