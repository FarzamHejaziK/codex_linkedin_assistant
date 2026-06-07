# Daily Workflow

Use this for `jobs daily`.

## Sequence

Run in this order:

1. Setup/platform check when required
2. Preflight
3. Check
4. Apply ready jobs
5. Referral orchestrator
6. Find jobs
7. Instant-apply newly found no-referral jobs
8. Commit
9. Summary

Do not ask the user to choose internal subcommands. Ask for approval only at approval gates.

## Setup And Platform Check

If required setup files are missing, or `resumes/search_profile.md` has no platform configuration, run or resume `jobs setup` first.

LinkedIn remains mandatory. Indeed is optional:

- If Indeed is enabled, `jobs find` includes Indeed after the LinkedIn search pass.
- If Indeed is disabled or missing, `jobs find` uses LinkedIn plus web/company-board search only.
- If Indeed is blocked, logged out, or thin, continue the daily workflow with other sources and report the Indeed issue in the summary.

## Empty Tracker

If the tracker is empty, skip dashboard sections and start with `jobs find`.

## Apply Ready Jobs

Use the ready-to-apply logic from `tracker-schema.md`. If multiple ready rows exist, process highest priority first. Stop when the user declines, a blocker appears, or all selected applications are complete.

## Referral

Run the full referral orchestrator after the first apply sweep. Referral may produce new ready-to-apply rows. Summarize those at the end or apply them in the same run if the user explicitly approves continuing.

## Find And Instant Apply

Run `jobs find` with its LinkedIn-in-Chrome search pass first. General web search and company job boards are supplemental; they do not replace the LinkedIn search pass unless LinkedIn/Chrome is blocked and the user approves continuing without LinkedIn.

If Indeed is enabled in `resumes/search_profile.md`, run the Indeed lanes from `find.md` and `indeed.md` after LinkedIn. Indeed-discovered rows flow through the same dedupe, referral-needed, and instant-apply logic as other rows.

Any newly added row with `Referral Needed=NO` is an instant-apply candidate in the same daily run.

## Commit

If the workspace is a git repo, commit at the end only. Do not force-add ignored private data. If there are no changes, skip commit. If the workspace is not a git repo, report that commit was skipped.

Suggested message:

```text
daily <YYYY-MM-DD>: <short summary>
```

Include concise sections for applications, referral outcomes, new jobs, outreach, and generated folders when relevant.
