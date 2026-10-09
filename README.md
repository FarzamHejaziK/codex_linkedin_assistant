<p align="center">
  <img src="assets/logo-animation.gif" width="600" alt="Codex Job Search Assistant" />
</p>

<p align="center">
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-green.svg"></a>
  <img alt="Codex App" src="https://img.shields.io/badge/Codex%20App-Ready-111827">
  <img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-via%20Codex%20Browser-0A66C2?logo=linkedin&logoColor=white">
  <img alt="Prompt First" src="https://img.shields.io/badge/Prompt--First-Markdown-blue">
  <a href="CONTRIBUTING.md"><img alt="PRs Welcome" src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg"></a>
  <img alt="Maintained" src="https://img.shields.io/badge/Maintained-yes-success">
  <a href="SECURITY.md"><img alt="Security policy" src="https://img.shields.io/badge/Security-policy-blue"></a>
  <img alt="Local workspace storage" src="https://img.shields.io/badge/Workspace-local%20files-success">
</p>

<p align="center">
  <a href="#prerequisites"><strong>Prerequisites</strong></a> ·
  <a href="#quick-start"><strong>Quick Start</strong></a> ·
  <a href="#how-it-works"><strong>How it works</strong></a> ·
  <a href="#what-it-does"><strong>What it does</strong></a> ·
  <a href="#daily-flow"><strong>Daily flow</strong></a> ·
  <a href="#privacy"><strong>Privacy</strong></a> ·
  <a href="#contributing"><strong>Contributing</strong></a>
</p>

A local job-search assistant for Codex and ChatGPT Work. Track jobs in a single CSV, discover openings via LinkedIn + web search + optional Indeed, orchestrate referrals, tailor resumes, prepare application materials, and keep application state in one local workspace.

> **Scope on purpose:** Codex handles the repetitive organization, drafting, browser navigation, and file preparation. You approve anything externally visible or judgment-heavy: messages, emails, uploads, applications, sensitive screening answers, and final submissions.

This is intentionally prompt-first: a local plugin with Markdown instructions and private workspace files, no helper programs or hidden services. Speak naturally; the `jobs ...` commands are optional shortcuts.

