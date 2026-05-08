# Check Workflow

Use this for `jobs check`.

## Empty Tracker

If `job_tracker.csv` has no data rows, report:

```text
Your tracker is empty. Nothing to dashboard yet. Run jobs find to discover your first jobs, or paste a job link and I will intake it through jobs find.
```

Do not show empty section tables.

## Dashboard Sections

Render each non-empty section as a Markdown table. Use `None.` for empty sections only when the tracker has at least one row.

1. Ready to apply now
   - Rows matching the ready-to-apply logic from `tracker-schema.md`.
2. Referral deadlines approaching
   - `Referral Needed=YES`, non-terminal referral status, deadline within 2 days.
3. Referral deadlines passed
   - Non-terminal referral status and deadline on or before today.
4. Stale outreach needing follow-up
   - `Referral Status=Outreach Sent` and last outreach at least 3 days ago, before deadline.
5. Awaiting connection acceptance
   - `Referral Status=Connection Pending`.
6. Active outreach summary
   - Read `outreach/*_contacts.md` and summarize stages by company.
7. Recently applied and next actions
   - Applied rows with upcoming `Next Action`.

End with the smallest useful next action.
