# AVE-13 Executor Handoff

## Start here

You are a local Codex executor implementing Linear AVE-13 from repo-committed Planner Support docs. Use the local `linear-executor` workflow plus `ce:work`.

Read in order:

1. `docs/linear/AVE-13/PLAN.md`
2. `docs/linear/AVE-13/ARCHITECTURE.md`
3. `docs/linear/AVE-13/BRAINSTORM.md`
4. `docs/linear/AVE-13/QA.md`
5. `docs/linear/AVE-13/REVIEWERS.md`
6. `docs/linear/AVE-13/LINEAR_UPDATE.md`
7. root `AGENTS.md` in each repo you touch.

## Repos and branches

Use branch `AVE-13` in every touched repo.

Primary implementation repo:

- `/home/openclawsvc/repos/aventor-backend`
- Remote: `https://github.com/Aventor-Apps/Aventor-backend.git`

Secondary repos:

- `/home/openclawsvc/repos/aventor`
- Remote: `https://github.com/Aventor-Apps/Aventor.git`
- Use only for frontend contract plumbing if needed.

- `/home/openclawsvc/repos/Aventor-Docs`
- Remote: `https://github.com/Aventor-Apps/Aventor-Docs.git`
- Required for the docs update.

Planner Support commits this docs package under `docs/linear/AVE-13/` on the AVE-13 branches. Treat backend docs as the canonical detailed source if copies diverge, then repair divergence before implementation.

## First commands

Run in each repo before edits:

```bash
git status --short --branch
git remote -v
git rev-parse --abbrev-ref HEAD
git rev-parse HEAD
```

Before creating or reusing `AVE-13`, run `git fetch origin main` in each repo. If not on `AVE-13`, switch/create the branch from latest `origin/main` unless TJ explicitly told you to reuse an existing implementation branch.

Planning-time baseline SHAs were backend `c66afd2`, frontend `936d119`, docs `d353f10`. If `origin/main` advanced, re-verify the key source files from `PLAN.md`/`ARCHITECTURE.md` before editing.

Do not overwrite unrelated user work.

## Branch-local handoff repair

Current inspected backend/frontend baselines had stale root `AGENTS.md` content from an older CTC-138 issue. On the AVE-13 branches:

- update root `AGENTS.md` to point to AVE-13 docs and scope;
- preserve generic routing/safety rules that still apply;
- in the docs repo, create `AGENTS.md` if absent;
- commit these docs/handoff repairs before broad implementation if needed.

Do not open a planning-docs-only PR. Implementation PR(s) should include code/docs changes and QA package.


## Planned local skills / plugins

Use these in addition to the source docs and repo tooling:

- `linear-executor` for implementation orchestration and workpad/QA-package discipline.
- `linear-reviewer` for report-only QA/review closeout.
- `agent-orchestration-skill` in the root session only when bounded worker/reviewer dispatch is useful; never ask workers to invoke it.
- `ce:work` for execution discipline and `ce:review` for review orchestration with ECC prompt-backed reviewers.
- Superpowers process skills: `using-git-worktrees`, `writing-plans`, `test-driven-development`, `verification-before-completion`, and `requesting-code-review` as applicable.
- Backend/API/security/data skills: `ecc:backend-patterns`, `ecc:api-design`, `ecc:error-handling`, `ecc:security-review`, `api-contract-reviewer`, and `supabase-postgres-best-practices` when schema/data persistence is touched.
- Supabase plugin in read-only mode for project/schema context unless TJ explicitly approves a write/migration action.
- Open Design MCP workflow only if substantive UI unexpectedly appears: `get_artifact()` for existing artifacts, or `create_project` / `start_run` / `get_run` / `get_artifact` for a new artifact when none exists; block as `BLOCKED_DESIGN` if unavailable.

## Implementation routing

Codex may own:

- backend services, tests, state machine, prompt contracts, docs;
- frontend non-visual API/type plumbing and tests;
- Aventor-Docs markdown/docs updates;
- validation, reviewer coordination, QA package, PR/Linear updates.

Substantive visible UI/design/styling/layout/component work is not expected. If it becomes necessary, follow the `linear-executor` Open Design route:

1. Inspect the repo design system first.
2. Fetch an existing Open Design artifact/design bundle with `get_artifact()` when available.
3. If no suitable artifact exists, create/commission a new Open Design artifact/run (`create_project` if needed, `start_run`, poll `get_run`, then `get_artifact`) and require reuse of repo primitives/tokens.
4. Record the Open Design project/artifact/run, design-system files inspected, integration scope, and blocker status in the workpad.
5. If Open Design MCP, artifact fetch, or artifact creation is unavailable/blocked, mark UI work `BLOCKED_DESIGN` and stop. Do not fall back to plain Codex, Claude Code, Kimi, or raw Ollama for substantive UI work.

## Phase checklist

### Phase 0 — Grounding

- [ ] Read all docs above.
- [ ] Read Graphify report if available (`aventor/graphify-out/GRAPH_REPORT.md`; backend report missing at planning time).
- [ ] Inspect current files before editing.
- [ ] Confirm no unrelated work will be overwritten.
- [ ] Restate local plan and validation commands in your session before broad edits.

### Phase 1 — Tests first

- [ ] Add failing backend tests for launch readiness preflight.
- [ ] Cover missing Google, missing Meta, tracking missing, tracking unknown, GBP states, structured switch/continue/pause decisions, and no-draft persistence on blockers.
- [ ] Run focused tests to confirm expected failures before implementation.

