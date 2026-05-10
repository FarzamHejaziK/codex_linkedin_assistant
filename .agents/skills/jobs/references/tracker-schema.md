# Tracker Schema

`job_tracker.csv` must keep this exact header:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

## Canonical Values

- `Priority`: `HIGH`, `MEDIUM`, `LOW`
- `Status`: `To Apply`, `Applied`, `Recruiter Call`, `Phone Screen`, `Onsite`, `Offer`, `Rejected`, `Withdrew`
- `Referral Needed`: `YES`, `NO`
- `Referral Status`: `Not Needed`, `Outreach Pending`, `Connection Pending`, `Outreach Sent`, `Got Referral`, `Declined`, `No Referral`
- `Apply Via`: `LinkedIn Easy Apply`, `Company Website`, `Blocked`, or a concise site/method label

Dates use `YYYY-MM-DD`.

## Rules

- Preserve untouched rows and fields exactly.
- Render tracker data as Markdown tables, not raw CSV.
- Every tracked job must have a URL.
- Deduplicate by exact URL first, then by similar company, role, and location.
- `Referral Deadline` defaults to `Discovered Date + 5 days` when `Referral Needed=YES`.

## Referral Needed

Set `Referral Needed=YES` when company size is greater than 500 employees or the company has more than 100K LinkedIn followers. Otherwise set `NO`.

When `Referral Needed=YES`:

- `Referral Status=Outreach Pending`
- `Referral Deadline=Discovered Date + 5 days`

When `Referral Needed=NO`:

- `Referral Status=Not Needed`
- `Referral Deadline` may be blank

## Ready To Apply

A row is ready to apply when:

```text
Status=To Apply
AND (
  Referral Needed=NO
  OR Referral Status=Got Referral
  OR Referral Status=No Referral
  OR Referral Status=Declined
  OR Referral Deadline <= today
)
```

If a referral deadline has passed and the referral is still pending, set `Referral Status=No Referral` before applying.
