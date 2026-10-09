---
name: jobs
description: Manage a local job-search workspace in Codex or ChatGPT Work, including setup, tracker checks, discovery, application preparation, referrals, and resuming saved progress. Requires access to the user's local files.
---

# Jobs Skill

Use this skill when the user asks for `jobs setup`, `jobs check`, `jobs find`, `jobs apply`, `jobs referral`, `jobs daily`, `jobs indeed-setup`, Indeed setup, Indeed profile optimization, LinkedIn outreach, resume tailoring, application tracking, referral handling, or job-search workflow help.

This local plugin is prompt-first. It ships Markdown instructions, manifests, presentation assets, and generic templates. Do not look for helper programs, automation scripts, browser libraries, or renderers owned by this repo. Use available tools and the user's local environment in Codex or ChatGPT Work. Installing the plugin does not grant access to local files, browsers, or accounts. If the selected environment cannot access the user's computer, explain the limitation before workspace setup; never initialize a replacement in a cloud sandbox.

## Workspace Resolution

Before every operational request, read `references/local-persistence.md` and resolve the stable user workspace. This applies even to local checks and continuation. All data paths below are relative to that workspace; references and empty templates are relative to this skill directory. Never rely on the chat's current directory or store personal data in the installed plugin.

The source repository also supplies a forwarding `.agents/skills/jobs/SKILL.md` entrypoint. If both it and the installed plugin are available, execute the workflow once. Repository design docs are for development, not required runtime inputs.

## Conversation Contract

Read `references/experience.md` for every job-search request. Natural language maps to existing workflows; commands are optional shortcuts. Read `references/session.md` for multi-step work or continuation, and `references/approvals.md` before any external action. Local work needs only its relevant source files; browser-dependent steps require preflight. Read `references/workspace-files.md` before initializing a tracker or migrating privacy settings.

## Required Reference Loading

Before acting, read only the references needed for the user's intent:

| User intent | References to read |
|---|---|
| `jobs setup` | `references/setup.md`, `references/workspace-files.md`; load browser, resume-backend, and Indeed references only when those steps are needed |
| `jobs check` | `references/tracker-schema.md`, `references/check.md`, `references/workspace-files.md` |
| `jobs find` | `references/tracker-schema.md`, `references/find.md`, `references/indeed.md`, `references/browser-preflight.md`, `references/writing-style.md` |
| `jobs apply` | `references/tracker-schema.md`, `references/apply.md`, `references/indeed.md`, `references/resume-backends.md`, `references/browser-preflight.md`, `references/writing-style.md` |
| `jobs referral` | `references/tracker-schema.md`, `references/referral.md`, `references/browser-preflight.md`, `references/writing-style.md` |
| `jobs daily` | `references/tracker-schema.md`, `references/daily.md`, `references/indeed.md`, `references/browser-preflight.md`, plus each referenced workflow as it runs |
| `jobs indeed-setup` or Indeed setup/optimization | `references/indeed.md`, `references/setup.md`, `references/browser-preflight.md`, `references/resume-backends.md`, `references/writing-style.md` |

If the user asks generally what this assistant does, how to start, what to do next, or appears to be in first-run onboarding, read `references/overview.md` and `references/setup.md`.

## Hard Rules

- `job_tracker.csv` is the source of truth for job state.
- Keep the tracker schema exactly as defined in `references/tracker-schema.md`.
- Before browser-dependent steps, use `references/browser-preflight.md`: prefer the Codex built-in browser, honor explicit browser/tab choices, verify the relevant account, and check required capabilities. Local setup, checks, and saved-source preparation need no browser.
- LinkedIn is mandatory and primary. Indeed is optional, disabled by default, and governed by `references/indeed.md`.
- Codex browser-bound Computer Use is supported for the built-in browser and connected Chrome. Verify LinkedIn identity in the selected browser before LinkedIn work. Checkpoint blocked steps and continue useful local work; never evade site restrictions by switching browsers.
- Use plain browser names and avoid internal tool namespaces in routine user-facing messages.
- Chrome is optional. Try the selected browser connection before declaring it unavailable; do not require extension setup when the built-in browser works.
- There is no standalone `jobs add` workflow. Manual job links are handled by `jobs find`.
- There is no standalone `jobs update` workflow. Status changes happen through workflow outcomes or explicit user-directed CSV corrections.
- Never invent resume experience, credentials, metrics, companies, dates, or personal details.
- Keep all user-specific resume, profile, outreach, and application state local.
- Follow `references/approvals.md` for concrete, scoped reviews before uploads, sends, connection requests, submissions, and public profile edits; obtain explicit answers to sensitive/ambiguous screening questions.
- Uploads require scoped approval and supported capabilities. Built-in uploads default to a manual handoff under current documented limits; Chrome-specific file access settings apply only when using Chrome. Re-read attachment state before submission.
- Referral is an orchestrator, not a router.
- First-run setup collects resume, roles, location, and dealbreakers; defer application fields and optional platforms. Create local files for the user.
- Keep `job_tracker.csv` ignored and initialize it from the bundled header-only template only during authorized first-use setup or explicit recovery. Do not automatically commit daily work.
- Use workspace-root `profile/session.md` for progress; the tracker remains the source of truth for job state. Read the saved workspace pointer in every new chat.
- Indeed profile optimization edits public-facing fields. Show proposed section changes and ask approval before each section save.