Looking for the Claude Code version? See [claude-linkedin-assistant](https://github.com/FarzamHejaziK/claude-linkedin-assistant).

## Prerequisites

- **Codex or ChatGPT Work on desktop** with access to the selected local workspace. Open this repository in Codex or install the plugin through a supported local source. Browser steps require available browser tools; local setup, tracker checks, and saved-source preparation also work without them.
- **LinkedIn sign-in in the selected browser** before LinkedIn discovery or outreach. The built-in browser has a separate profile from your regular Chrome session, so you may need to sign in there.
- **Git** for the repository installation route. A packaged plugin does not require a repository checkout for daily use. Daily work is not automatically committed.

**Chrome is optional.** Use connected Chrome when you explicitly want an existing tab/profile, the built-in browser is unavailable, or a required capability needs it. The default experience does not require installing a Chrome extension.

The built-in browser can be invoked with `@Browser`; `@Chrome` selects connected Chrome. Automated uploads in the built-in browser are currently documented as unsupported, so Codex prepares the resume and hands the attachment step to you, then verifies it before continuing. See [OpenAI's browser documentation](https://learn.chatgpt.com/docs/browser?surface=app) and [browser requirements](REQUIREMENTS.md).

Indeed remains optional and disabled by default; enable it later with `jobs indeed-setup`.

## Quick Start

### 1. Clone and open in Codex

```bash
git clone https://github.com/FarzamHejaziK/codex_linkedin_assistant.git
cd codex_linkedin_assistant
```

Open the folder in Codex and say **“Help me get started”** or `jobs setup`. The repository entrypoint still works.

To make the plugin available in other local Codex chats, register and install the local package from this repository directory:

```bash
codex plugin marketplace add .
codex plugin add job-search-assistant@codex-job-search-local
```

Start a new chat if the current chat has not refreshed its available plugins. The installable package is only `plugins/job-search-assistant/`; do not zip or copy your whole working repository into a plugin release.

### 2. Add your resume and search essentials

On first use, select an existing job-search workspace or create a local folder. The assistant remembers its location separately from the plugin. Your existing repository workspace can be reused without moving its files.

Attach a resume in chat or give its absolute local path. Codex copies it into `resumes/`, extracts known information, and asks only for missing target roles, location/remote preference, and dealbreakers. PDF, DOCX, LaTeX, Markdown, and HTML are supported sources.

Codex creates the local search/profile files for you. Compensation can be added now or later. Application-specific questions, editable resume formats, upload permissions, and optional Indeed setup are handled when needed.

### 3. Get your first matches

Say **“Find three roles that fit my resume.”** Codex uses the built-in browser by default and checks the signed-in LinkedIn identity before searching. Each match includes fit evidence, a gap, unknown details, and a next step.

If browser access is unavailable, local setup and tracker checks still work. Browser readiness is reported separately.

## How it works

- `job_tracker.csv` is the private, local source of truth for job state.
- The plugin bundles a header-only tracker template; setup creates a working tracker only for first-use initialization or explicit recovery. The root `job_tracker.example.csv` remains a public example.
- `resumes/` stores resume sources and search preferences.
- `profile/` stores reusable application details, screening answers, and `session.md` progress.
- `applications/` and `outreach/` store materials and contact evidence.
- `AGENTS.md` routes to the repository skill, which forwards to the same canonical skill shipped by the plugin.

There is no database or app build. The dashboard is rendered in chat from local files. Saved progress records completed steps and pending work; the tracker remains authoritative for job state.

Before browser-dependent steps, Codex verifies the selected browser and the relevant account. For LinkedIn work, the active profile must match the resume. Codex browser-bound Computer Use is supported; a Chrome extension is not required for the built-in browser. Local checks and preparation from saved sources need no browser. LinkedIn remains the primary discovery source; company boards and web search supplement it, with Indeed optional. If LinkedIn is blocked, switching discovery sources requires your approval.

## Local persistence across chats

The assistant records the selected workspace in `<user-home>/.config/codex-job-search-assistant/workspace.json`. That private pointer and the matching `profile/workspace.json` marker let new local chats find the same tracker, preferences, files, and pending progress. These are this assistant's own local files; no account, cloud database, or server is added.

Instructions/templates come from the plugin; all candidate data comes from the saved workspace. An unrelated chat directory does not become a new job-search workspace. A missing directory, invalid pointer, mismatched identity, or missing registered tracker produces a recovery message instead of a silent reset.

Plugin updates and removal leave the external workspace alone. To relocate it, move the workspace locally and ask the assistant to relink that folder. To change the default, say so explicitly. The same files must be accessible to the new session; this is local persistence, not cross-device sync. Keep one modifying workflow active per workspace.

See the [persistence contract](plugins/job-search-assistant/skills/jobs/references/local-persistence.md) for initialization and recovery rules.

## Plugin distribution

The installable package is `plugins/job-search-assistant/`. Version 0.1.3 includes expanded workflow descriptions, practical starter prompts, icons, and a public privacy policy link. The local marketplace and GitHub source are separate from OpenAI's public plugin directory; a release ZIP must pass OpenAI's checks and review before publication. This repository does not claim directory approval or availability.

See the [publication guide](.docs/plugin-publication.md) for packaging, validation, and submission status. The package's [privacy policy](plugins/job-search-assistant/PRIVACY.md) explains local storage and processing by OpenAI and selected services. Cloud-only chats need authorized access to the same computer to use its saved workspace; installation does not synchronize private files.

## What it does

| Say this | Shortcut | Result |
|---|---|---|
| Help me get started | `jobs setup` | Resume intake and essential search preferences |
| What needs my attention? | `jobs check` | Up to three recommended actions with reasons and blockers |
| Find five remote roles / Track this job link | `jobs find` | Explained matches or manual-link intake |
| Prepare three applications | `jobs apply` | Local materials only; no upload or submission |
| Help me apply to this role | `jobs apply` | Materials and form preparation, followed by explicit review |
| Help with referrals | `jobs referral` | Reply handling, drafts, follow-ups, and deadlines |
| I have 15 minutes for my search | `jobs daily` | A scoped run with saved progress |
| Enable Indeed | `jobs indeed-setup` | Optional additional discovery and separate profile optimization |
| Continue | Existing workflow | Resume after reconciling saved progress and confirmed outcomes |

There is no standalone `jobs add` or `jobs update`. Ask for the full dashboard when you want all queues. Tell Codex why a match is wrong to refine preferences; a single-job skip does not silently become a broad exclusion.

## Daily flow

The default daily run covers up to three distinct jobs, prioritizing urgent existing work. You can set a different count, a time allowance, or prepare-only scope.

1. Read local state and recommend actions.
2. Prepare ready applications and referral work within the selected scope.
3. Search only if requested or the batch has room.
4. Review concrete external actions before execution.
5. Save progress and summarize confirmed outcomes, materials, and blockers.

When you need to sign in or attach a resume manually, Codex keeps the relevant tab available, saves the pending step, and verifies page/account state when you return. Switching browsers does not transfer logins or unfinished forms.

Time limits are best effort, checked between operations. A browser failure pauses browser-dependent steps while local work can continue. “Continue” resumes unfinished steps after checking what already happened. Uncertain sends or submissions are verified before retrying.

Daily work does not automatically create Git commits.

## Review before external actions

Codex shows the action, destination, exact content or application answers, and attachment versions. You can approve, edit, or skip. Uploads can expose personal data, so they also require approval; approving an upload alone does not authorize submission. Material changes require a new review.

Prepare-only requests never upload, send, submit, request connections, or save public profile edits. Confirmed outcomes are distinguished from prepared materials and uncertain attempts.

## Resume Backends

| Backend | Use when |
|---|---|
| LaTeX | You already maintain editable `.tex` sources |
| DOCX | You use Word-style resume sources |
| Markdown/HTML | You want simple editable sources |
| PDF-only | You have a final attachment; true tailoring needs editable source |

Choose a backend when tailoring is needed. Codex must never invent experience, credentials, dates, or metrics.

## Tracker Format

The schema remains unchanged:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

## Privacy

The working tracker, real resumes, profile/session files, application folders, outreach logs, and base resume sources are ignored by default. Only the empty tracker template and generic examples belong in the public repository. Never force-add private files.

Local persistence does not mean offline processing. Content read by the assistant can enter the OpenAI conversation, and approved browser/tool actions can share information with selected services. See the [data-handling notice](plugins/job-search-assistant/PRIVACY.md).

**Existing checkout migration:** if your tracker is still tracked, preserve a private backup before updating the repository. Ask Codex to retain the local file and remove only its Git index entry; adding an ignore rule alone does not protect tracked files. See [workspace migration](.agents/skills/jobs/references/workspace-files.md). Restoring the working file after an update does not change its schema. Previously committed data remains in Git history; this change does not rewrite history.

## Contributing

Pull requests only; see [CONTRIBUTING.md](CONTRIBUTING.md), [MAINTAINERS.md](MAINTAINERS.md), and [SECURITY.md](SECURITY.md). Keep changes prompt-first and private user data local.

## Design Docs

- [Product requirements](.docs/PRD.md)
- [Implementation plan](.docs/plan.md)
- [UX improvement plan and acceptance scenarios](.docs/ux-improvement-plan.md)
- [Local plugin implementation plan](.docs/local-plugin-plan.md)
- [Plugin publication guide](.docs/plugin-publication.md)

## Related

- [Claude Code version](https://github.com/FarzamHejaziK/claude-linkedin-assistant)

## License

MIT. See [LICENSE](LICENSE).
