# AGENTS.md - Codex Job Search Assistant

This repository develops a local Codex job-search plugin and retains a compatible workspace entrypoint. It is opened directly in Codex and uses local Markdown instructions, local workspace files, and browser tooling.

## Primary Skill

Use the project-local skill:

```text
.agents/skills/jobs/SKILL.md
```

That file forwards to the canonical skill in `plugins/job-search-assistant/skills/jobs/SKILL.md`. Edit operational instructions there; legacy reference paths are forwarding documents.

That skill is the operational entrypoint for:

- `jobs setup`
- `jobs check`
- `jobs find`
- `jobs apply`
- `jobs referral`
- `jobs daily`
- `jobs indeed-setup`
- Indeed discovery and profile optimization
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
5. Require explicit scoped approval before uploads, messages, emails, connection requests, application submissions, public profile saves, or sensitive/ambiguous screening answers. Prepare a concrete review first; honor approval already given for that exact action.
6. Before browser-dependent work, select and verify an available browser using `.agents/skills/jobs/references/browser-preflight.md`. Default to the Codex built-in browser; honor an explicitly selected browser/tab. Verify LinkedIn identity against the resume before LinkedIn work, and the relevant account for other authenticated steps. Local work needs no browser preflight.
7. Codex browser-bound Computer Use supports the built-in browser and connected Chrome. Verify live state in the selected browser; unrelated desktop screenshots and undocumented automation do not substitute for account checks. If a browser step is blocked, checkpoint it and continue useful local work.
8. Use plain browser names in user-facing messages: Codex built-in browser or Chrome. Keep internal tool namespaces out of routine messages.
9. Discover and check the selected browser before declaring it unavailable. Chrome is optional. Verify capabilities per step; use a manual upload handoff when built-in uploads are unsupported, and reverify accounts after switching browsers.
10. Discovery in `jobs find` runs LinkedIn Jobs first in the selected browser. Web search, company boards, and optional Indeed supplement it; skipping blocked LinkedIn requires approval. Manual link intake inspects the supplied job without an unrelated search pass.
11. There is no standalone `jobs add`; manual job links go through `jobs find`.
12. There is no standalone `jobs update`; status changes happen through workflow outcomes or direct CSV edits.
13. Referral is an orchestrator, not a router.
14. Indeed is optional and disabled by default. Follow `.agents/skills/jobs/references/indeed.md` for Indeed discovery, Career Scout intake, and profile optimization.
15. This repo is prompt-first. Plugin/marketplace manifests and empty templates are supported; do not add helper programs, hooks, automation scripts, app code, servers, or custom renderers for v1.

16. Accept natural-language requests using `.agents/skills/jobs/references/experience.md`; respect time/count limits and prepare-only scope.
17. Keep the working `job_tracker.csv` private and ignored. Initialize it from the bundled header-only template during authorized first-use setup or explicit recovery; never silently replace missing registered state. No automatic daily commits.
18. Save multi-step progress in ignored `profile/session.md`; reconcile confirmed outcomes before resuming or retrying.

19. Before every operational workflow, resolve the saved local workspace using `plugins/job-search-assistant/skills/jobs/references/local-persistence.md`. Resolve data paths from that workspace and instruction/template paths from the skill. Never store user data in an installed plugin or assume the current chat directory is the data workspace.

## Key Files

- `job_tracker.csv` - private local dashboard and source of truth
- `job_tracker.example.csv` - public header-only template
- `resumes/` - resume files and optional search profile
- `profile/` - application memory examples and local user profile files
- `base_resumes/` - optional editable resume sources
- `applications/` - generated per-application materials
- `outreach/` - per-company contact logs
- `plugins/job-search-assistant/` - self-contained public plugin package
- `.agents/skills/jobs/` - compatible repository entrypoint and forwarding references
- `.agents/plugins/marketplace.json` - local plugin catalog
- `profile/workspace.json` - ignored workspace identity marker
