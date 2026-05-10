# AGENTS.md - Codex LinkedIn Job Assistant

This repository is a generic Codex job-search workspace assistant. It is opened directly in Codex and uses local Markdown instructions, local workspace files, and browser tooling.

## Primary Skill

Use the project-local skill:

```text
.agents/skills/jobs/SKILL.md
```

That skill is the operational entrypoint for:

- `jobs setup`
- `jobs check`
- `jobs find`
- `jobs apply`
- `jobs referral`
- `jobs daily`
- LinkedIn outreach
- Resume tailoring
- Referral handling
- Application tracking

## Canonical Design Docs

Design decisions live in:

- `.docs/PRD.md`
- `.docs/plan.md`

Use these when changing the assistant itself.

## Hard Rules

1. `job_tracker.csv` is the source of truth for job state.
2. Preserve the tracker schema exactly.
3. Keep private user data local and gitignored by default.
4. Never fabricate resume experience, credentials, metrics, companies, dates, or personal details.
5. Ask for explicit approval before sending messages, sending emails, submitting applications, or answering sensitive/ambiguous screening questions.
6. At the start of every operational `jobs ...` workflow, confirm Codex can connect to Chrome and the active LinkedIn profile matches the resume name. If the resume is missing, do resume intake first.
7. There is no standalone `jobs add`; manual job links go through `jobs find`.
8. There is no standalone `jobs update`; status changes happen through workflow outcomes or direct CSV edits.
9. Referral is an orchestrator, not a router.
10. This repo is prompt-first. Do not add helper programs, automation scripts, app code, or custom renderers for v1.

## Key Files

- `job_tracker.csv` - main dashboard and source of truth
- `resumes/` - resume files and optional search profile
- `profile/` - application memory examples and local user profile files
- `base_resumes/` - optional editable resume sources
- `applications/` - generated per-application materials
- `outreach/` - per-company contact logs
- `.agents/skills/jobs/references/` - workflow reference docs
