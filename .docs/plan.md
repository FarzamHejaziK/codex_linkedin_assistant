# Implementation Plan: Prompt-First Job Search Assistant

## Current Direction

The repository develops the self-contained local plugin at plugins/job-search-assistant/. Keep AGENTS.md and .agents/skills/jobs/ as compatible repository entrypoints. Canonical workflow instructions and templates live inside the plugin; persistent candidate data lives in a separately resolved user workspace. Preserve existing commands and the exact tracker schema. No helper programs, scripts, app code, custom renderers, or separate service.

Local packaging, persistence, and installation are specified in [local-plugin-plan.md](local-plugin-plan.md).

The current UX work is specified in [ux-improvement-plan.md](ux-improvement-plan.md), including implementation order, acceptance scenarios, and validation results.

## Instruction Surfaces

- AGENTS.md: privacy, scoped approvals, browser-only preflight, and source-of-truth rules.
- Packaged SKILL.md / local-persistence.md: workspace resolution, natural-language routing, and required reference loading.
- experience.md: intent mapping, scope, local readiness, and preference feedback.
- setup.md / overview.md: progressive first-run guidance.
- check.md: three recommended actions, full dashboard on request.
- find.md: LinkedIn-first discovery, manual links, fit evidence, and limits.
- apply.md / referral.md / approvals.md: preparation, concrete reviews, execution evidence.
- daily.md / session.md: scoped orchestration and reliable continuation.
- browser-preflight.md / indeed.md / resume-backends.md: capability-specific rules, built-in browser default, optional Chrome, relevant account verification, and manual upload handoffs where required.
- tracker-schema.md / workspace-files.md: unchanged schema, private tracker, safe migration.
- writing-style.md: outgoing copy and truthful materials.

## Public and Private Files

Publish job_tracker.example.csv with the canonical header only. Authorized first-use setup creates ignored job_tracker.csv from the bundled template; missing registered files require recovery. Retain any existing working file when removing its index entry. Ignore real resumes, profile/session files, application materials, all outreach formats, and base resume sources. Never force-add private files or automatically commit daily state. Do not rewrite history as part of this change.

## Validation

Read changed instructions end to end; walk representative onboarding, local-check, prepare-only, limited-daily, interrupted-resume, feedback, and approval scenarios. Inspect diff for contradictions, private data, and unintended changes. Verify the public header equals the original schema, the working tracker contents are unchanged, private paths are ignored, and a fresh copy can initialize the tracker from the template. No live job-search or sending/submission is needed for this documentation change.

## Delivery

Work locally and provide the completed plan and validation evidence. Contributions go through a branch/PR when publication is requested. Do not push directly to main or publish private data.
