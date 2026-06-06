# AVE-13 Reviewer Plan

## Reviewer gate summary

Run independent reviewer subagents after implementation and after major repairs. Do not treat Codex self-review as a substitute.

Because AVE-13 touches OAuth-backed external integrations, campaign generation state, provider API contracts, and docs, the reviewer set should include:

1. **Correctness / spec compliance reviewer** — required.
2. **Engineering / maintainability reviewer** — required.
3. **API-contract / external integration reviewer** — required.
4. **Security reviewer** — required because OAuth tokens/provider APIs/customer account data are in scope.
5. **Testing reviewer** — required.
6. **Docs reviewer** — required for Aventor-Docs update.
7. **Frontend/design/accessibility reviewer** — conditional only if visible frontend UI changes are made.
8. **Schema/data reviewer** — conditional only if a migration/RPC/table/policy/persistence change is introduced.

## Required reviewer questions

### Correctness / spec compliance

- Does missing Google or Meta required account setup block the whole generation flow before drafts persist?
- Are conversion-tracking gaps warnings/decisions, not hard blockers, after required accounts exist?
- Are switch/continue/pause semantics implemented exactly through structured Mastra/orchestrator decisions, without deterministic regex parsing of freeform user responses?
- Is GBP optional/non-blocking?
- Is Final Review unchanged except any explicitly approved/Open Design-sourced AVE-13 interaction surface?
- Is AVE-40 setup wizard still out of scope?

### Engineering / maintainability

- Are Meta Pixel, Google conversion readiness, and GBP readiness implemented as reusable services rather than duplicated route/tool logic?
- Did extracting Google conversion readiness preserve launch-time behavior?
- Is the state-machine integration placed before draft persistence?
- Are `launch_readiness.*` blockers/warnings deduped against broad marketing advisories so users do not get conflicting prompts?
- Are accepted warnings idempotent across retries?
- Are old sessions/advisory states tolerated?
- Are docs-link constants centralized/testable enough?

### API-contract / external integration

- Does the diff avoid deterministic regex/string intent parsing of user responses for switch/continue/pause, leaving natural-language interpretation to Mastra/orchestrator and applying only structured decision payloads?

- Do service inputs/outputs distinguish ready/missing/unknown/unavailable/not-applicable?
- Are provider API errors normalized and redacted?
- Does Meta Pixel extraction avoid token leakage in URLs/logs where provider-compatible?
- Are frontend-visible contract changes backward compatible?
- Are Google Ads GAQL queries and Meta Pixel fetches tested via mocks?
- Are docs links consistent with backend copy/tests?

### Security

- Are OAuth tokens and refresh tokens never exposed in frontend/chat/logs/Linear/QA docs?
- Are provider error bodies redacted?
- Are any new logs safe?
- Is there no live Supabase mutation or schema change without approval?
- If schema was added, are RLS/policies/indexes/triggers reviewed?

### Testing

- Do tests cover every acceptance criterion?
- Do tests prove no drafts persist on required account blockers or pause decisions?
- Do tests cover unknown/unavailable provider responses?
- Do existing Google/Facebook launch and parameter tests still pass?
- Are frontend tests updated if frontend contract changed?

### Docs

- Does `integrations/google-ads.mdx` explain connection/customer selection, conversion actions/readiness, and GBP readiness accurately?
- Does Meta warning still point to `/integrations/pixel-setup`?
- Do docs avoid claiming the future setup wizard is shipped?
- Are docs links/anchors valid, including `/integrations/google-ads#conversion-tracking-readiness` and `/integrations/google-ads#google-business-profile-readiness`?

### Conditional frontend/design/accessibility

Only run if visible frontend UI changes are made.

- Does UI follow Aventor patterns?
- Are response options clear, accessible, and wired to structured decision payloads rather than raw-text regex parsing?
- Did Final Review avoid unapproved readiness card/panel scope or redesign?
- Was the required launch-readiness UI design routed through local Open Design per `linear-executor`, with artifact/run provenance recorded, or blocked as `BLOCKED_DESIGN` if Open Design was unavailable?

### Conditional schema/data

Only run if schema/persistence changes are introduced.

- Is migration necessary for AVE-13, or should state remain in existing session metadata?
- Are RLS policies, indexes, constraints, and updated_at triggers complete?
- Are Supabase advisor warnings not made worse?
- Was no live migration applied without explicit approval?

## Reviewer artifact paths

Recommended artifacts:

```text
docs/linear/AVE-13/qa-artifacts/correctness-review.md
docs/linear/AVE-13/qa-artifacts/engineering-review.md
docs/linear/AVE-13/qa-artifacts/api-contract-review.md
docs/linear/AVE-13/qa-artifacts/security-review.md
docs/linear/AVE-13/qa-artifacts/testing-review.md
docs/linear/AVE-13/qa-artifacts/docs-review.md
```

Add conditional artifacts only when applicable:

```text
docs/linear/AVE-13/qa-artifacts/frontend-open-design-review.md
docs/linear/AVE-13/qa-artifacts/schema-data-review.md
```

Summarize reviewer outcomes in `qa-package/validation_results.md` and PR descriptions.

## Blocking finding policy

Treat blocker findings as repair work. After repairs:

- rerun impacted validation;
- rerun the impacted reviewer where practical;
- document final outcome in QA package.

Do not mark READY if security/API-contract/testing reviewers have unresolved blockers.
