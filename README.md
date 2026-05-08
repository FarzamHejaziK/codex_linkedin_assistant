# Codex LinkedIn Job Assistant

<p align="center">
  <strong>A prompt-first Codex plugin for running a complete local job-search workflow.</strong>
</p>

<p align="center">
  <img alt="Codex Plugin" src="https://img.shields.io/badge/Codex-Plugin-111827">
  <img alt="Prompt First" src="https://img.shields.io/badge/Prompt--First-Markdown-blue">
  <img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-via%20Codex%20Chrome-0A66C2?logo=linkedin&logoColor=white">
  <img alt="Privacy" src="https://img.shields.io/badge/Private%20Data-Local%20Only-success">
</p>

<p align="center">
  <a href="#quick-start"><strong>Quick Start</strong></a> ·
  <a href="#how-it-works"><strong>How It Works</strong></a> ·
  <a href="#what-it-does"><strong>What It Does</strong></a> ·
  <a href="#daily-flow"><strong>Daily Flow</strong></a> ·
  <a href="#privacy"><strong>Privacy</strong></a> ·
  <a href="#design-docs"><strong>Design Docs</strong></a>
</p>

This repo contains a repo-local Codex plugin that helps manage the full job-search loop: tracking jobs, discovering openings, orchestrating referrals, preparing resume materials, handling application memory, and guiding application submission with explicit approval gates.

It is intentionally **prompt-first**. The plugin is made of Markdown skills, workflow references, and workspace templates. There are no helper programs, scripts, ATS adapters, or hidden services. Codex reads the prompts and uses the tools available in the active session.

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/FarzamHejaziK/codex_linkedin_assistant.git
cd codex_linkedin_assistant
```

### 2. Open it in Codex

Open this folder in the Codex desktop app. The repo includes a local plugin marketplace entry at:

```text
.agents/plugins/marketplace.json
```

The plugin itself lives at:

```text
plugins/codex-linkedin-job-assistant/
```

### 3. Install or enable the plugin

Use the Codex plugin UI to install the local plugin named:

```text
Codex LinkedIn Job Assistant
```

The plugin manifest is:

```text
plugins/codex-linkedin-job-assistant/.codex-plugin/plugin.json
```

### 4. Create or open a job-search workspace

In a workspace where you want to track applications, run:

```text
jobs setup
```

Codex will create or verify:

```text
job_tracker.csv
resumes/
profile/
base_resumes/
applications/
outreach/
```

### 5. Set up browser access

For LinkedIn and application forms, sign into LinkedIn in Chrome and make sure the Codex Chrome extension is connected.

For resume and cover-letter uploads, enable local file access:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Enable: Allow access to file URLs
```

Without file URL access, Codex should stop before upload steps and walk you through the setting.

## How It Works

The assistant is built around local files:

- `job_tracker.csv` is the source of truth for job state.
- `resumes/` is the source of truth about who you are and what roles fit.
- `resumes/search_profile.md` optionally describes what you want, such as locations, salary, industries, deal-breakers, and role level.
- `profile/personal_info.json` stores reusable application fields.
- `profile/screening_answers.md` stores reusable answers to custom application questions.
- `applications/` stores per-job materials.
- `outreach/` stores per-company referral contact logs.

Codex reads and updates these files directly. There is no database and no hidden state. If you want to understand the current state of the search, open the CSV and the local folders.

## What It Does

| Feature | Intent | What Codex does |
|---|---|---|
| Setup | `jobs setup` | Creates/verifies workspace files, privacy defaults, resume backend choice, and browser preflight |
| Dashboard | `jobs check` | Shows ready-to-apply jobs, referral deadlines, stale outreach, pending connections, and next actions |
| Discovery | `jobs find` | Reads your resume/search profile, searches LinkedIn/web, dedupes, scores, and adds jobs to the tracker |
| Referral orchestration | `jobs referral` | Handles warm replies, referral materials, email-vs-LinkedIn decisions, follow-ups, outreach, and deadline enforcement |
| Applications | `jobs apply` | Prepares materials, tailors resumes when editable source exists, fills forms, uploads files, and submits after approval |
| Daily run | `jobs daily` | Runs the core sequence end to end and commits at the end when appropriate |

There is deliberately no standalone `jobs add` or `jobs update` workflow. Manual job links are handled through `jobs find`, and status changes happen through workflow outcomes or direct CSV edits.

## What It Does Not Do Automatically

- It does not invent resume experience, credentials, metrics, companies, dates, or personal details.
- It does not send messages, send emails, or submit applications without explicit approval.
- It does not force-add ignored private data to git.
- It does not ship custom scraping code, ATS automation code, or resume-rendering code.

## Daily Flow

`jobs daily` runs the intended daily sequence:

```text
1. preflight
2. jobs check
3. jobs apply        # ready jobs only
4. jobs referral     # replies, follow-ups, outreach, deadlines
5. jobs find         # discover or intake jobs
6. instant apply     # newly found no-referral jobs, with approval
7. commit
8. summary
```

The referral step is an orchestrator, not a menu. It inspects the tracker and outreach logs, then handles the most urgent referral work first.

## Resume Backends

The plugin supports several resume workflows through prompts:

| Backend | Use when | Notes |
|---|---|---|
| LaTeX | You maintain `.tex` resume sources | Codex can edit source and use local LaTeX tools if available |
| DOCX | You maintain Word-style resumes | Codex uses available document tools or prepares edits for manual export |
| Markdown/HTML | You want simple editable text sources | Codex edits source and uses available export tools |
| PDF-only | You only have a final PDF | Codex can attach it, but true tailoring needs editable source |

Resume tailoring must stay truthful. Codex can rewrite for relevance, clarity, and keyword alignment, but it must not fabricate experience.

## Folder Layout

```text
codex_linkedin_assistant/
├── README.md
├── .docs/
│   ├── PRD.md
│   └── plan.md
├── .agents/
│   └── plugins/
│       └── marketplace.json
└── plugins/
    └── codex-linkedin-job-assistant/
        ├── .codex-plugin/
        │   └── plugin.json
        ├── skills/
        │   └── job-search/
        │       ├── SKILL.md
        │       └── references/
        └── templates/
```

The files under `templates/` are copied or recreated by Codex when initializing a user workspace.

## Tracker Format

The tracker schema is fixed:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

Canonical statuses:

```text
To Apply -> Applied -> Recruiter Call -> Phone Screen -> Onsite -> Offer
                                                -> Rejected / Withdrew
```

Referral status values:

```text
Not Needed / Outreach Pending / Connection Pending / Outreach Sent / Got Referral / Declined / No Referral
```

## Privacy

The generated workspace template ignores private job-search state by default:

- Resume files
- Profile details
- Screening answers
- Application folders
- Outreach contact logs
- Base resume sources

Do not commit private data unless you intentionally edit `.gitignore`.

## Design Docs

The canonical design references are:

- [.docs/PRD.md](.docs/PRD.md)
- [.docs/plan.md](.docs/plan.md)

Operational prompt references live under:

```text
plugins/codex-linkedin-job-assistant/skills/job-search/references/
```
