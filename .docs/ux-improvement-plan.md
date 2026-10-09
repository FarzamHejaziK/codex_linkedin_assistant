# Job Search UX Improvement Plan

## Scope

Improve the existing prompt-first experience using Markdown instructions, examples, and Git ignore rules. Preserve all 16 tracker columns and existing workflow commands. No app code, helper scripts, new service, or public publishing is required.

## Implementation Order

1. **Fast onboarding and natural language.** Route ordinary requests to existing workflows. Collect a resume, target roles, location, and dealbreakers first. Defer optional platforms, application details, and backend decisions until needed. Distinguish local readiness from browser readiness.
2. **Action-first dashboard.** Show up to three deduplicated actions with reasons, deadlines, and blockers. Show full queues only when requested. Local checks must work without browser access or a resume.
3. **Scoped, resumable daily work.** Respect time, count, and prepare-only requests. Default to a small reviewable batch. Save private progress, reconcile it with the tracker on resume, and verify uncertain external outcomes before retrying.
4. **Explainable matches and approvals.** Show fit evidence, gaps, unknowns, and the next step. Record explicit preference feedback without silently broadening exclusions. Use one review format for uploads, submissions, messages, and connection requests.
5. **Private working data.** Publish a header-only tracker template; retain `job_tracker.csv` locally and remove it from Git tracking. Ignore all private outreach formats. Daily work must never automatically commit personal state or unrelated changes.
6. **Documentation and validation.** Align root rules, skill routing, workflow references, README, PRD, and implementation plan. Review representative user journeys and verify schema, ignore behavior, and the diff.

## Acceptance Scenarios

- A first-time user provides a resume and three search preferences, then can proceed to first matches without completing application intake or Indeed setup.
- “What needs my attention?” reads local state without browser preflight, shows at most three distinct actions, and handles an empty tracker without empty tables.
- “Prepare three applications” routes to apply in prepare-only mode; no upload, send, or submit occurs.
- “I have 15 minutes” scopes daily work; elapsed time is best effort, with checkpoints between operations and a useful summary at the limit.
- A disconnected browser blocks only browser-dependent steps. Local preparation continues and reports exactly what remains blocked.
- “Continue” reconciles saved progress with confirmed tracker/log outcomes; uncertain submissions or sends are inspected before any retry.
- Every match explains evidence, gaps, and unknowns; explicit feedback changes future preferences without invented candidate facts.
- Every external action has a concrete review and scoped approval; changes to payload, destination, or attachments invalidate that approval.
- A fresh clone creates the local tracker from the template. Existing trackers are never overwritten. Private state is ignored, and the canonical header is unchanged.

## Delivery

Implement locally, inspect the complete diff, and record validation results here. Git history is not rewritten; users who previously published personal tracker rows may need separate history cleanup.

## Completed Implementation and Validation — 2026-10-08

All six implementation steps are complete in the local checkout. Root rules, skill routing, workflow references, README, requirements, PRD, and the implementation plan reflect the new behavior.

Repository checks passed:

- Working tracker is byte-for-byte equal to its pre-change contents and remains on disk; its Git index entry is removed and it is ignored.
- Public template contains only the original header. All 16 columns match the schema, README, and PRD.
- Private session/profile files, resumes, application folders, base sources, and Markdown/CSV/nested outreach files are ignored; public examples remain eligible for version control.
- A temporary fresh Git workspace successfully initializes an ignored tracker from the public template.
- Skill references and README/plan local links resolve.
- Working and staged diffs pass whitespace checks. No helper/app code or private candidate data was added.

Instruction walkthroughs completed (these are document reviews, not live browser tests):

| Scenario | Resulting instruction path |
|---|---|
| First-time setup | Resume plus three search essentials; application fields and Indeed deferred |
| Local dashboard / empty tracker | No preflight; up to three distinct actions or one setup/discovery next step |
| Prepare three applications | Apply preparation stops before forms/uploads; pending referrals stay intact |
| Fifteen-minute daily run | Scope applies across phases; elapsed time checked between operations; checkpoint at limit |
| Browser disconnected | Browser phase pauses; available local preparation continues within scope |
| Resume after uncertain submission | Reconcile tracker and evidence; inspect outcome before retrying |
| Negative match feedback | Explicit durable feedback updates preferences; a one-job skip stays narrow |
| Upload/message/application review | Exact destination, content, and file versions; scoped approval and confirmation evidence |
| Existing or fresh tracker | Preserve existing rows; initialize only when missing; private working file stays ignored |

Live LinkedIn, upload, messaging, and submission flows were not executed. At the end of the initial implementation, changes were local and uncommitted; no push or Git history rewrite had been performed.


## Browser Integration Follow-Up — 2026-10-08

Use the Codex built-in browser by default and connected Chrome as an optional path. Explicit browser/tab selection takes precedence. Replace Chrome-only preflight with live browser, relevant-account, and per-step capability checks. Keep LinkedIn-first discovery, scoped approvals, and private local state.

For built-in resume attachments, use a manual handoff under the currently documented upload limitation: retain the relevant form tab, link the file, save the pending step, and verify attachment/account state on return. An optional Chrome path must reverify identity and reconstruct form state; never imply shared sessions or bypass platform blocks.

Official source: [OpenAI Browser documentation](https://learn.chatgpt.com/docs/browser?surface=app), checked 2026-10-08. The active built-in browser opened and read this public page successfully. Shared runtime file-chooser documentation was inspected, but no private upload was attempted and browser-specific upload support was not established. LinkedIn sign-in, ATS submission, and Chrome fallback remain untested live.

Browser integration validation passed: no obsolete Chrome-only requirements remain in current workflow/design docs; updated local links and reference paths resolve; working tracker/template and private ignore rules remain intact; staged and working diff checks pass. Instruction walkthroughs cover built-in-only access, explicit Chrome selection, separate sign-in, unsupported upload handoff, and identity revalidation after switching. Only the public-page browser read was tested live.
