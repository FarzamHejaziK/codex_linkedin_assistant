# Implementation Plan: Repo-First Codex LinkedIn Assistant

## 1. Direction

This repository should behave like a normal Codex workspace, not a separately installed package. The user opens the repo in Codex, and Codex follows `AGENTS.md` plus the project-local skill in `.agents/skills/jobs/`.

Keep v1 prompt-first:

- Markdown instructions.
- Markdown reference docs.
- Root workspace files and examples.
- No helper programs, automation scripts, app code, or custom renderers.

## 2. Repository Shape

Use this structure:

```text
AGENTS.md
README.md
REQUIREMENTS.md
job_tracker.csv
.gitignore
.docs/
  PRD.md
  plan.md
.agents/
  skills/
    jobs/
      SKILL.md
      references/
        overview.md
        setup.md
        tracker-schema.md
        browser-preflight.md
        resume-backends.md
        check.md
        find.md
        apply.md
        referral.md
        daily.md
        writing-style.md
        workspace-files.md
resumes/
  README.md
  search_profile.example.md
profile/
  personal_info.example.json
  screening_answers.example.md
base_resumes/
  README.md
applications/
  README.md
outreach/
  README.md
```

## 3. Root Contract

`AGENTS.md` is the project contract loaded by Codex. It must:

- Describe the repo as a generic Codex LinkedIn job-search assistant.
- Point to `.agents/skills/jobs/SKILL.md`.
- Name `.docs/PRD.md` and `.docs/plan.md` as canonical design docs.
- Define hard rules: tracker source of truth, no fabricated experience, approval gates, privacy, no standalone add/update.

## 4. Skill Contract

`.agents/skills/jobs/SKILL.md` is the workflow entrypoint. It must:

- Trigger on `jobs setup`, `jobs check`, `jobs find`, `jobs apply`, `jobs referral`, `jobs daily`, LinkedIn outreach, resume tailoring, referral handling, and application tracking.
- Load only the needed reference docs for each intent.
- Route manual job links through `jobs find`.
- Treat referral as an orchestrator.
- Keep all work generic and local.

## 5. Root Workspace Files

Root files replace templates:

- `job_tracker.csv` contains only the canonical header.
- `.gitignore` protects private search data.
- `README.md` explains installation-free Codex usage.
- `REQUIREMENTS.md` explains Codex, Chrome, file URL access, git, optional resume tooling, and CSV editors.
- Folder README/example files explain how each workspace area is used.

## 6. Workflow References

Move the existing Markdown workflow references to `.agents/skills/jobs/references/` and keep them prompt-only:

- `setup.md`: workspace setup and file URL preflight.
- `tracker-schema.md`: schema and canonical values.
- `browser-preflight.md`: mandatory startup Codex Chrome extension/tool connection and LinkedIn profile-match checks, explicit no-Computer-Use fallback rule, plus upload readiness.
- `resume-backends.md`: LaTeX, DOCX, Markdown/HTML, PDF-only.
- `check.md`: dashboard.
- `find.md`: discovery and manual-link intake.
- `apply.md`: application folders, resume material, forms, uploads, approval.
- `referral.md`: replies, materials, email-vs-LinkedIn, follow-ups, outreach, deadlines.
- `daily.md`: check, apply, referral, find, instant no-referral apply, commit.
- `writing-style.md`: outgoing-message and resume/cover-letter rules.
- `workspace-files.md`: folder layout and privacy defaults.

## 7. Validation Plan

After implementation:

- Confirm there is no package-only structure or marketplace metadata.
- Confirm `.agents/skills/jobs/SKILL.md` and references exist.
- Confirm root `AGENTS.md`, `README.md`, `REQUIREMENTS.md`, `.gitignore`, and `job_tracker.csv` exist.
- Confirm `job_tracker.csv` header exactly matches the schema.
- Search for stale metadata terms from the previous architecture and remove them.
- Search for stale assistant-source terms and private user strings.
- Confirm no real resume/profile/outreach/application data is committed.
- Confirm no `.py`, `.sh`, `.js`, `.ts`, or app-code files exist.

## 8. Git Plan

Commit with:

```text
repo: finalize Codex workspace assistant
```

Then push `main` to `origin`.
