# Codex LinkedIn Job Assistant

<p align="center">
  <strong>A repo-first Codex assistant for running a complete local job-search workflow.</strong>
</p>

<p align="center">
  <img alt="Codex Ready" src="https://img.shields.io/badge/Codex-Ready-111827">
  <img alt="Prompt First" src="https://img.shields.io/badge/Prompt--First-Markdown-blue">
  <img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-via%20Codex%20Chrome-0A66C2?logo=linkedin&logoColor=white">
  <img alt="Privacy" src="https://img.shields.io/badge/Private%20Data-Local%20Only-success">
</p>

<p align="center">
  <a href="#prerequisites"><strong>Prerequisites</strong></a> ·
  <a href="#quick-start"><strong>Quick Start</strong></a> ·
  <a href="#how-it-works"><strong>How It Works</strong></a> ·
  <a href="#workflows"><strong>Workflows</strong></a> ·
  <a href="#daily-flow"><strong>Daily Flow</strong></a> ·
  <a href="#privacy"><strong>Privacy</strong></a>
</p>

This is a normal repository you open directly in Codex. It tracks jobs in a CSV, reads your resume and search preferences, orchestrates referrals, prepares application materials, remembers reusable screening answers, and guides applications through browser workflows.

It is intentionally prompt-first: Markdown instructions and local files, no helper programs or hidden services.

## Prerequisites

You need these before the job-search flows can work end to end:

- **[Codex app](https://developers.openai.com/codex/app)** - open this repository in the Codex desktop app so Codex can read and write the local workspace files. The browser-only experience is not enough for this repo because the assistant needs local resumes, tracker files, application folders, and git commits.
- **[Codex Chrome extension](https://developers.openai.com/codex/app/chrome-extension)** - this lets Codex use your signed-in Chrome session for LinkedIn, Gmail, company career pages, and ATS forms. Set it up from Codex with `Codex -> Plugins -> Chrome`, follow the setup flow, then confirm the Chrome toolbar extension shows `Connected`.
- **Chrome signed in to LinkedIn** - use the same Chrome profile where the Codex extension is connected.
- **Git** - used to clone the repo and create end-of-day commits.

For resume uploads and referral/application attachments, also enable file access:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Allow access to file URLs
```

## Quick Start

### 1. Clone and open in Codex

```bash
git clone https://github.com/FarzamHejaziK/codex_linkedin_assistant.git
cd codex_linkedin_assistant
```

Open the folder in Codex. Codex reads:

```text
AGENTS.md
```

That file points to the project skill:

```text
.agents/skills/jobs/SKILL.md
```

### 2. Add your resume

Put one or more resume files in:

```text
resumes/
```

Supported formats:

- `.pdf`
- `.tex`
- `.docx`
- `.md`
- `.html`

### 3. Add search preferences

Optional:

```text
resumes/search_profile.md
```

Use it for must-haves, deal-breakers, locations, salary floor, level, industries, company stage, and role preferences.

### 4. Confirm browser file access

For application uploads and referral materials:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Allow access to file URLs
```

### 5. Run setup

```text
jobs setup
```

## How It Works

The assistant is built around local files:

- `job_tracker.csv` is the source of truth.
- `resumes/` describes who you are and what jobs fit.
- `profile/` stores reusable application fields and screening answers.
- `base_resumes/` can hold editable resume sources.
- `applications/` stores per-job materials.
- `outreach/` stores referral contact logs.
- `.agents/skills/jobs/references/` contains workflow instructions.

There is no database. Open the CSV and folders to inspect the current state.

## Repository Structure

```text
Codex_Linkedlin_assistant/
├── AGENTS.md
├── README.md
├── REQUIREMENTS.md
├── job_tracker.csv
├── .gitignore
├── .docs/
│   ├── PRD.md
│   └── plan.md
├── .agents/
│   └── skills/
│       └── jobs/
│           ├── SKILL.md
│           └── references/
│               ├── apply.md
│               ├── browser-preflight.md
│               ├── check.md
│               ├── daily.md
│               ├── find.md
│               ├── overview.md
│               ├── referral.md
│               ├── resume-backends.md
│               ├── setup.md
│               ├── tracker-schema.md
│               ├── workspace-files.md
│               └── writing-style.md
├── resumes/
│   ├── README.md
│   └── search_profile.example.md
├── profile/
│   ├── personal_info.example.json
│   └── screening_answers.example.md
├── base_resumes/
│   └── README.md
├── applications/
│   └── README.md
└── outreach/
    └── README.md
```

## Workflows

| Intent | What it does |
|---|---|
| `jobs setup` | Verifies workspace files, tracker schema, resume backend, and browser preflight |
| `jobs check` | Shows dashboard, deadlines, ready-to-apply jobs, and outreach queues |
| `jobs find` | Discovers jobs or intakes a pasted job link, then adds qualified rows |
| `jobs referral` | Handles replies, referral materials, email-vs-LinkedIn decisions, follow-ups, outreach, and deadlines |
| `jobs apply` | Prepares materials, tailors resume when possible, fills forms, uploads files, and submits after approval |
| `jobs daily` | Runs the daily sequence and commits at the end when appropriate |

There is no standalone `jobs add` or `jobs update`.

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

## Resume Backends

| Backend | Use when | Notes |
|---|---|---|
| LaTeX | You maintain `.tex` resume sources | Codex can edit source and use local LaTeX tools if available |
| DOCX | You maintain Word-style resumes | Codex can use available document tools or prepare edits for manual export |
| Markdown/HTML | You want simple editable sources | Codex edits source and uses available export tools |
| PDF-only | You only have a final PDF | Works as an attachment, but true tailoring needs editable source |

Codex must never invent experience. Tailoring means truthful emphasis and wording, not fabrication.

## Tracker Format

Do not change the header:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

## Privacy

Private files are ignored by default:

- Real resumes
- Real profile data
- Screening answers
- Application folders
- Outreach contact logs
- Base resume sources

Do not commit private job-search data unless you intentionally change `.gitignore`.

## Design Docs

- `.docs/PRD.md`
- `.docs/plan.md`
