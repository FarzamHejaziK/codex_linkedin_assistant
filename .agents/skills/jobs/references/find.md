# Find Workflow

Use this for `jobs find` and for manual job-link intake. There is no standalone add workflow.

## Inputs

Read:

- Resume files in `resumes/`
- Optional `resumes/search_profile.md`
- Existing `job_tracker.csv`

If no resume exists, ask the user to add one before searching.

Check `resumes/search_profile.md` for platform configuration:

- LinkedIn is mandatory and primary.
- Indeed is enabled only when the search profile contains `Indeed: yes` in `## Platforms` or `Discovery: enabled` in `## Indeed`.
- If neither is present, skip Indeed entirely.
- If no platform configuration exists, continue with LinkedIn + web/company-board search and suggest `jobs setup` to configure optional platforms.

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

Use LinkedIn in Chrome as the primary discovery source after browser preflight passes. Do not skip LinkedIn search and satisfy `jobs find` only with general web search unless LinkedIn/Chrome preflight is blocked or the user explicitly asks to avoid LinkedIn.

Run LinkedIn job searches for the inferred target titles and target locations. Prefer recent postings and filter/sort for relevance, recency, and target location when available.

Use web search and direct company job-board pages as supplemental sources after the LinkedIn search pass, or as fallback sources when LinkedIn/Chrome is blocked and the user approves continuing without LinkedIn.

If Indeed is enabled, run the Indeed lanes from `indeed.md` after the LinkedIn search pass. Indeed is additional discovery; it does not replace LinkedIn.

For manual intake, inspect the supplied URL and extract job details from that page.

## LinkedIn Search Pass

For each `jobs find` run:

1. Complete startup preflight from `browser-preflight.md`.
2. Open LinkedIn Jobs in Chrome.
3. Search 2-4 target title queries inferred from the resume/search profile.
4. Apply location/remote filters from `resumes/search_profile.md` when available.
5. Prefer recent postings, such as past 24 hours or past week, when the result volume allows it.
6. Inspect enough results to collect qualified candidates, not just the first page title list.
7. Capture the canonical LinkedIn job URL or the apply URL when LinkedIn redirects to a company site.
8. Deduplicate against `job_tracker.csv`.

If LinkedIn search cannot run, say why clearly and ask whether to continue with web/company-board search only. Do not silently replace LinkedIn search with web search.

Extract:

- Company
- Role
- Location
- Type
- Salary
- URL
- Why it fits

Deduplicate against the tracker. Drop hard exclusions before scoring.

## Indeed Search Pass

Run this section only when Indeed is enabled in `resumes/search_profile.md`.

Follow `indeed.md`:

1. Run active Indeed searches for the same target titles and allowed locations.
2. Include Remote when the search profile allows remote work.
3. Harvest logged-in Indeed recommendations only as a best-effort bonus lane.
4. Accept pasted Career Scout recommendations as user-fed personalized recommendations.
5. Stop the current Indeed lane on CAPTCHA, verification, 403, blank page, or platform block.
6. Continue LinkedIn, web/company-board search, tracker updates, and daily workflow when Indeed is blocked.

Tag source labels in candidate notes:

- `Indeed Search`
- `Indeed Recommendation`
- `Career Scout`

## Scoring and Priority

Score candidates by title fit, location fit, keyword overlap, freshness, salary fit, and search-profile alignment.

For Indeed candidates:

- Add a small bonus for `Indeed Recommendation` or `Career Scout` source, but never let that bonus rescue a weak fit.
- Verify the same company/role on the company careers site when possible.
- Prefer the company careers URL when a live posting exists.
- Keep the Indeed URL only when it is live and credible and no better company URL is found.

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
- For Indeed-only jobs, set `Apply Via=Indeed` and add `Indeed-only posting; verify before apply` to notes.
- For Indeed-discovered jobs verified on the company site, prefer `Apply Via=Company Website` and add `Verified company site from Indeed discovery` to notes.

Show a summary table of added rows and a short list of deduped/skipped jobs.

If any newly added row has `Referral Needed=NO`, return it as an instant-apply candidate for `jobs daily`.
