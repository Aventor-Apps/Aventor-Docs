# AVE-13 Linear Update Draft

> Planner Support should post this only after TJ approves the groomed plan/handoff. Do not post during preview.

## Suggested Linear comment

Groomed AVE-13 for local-Codex execution.

**Scope:** data-first launch-agent readiness preflight before campaign parameter generation, plus a small Aventor-Docs update.

**Branches:** `AVE-13`

**Repos / docs roots:**

- Backend primary: `Aventor-Apps/Aventor-backend` — `docs/linear/AVE-13/`
- Frontend secondary: `Aventor-Git/Aventor` — `docs/linear/AVE-13/` if frontend contract plumbing is needed
- Docs secondary: `Aventor-Apps/Aventor-Docs` — `docs/linear/AVE-13/` plus docs-site implementation changes

**Plan docs:**

- `PLAN.md`
- `BRAINSTORM.md`
- `ARCHITECTURE.md`
- `QA.md`
- `EXECUTOR.md`
- `REVIEWERS.md`
- `LINEAR_UPDATE.md`
- `source_bundle/context_manifest.md`

**Required local implementation workflow:** use local Codex `linear-executor` + `ce:work`; use local Linear and Supabase plugins/connectors, not Planner Support Hermes credentials. Use Graphify/docs-first inspection where available.

**Review gates:** Planner ran product/CEO, engineering, and DevEx handoff review gates. Result: `CLEAR_WITH_CHANGES`; doc clarifications were incorporated before handoff.

**Key decisions captured:**

- Google + Meta connection/ad-account readiness blocks the whole pre-generation flow if missing, even for an apparent single-platform subset; single-platform-only generation is separate product scope.
- Conversion tracking readiness is fetched from connected account data first.
- Missing conversion tracking after required accounts exist is a warning/decision, not a hard blocker.
- Launch Agent asks switch goal / continue with warning / pause for setup.
- GBP readiness is fetched and handled conversationally when local-relevant; missing scope/unknown GBP is not a blocker, and non-local campaigns can treat GBP as not applicable.
- No new conversion/GBP readiness card or Final Review redesign.
- Meta docs target: `/integrations/pixel-setup`.
- Google conversion/GBP docs update is included in AVE-13 with expected anchors `#conversion-tracking-readiness` and `#google-business-profile-readiness`.
- Setup wizard remains out of scope in AVE-13; follow-up is AVE-40.

**Validation expected before implementation PR(s):**

- Backend: `npm run typecheck`, `npm run build`, `npm test`, plus focused readiness tests.
- Frontend if touched: `npm run typecheck`, `npm run lint`, `npm run build`, `npm run test:unit`.
- Docs: repo-supported docs validation or static MDX/JSON/link/anchor validation.
- Reviewer gates: correctness/spec, engineering, API-contract/external integration, security, testing, docs; frontend/design only if visible UI changes; schema/data only if schema changes.

**Final QA package required before closeout:**

- `docs/linear/AVE-13/qa-package/checkpoint_manifest.md`
- `docs/linear/AVE-13/qa-package/browser_qa_prompt.md`
- `docs/linear/AVE-13/qa-package/validation_results.md`
- `docs/linear/AVE-13/qa-package/qa_readiness_result.json`

Browser screenshots/video are optional unless TJ or a reviewer specifically asks.

## Approved closeout action after posting

After TJ approves posting this handoff, Planner Support should:

1. Commit/push AVE-13 handoff docs to the issue branches.
2. Post this Linear comment.
3. Verify the live Linear comment.
4. Move AVE-13 to the Aventor team `Todo`/`To Do` state unless TJ says to leave status unchanged.
5. Verify labels/status: `Groomed` present, `Grooming` absent.

Do not create subtasks unless TJ says exactly `create the subtasks`. Do not update the issue description unless TJ says exactly `update the description`.
