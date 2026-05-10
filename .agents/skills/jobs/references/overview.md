# Overview

This assistant turns a local folder into a job-search workspace managed by Codex. It is generic and user-configurable. It must not assume a specific candidate, role, company list, location, email, resume format, or application history.

## Core Model

- `job_tracker.csv` tracks every job.
- `resumes/` holds the user's resume files and optional search profile.
- `profile/` holds reusable application details and screening answers.
- `applications/` holds generated per-job materials.
- `outreach/` holds per-company referral/contact logs.
- `base_resumes/` optionally holds editable resume sources or templates.

## Primary Workflows

- `jobs setup`: initialize a workspace from templates and run preflight.
- `jobs check`: show the dashboard and queues.
- `jobs find`: discover jobs or intake a pasted job link.
- `jobs apply`: tailor materials, fill forms, upload files, and update tracker after confirmation.
- `jobs referral`: orchestrate warm replies, referral material sends, follow-ups, connection checks, outreach, and deadline enforcement.
- `jobs daily`: run check, apply-ready, referral, find, instant no-referral apply, commit, and summary.

## First-Run Guidance

When the user asks "what should I do?", "how should I work with this?", or similar first-run questions, do not give only a generic checklist. Inspect the workspace and guide the next concrete action.

If no real resume is present in `resumes/`, the next action is resume intake. Say:

```text
First, upload your resume here or paste its absolute file path.

I will copy it into resumes/ for you, then I will ask a few setup questions and create the search/profile files myself.
```

If a resume exists but `resumes/search_profile.md` or profile files are missing, offer guided setup:

```text
I can create the setup files for you. I will ask a few questions about target roles, locations, salary, work authorization, sponsorship, and dealbreakers, then write the files into the repo.
```

Never tell the user to manually create `resumes/search_profile.md`, `profile/personal_info.json`, or `profile/screening_answers.md` as the primary path. Manual editing is optional, not the default.

## Runtime Expectations

This repo ships prompts and templates only. Use Codex's active local tools, browser tools, and available user-local programs as needed. If a required runtime capability is unavailable, explain the blocker and offer the smallest manual handoff.
