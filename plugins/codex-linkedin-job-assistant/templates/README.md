# Codex LinkedIn Job Assistant Workspace

This is your local job-search workspace. Codex uses the files here to track jobs, discover roles, orchestrate referrals, prepare application materials, remember reusable screening answers, and guide applications through Chrome.

There is no database and no hidden service. The CSV and local folders are the whole system.

## Quick Start

### 1. Add your resume

Put one or more resume files in:

```text
resumes/
```

Supported sources:

- `.pdf`
- `.tex`
- `.docx`
- `.md`
- `.html`

If you have editable resume source, Codex can tailor a role-specific version. If you only have a PDF, Codex can use it as an attachment, but true tailoring requires editable source.

### 2. Add search preferences

Optional but useful:

```text
resumes/search_profile.md
```

Use it for must-haves, deal-breakers, locations, salary floor, target levels, industries, company stages, or anything your resume does not say.

### 3. Fill in application memory

Use:

```text
profile/personal_info.json
profile/screening_answers.md
```

Codex reads these before asking you repeated application questions. When a new answer becomes reusable, Codex should save it here.

### 4. Enable browser file access

For LinkedIn, referrals, and application forms, sign into LinkedIn in Chrome and connect the Codex Chrome extension.

For uploads, enable:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Allow access to file URLs
```

Codex should stop before file uploads if this is not enabled.

## Workflows

| Intent | What it does |
|---|---|
| `jobs setup` | Verify workspace files, tracker schema, resume backend, and browser preflight |
| `jobs check` | Show dashboard, deadlines, ready-to-apply jobs, and outreach queues |
| `jobs find` | Discover jobs or intake a pasted job link, then add qualified rows to the tracker |
| `jobs referral` | Handle replies, referral materials, email-vs-LinkedIn decisions, follow-ups, outreach, and deadlines |
| `jobs apply` | Prepare materials, tailor resume when possible, fill forms, upload files, and submit after approval |
| `jobs daily` | Run check, apply-ready, referral, find, instant no-referral apply, commit, and summary |

There is no standalone `jobs add` or `jobs update`. Manual job links go through `jobs find`, and status changes happen through workflow outcomes or direct CSV edits.

## Daily Flow

```text
1. jobs check
2. jobs apply        # ready jobs
3. jobs referral     # replies, follow-ups, outreach, deadlines
4. jobs find         # discovery and manual-link intake
5. instant apply     # newly found no-referral jobs, with approval
6. commit
7. summary
```

## Folder Layout

```text
job_tracker.csv
resumes/
base_resumes/
profile/
applications/
outreach/
```

Application folders use:

```text
applications/<YYYY-MM-DD>_<Company>_<Role>/
```

Typical contents:

- `job_description.md`
- `resume_source.<ext>`
- `resume.pdf`
- `cover_letter.md`
- `notes.md`
- `contacts.md`

## Tracker Schema

Do not change the header:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

## Privacy

Private files are ignored by default:

- Resumes
- Profile details
- Screening answers
- Application folders
- Outreach logs
- Base resume sources

Do not commit private job-search data unless you intentionally change `.gitignore`.
