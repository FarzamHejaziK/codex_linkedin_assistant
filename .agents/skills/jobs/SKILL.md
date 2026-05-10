---
name: jobs
description: Run a prompt-first local job-search workspace in Codex, including setup, tracker checks, job discovery, referral orchestration, resume tailoring guidance, application submission guidance, and daily runs.
---

# Jobs Skill

Use this skill when the user asks for `jobs setup`, `jobs check`, `jobs find`, `jobs apply`, `jobs referral`, `jobs daily`, LinkedIn outreach, resume tailoring, application tracking, referral handling, or job-search workflow help.

This repository is prompt-first. It ships Markdown instructions and local workspace files only. Do not look for helper programs, automation scripts, browser libraries, or renderers owned by this repo. Use active Codex tools and the user's local environment at runtime.

## Canonical References

Design decisions live in:

- `.docs/PRD.md`
- `.docs/plan.md`

Operational workflow references live in:

```text
.agents/skills/jobs/references/
```

## Required Reference Loading

Before acting, read only the references needed for the user's intent:

| User intent | References to read |
|---|---|
| `jobs setup` | `references/setup.md`, `references/workspace-files.md`, `references/browser-preflight.md`, `references/resume-backends.md` |
| `jobs check` | `references/tracker-schema.md`, `references/check.md` |
| `jobs find` | `references/tracker-schema.md`, `references/find.md`, `references/browser-preflight.md`, `references/writing-style.md` |
| `jobs apply` | `references/tracker-schema.md`, `references/apply.md`, `references/resume-backends.md`, `references/browser-preflight.md`, `references/writing-style.md` |
| `jobs referral` | `references/tracker-schema.md`, `references/referral.md`, `references/browser-preflight.md`, `references/writing-style.md` |
| `jobs daily` | `references/tracker-schema.md`, `references/daily.md`, plus each referenced workflow as it runs |

If the user asks generally what this assistant does, read `references/overview.md`.

## Hard Rules

- `job_tracker.csv` is the source of truth for job state.
- Keep the tracker schema exactly as defined in `references/tracker-schema.md`.
- There is no standalone `jobs add` workflow. Manual job links are handled by `jobs find`.
- There is no standalone `jobs update` workflow. Status changes happen through workflow outcomes or direct CSV edits by the user.
- Never invent resume experience, credentials, metrics, companies, dates, or personal details.
- Keep all user-specific resume, profile, outreach, and application state local.
- Ask for explicit approval before sending messages, sending emails, submitting applications, or answering sensitive/ambiguous screening questions.
- File uploads are allowed only after Codex browser access is ready and Chrome extension file URL access has been enabled.
- Referral is an orchestrator, not a router.
