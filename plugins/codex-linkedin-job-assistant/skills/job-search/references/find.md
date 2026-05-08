# Find Workflow

Use this for `jobs find` and for manual job-link intake. There is no standalone add workflow.

## Inputs

Read:

- Resume files in `resumes/`
- Optional `resumes/search_profile.md`
- Existing `job_tracker.csv`

If no resume exists, ask the user to add one before searching.

## Search Plan

From resume(s), infer:

- Target titles
- Adjacent titles
- Top skills and keywords
- Default location
- Short profile summary

From `search_profile.md`, overlay:

- Must-have locations
- Deal-breakers
- Salary floor
- Preferred levels
- Preferred industries or company stages
- Excluded industries or role shapes

When the resume and search profile conflict, the search profile wins.

## Discovery

Use available browser and web tools to find recent jobs. For LinkedIn, prefer searches scoped to recent postings and target locations. For manual intake, inspect the supplied URL and extract job details from that page.

Extract:

- Company
- Role
- Location
- Type
- Salary
- URL
- Why it fits

Deduplicate against the tracker. Drop hard exclusions before scoring.

## Scoring and Priority

Score candidates by title fit, location fit, keyword overlap, freshness, salary fit, and search-profile alignment.

Set priority:

- Strong match: `HIGH`
- Plausible match: `MEDIUM`
- Borderline but relevant: `LOW`

## Add To Tracker

For accepted or high-confidence candidates:

- `Status=To Apply`
- `Discovered Date=today`
- Fill company, role, location, type, salary, URL, priority, and notes.
- Determine referral need using company size/followers.
- Set referral fields from `tracker-schema.md`.

Show a summary table of added rows and a short list of deduped/skipped jobs.

If any newly added row has `Referral Needed=NO`, return it as an instant-apply candidate for `jobs daily`.