### Phase 2 — Provider readiness services

- [ ] Extract Meta Pixel service from `facebook-get-pixels.ts`; keep tokens out of logs and prefer bearer authorization over query-string tokens where the provider contract allows.
- [ ] Extract Google conversion readiness service from `google-launch-campaign.ts` without changing launch-time behavior.
- [ ] Add GBP readiness helper around existing selection data.
- [ ] Ensure tests mock provider/Supabase responses and do not require live credentials.

### Phase 3 — Launch readiness domain

- [ ] Implement `src/mastra/lib/launch-readiness.ts` or equivalent.
- [ ] Add safe readiness snapshot/status schema to campaign state if needed.
- [ ] Reuse existing finalization blocker/decision patterns where possible.
- [ ] Keep required connection blockers distinct from warning decisions.

### Phase 4 — Param-generate integration

- [ ] Integrate preflight before parameter drafts persist.
- [ ] Block whole generation for missing required Google/Meta setup.
- [ ] Add conversion warning decision group for conversion-dependent goals.
- [ ] Implement structured switch/continue/pause decision application idempotently. Prefer the two-turn goal-switch pattern documented in `ARCHITECTURE.md`; do not add recursive in-turn generation unless existing code already supports it safely. Do not add deterministic regex/string parsing of freeform user responses; Mastra/orchestrator owns natural-language interpretation and must emit structured `finalizationDecision` / `finalizationDecisions` payloads.
- [ ] Update launch prompt to remove conflict with required account blockers using the exact replacement direction in `PLAN.md`.
- [ ] Suppress/replace broad pixel/GBP advisory questions when readiness facts exist.

### Phase 5 — Frontend if needed

- [ ] Verify existing chat response options and connection modal recover the blocker/warning flows through structured decision payloads, without adding regex parsing of raw user text.
- [ ] Update `src/utils/api.ts` only if a frontend-visible contract is introduced.
- [ ] No Final Review readiness cards.

### Phase 6 — Docs

- [ ] Update `Aventor-Docs/integrations/google-ads.mdx` in-place with conversion tracking and GBP readiness guidance.
- [ ] Create/verify anchors `#conversion-tracking-readiness` and `#google-business-profile-readiness`.
- [ ] Keep Meta target `/integrations/pixel-setup`.
- [ ] Do not add a new docs page unless implementation explicitly chooses to change the docs-link contract; `docs.json` should not need changes.
- [ ] Do not claim the future setup wizard is shipped.

### Phase 7 — Validation/review/QA package

- [ ] Run backend validation.
- [ ] Run frontend validation if frontend touched.
- [ ] Run docs validation.
- [ ] Run reviewer gates from `REVIEWERS.md`.
- [ ] Repair blocking reviewer findings and rerun impacted validation.
- [ ] Create final QA package with all four required files.
- [ ] Open PR(s), link PRs and QA package in Linear update.

## Validation commands

Backend:

```bash
npm run typecheck
npm run build
npm test
```

Frontend if touched:

```bash
npm run typecheck
npm run lint
npm run build
npm run test:unit
```

Docs:

- Use exact repo-supported docs validation if available.
- Planning baseline showed no docs `package.json`. Minimum fallback:
  - `python3 -m json.tool docs.json` if `docs.json` is edited or to verify structure;
  - manually/static-check edited `.mdx` for broken Markdown/MDX and internal links;
  - verify backend docs-link constants resolve to the final docs anchors.
- If you install/use a Mintlify CLI locally, record the exact command and result.

Record exact commands and results in `docs/linear/AVE-13/qa-package/validation_results.md`.

## Final QA package contract

Create before final closeout in the backend repo as the canonical QA package:

```text
docs/linear/AVE-13/qa-package/checkpoint_manifest.md
docs/linear/AVE-13/qa-package/browser_qa_prompt.md
docs/linear/AVE-13/qa-package/validation_results.md
docs/linear/AVE-13/qa-package/qa_readiness_result.json
```

`browser_qa_prompt.md` must be report-only and forbid code patches, commits, pushes, live DB/Supabase mutation, Linear Done/closed transitions, and secrets in reports.

Use readiness verdicts:

- `READY`
- `READY_WITH_SETUP_CHECKPOINTS`
- `BLOCKED_SETUP`
- `BLOCKED_IMPLEMENTATION`

## PR / Linear closeout

After implementation and reviewer gates pass:

1. Commit intended changes in each touched repo.
2. Push `AVE-13` branches.
3. Open implementation PR(s) only after code/docs/validation/QA package are ready.
4. Update Linear through the local Linear plugin with:
   - branches/PRs;
   - summary;
   - validation results;
   - reviewer outcomes;
   - QA package paths/verdict;
   - docs update summary;
   - follow-up AVE-40 remains out of scope.

Do not use Planner Support Hermes API keys or Hermes-only MCP tool names.

If `linear-executor`, `ce:work`, local Linear plugin, or local Supabase plugin access is unavailable, fall back to git/gh/manual Linear workflows rather than blocking silently. Document the substitution in `validation_results.md` and stop only if required repo/Linear/GitHub access is unavailable for closeout.

## Stop conditions

Stop and ask TJ / report blocker if:

- required repo access is missing;
- Linear/GitHub access needed for closeout is unavailable;
- implementation requires live schema/data mutation not already approved;
- provider credentials are required for tests beyond mocked/local fixtures;
- substantive UI work is needed and approved UI route is unavailable;
- plan assumptions conflict with current repo behavior in a way that changes scope.
