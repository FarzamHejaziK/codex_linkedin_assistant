# Daily Workflow

Use this for `jobs daily`.

## Sequence

Run in this order:

1. Preflight
2. Check
3. Apply ready jobs
4. Referral orchestrator
5. Find jobs
6. Instant-apply newly found no-referral jobs
7. Commit
8. Summary

Do not ask the user to choose internal subcommands. Ask for approval only at approval gates.

## Empty Tracker

If the tracker is empty, skip dashboard sections and start with `jobs find`.

## Apply Ready Jobs

Use the ready-to-apply logic from `tracker-schema.md`. If multiple ready rows exist, process highest priority first. Stop when the user declines, a blocker appears, or all selected applications are complete.

## Referral

Run the full referral orchestrator after the first apply sweep. Referral may produce new ready-to-apply rows. Summarize those at the end or apply them in the same run if the user explicitly approves continuing.

## Find And Instant Apply

Run `jobs find` with its LinkedIn-in-Chrome search pass first. General web search and company job boards are supplemental; they do not replace the LinkedIn search pass unless LinkedIn/Chrome is blocked and the user approves continuing without LinkedIn.

Any newly added row with `Referral Needed=NO` is an instant-apply candidate in the same daily run.

## Commit

If the workspace is a git repo, commit at the end only. Do not force-add ignored private data. If there are no changes, skip commit. If the workspace is not a git repo, report that commit was skipped.

Suggested message:

```text
daily <YYYY-MM-DD>: <short summary>
```

Include concise sections for applications, referral outcomes, new jobs, outreach, and generated folders when relevant.
