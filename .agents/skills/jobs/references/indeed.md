# Indeed Integration

Use this reference when `jobs setup`, `jobs find`, or `jobs daily` involves optional Indeed support, and when the user asks to enable, retry, or optimize Indeed.

Indeed expands discovery. It does not replace LinkedIn. LinkedIn remains the required primary source for `jobs find` unless LinkedIn/Chrome is blocked and the user explicitly approves continuing without LinkedIn.

## Platform State

Indeed is disabled by default. Store the user's choice in the local, gitignored `resumes/search_profile.md`.

If the file has no platform configuration, `jobs setup` should create or merge:

```markdown
## Platforms
- LinkedIn: yes
- Indeed: no

## Indeed
- Discovery: disabled
- Profile optimization: skipped
```

Enable Indeed when either of these is present:

```markdown
- Indeed: yes
```

or:

```markdown
- Discovery: enabled
```

Allowed values:

- `Discovery: enabled | disabled`
- `Profile optimization: completed | skipped | later | blocked`

Never store login credentials, cookies, tokens, raw recommendation lists, account IDs, or resume-derived Indeed profile prose in committed files.

## Setup Opt-In

During `jobs setup`, ask whether the user wants Indeed as an additional job source.

If the user says no or later:

- Preserve existing search preferences.
- Write `Indeed: no` and `Discovery: disabled`.
- Do not open or modify Indeed.
- Continue setup.

If the user says yes:

1. Use the same Codex Chrome extension/profile path as the LinkedIn preflight.
2. Open `https://www.indeed.com/`.
3. Verify the user is signed in.
4. If signed in, write `Indeed: yes` and `Discovery: enabled`.
5. If not signed in, ask the user to sign into Indeed in that Chrome profile and reply `done`, then re-check.
6. If CAPTCHA, block, blank page, or unusual verification appears, write `Profile optimization: blocked` when relevant, leave discovery disabled unless already enabled, and continue the rest of setup.

After enabling discovery, ask separately whether the user wants Indeed profile optimization. Profile optimization is optional; declining it must not disable Indeed discovery.

## Indeed Profile Optimization

Indeed profile optimization edits public-facing profile fields. It is never automatic.

Source material:

- Resume files in `resumes/`
- `resumes/search_profile.md`
- `profile/personal_info.json`, only for user-provided application fields such as phone, email, location, links, work authorization, sponsorship, salary, and availability

Rules:

- Build proposed edits from user-provided local sources only.
- Do not invent experience, credentials, certifications, languages, salary, work authorization, dates, employers, or availability.
- Do not upload or delete resumes unless the user explicitly asks and approves.
- Do not delete the structured Indeed Resume. Deletion is destructive and user-only.
- Show proposed changes by section before saving.
- Ask explicit approval once per section/save, not once per individual field.
- If a field is unclear, ask or skip it.

Supported sections:

- Contact/header fields, only if resume-backed, profile-backed, or already visible on Indeed.
- Headline, derived from current/target role and strongest resume keywords.
- Summary/About, converted from resume summary into concise first-person profile text.
- Work experience, from resume titles, companies, dates, and bullets.
- Education, from resume.
- Skills, from the resume skills section and repeated resume keywords.
- Qualifications, from resume-backed current role, education, and 15-25 high-signal skills.
- Job preferences, from `resumes/search_profile.md` and `profile/personal_info.json`.
- Ready to Work, only if the user explicitly asks or their setup profile clearly says immediate availability.
- Visibility, only after reading the current setting and confirming the user's privacy choice.

On CAPTCHA/block while saving:

- Stop the Indeed profile step.
- Ask the user to complete the human check if appropriate.
- If the user cannot complete it or the block persists, set `Profile optimization: blocked`.
- Continue non-Indeed workflows when possible.

## Find Workflow

When Indeed is enabled, `jobs find` should run Indeed after the LinkedIn search pass.

### Lane A - Active Search

For each target role and allowed location:

```text
https://www.indeed.com/jobs?q=<role>&l=<location>&fromage=7&sort=date
```

Also search Remote when the search profile allows remote work.

Extract:

- Company
- Role
- Location
- Type when visible
- Salary when visible
- URL
- Match clues or visible labels

Tag source as `Indeed Search`.

### Lane B - Recommendation Harvest

Best effort only. Open logged-in Indeed home/search surfaces and look for visible personalized modules such as:

- Jobs for you
- Because of your profile
- Profile match
- Based on your qualifications

Extract visible job details and tag source as `Indeed Recommendation`. If nothing useful appears, move on.

### Lane C - Career Scout Intake

Career Scout is mobile-app only and cannot be automated from Chrome.

If the user has Career Scout recommendations, they may paste links or company/title pairs into chat. Treat those as user-fed personalized recommendations:

- Tag source as `Career Scout`.
- Resolve a working URL if missing.
- Deduplicate against the tracker.
- Verify company site when possible.
- Score and add through the normal `jobs find` pipeline.

## Candidate Handling

Indeed candidates are leads, not truth.

For candidates that pass hard filters:

1. Deduplicate against `job_tracker.csv`.
2. Score using the normal `jobs find` scoring.
3. Add a small bonus for `Indeed Recommendation` or `Career Scout`, but never let the bonus rescue a poor fit.
4. Search the company careers site for the same company and role when possible.
5. If a live company posting exists, prefer the company apply URL and add `Verified company site from Indeed discovery` to notes.
6. If only Indeed is live and credible, keep the Indeed URL, set `Apply Via=Indeed`, and add `Indeed-only posting; verify before apply` to notes.
7. If the company site contradicts the Indeed posting or the posting looks stale, drop it.

Preserve source labels in tracker notes:

- `Indeed Search`
- `Indeed Recommendation`
- `Career Scout`
- `Verified company site`
- `Indeed-only posting; verify before apply`

## Failure Policy

Indeed is fail-soft:

- CAPTCHA, login failure, block, 403, or blank page stops the current Indeed lane only.
- Do not try to bypass verification or solve CAPTCHA automatically.
- Continue LinkedIn, web/company-board search, referral, apply, tracker updates, and daily summary when possible.
- Report `Indeed blocked after <N> results` when partial results exist.
