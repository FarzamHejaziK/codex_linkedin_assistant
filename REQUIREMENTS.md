# Requirements & Setup

This repo is designed to be opened directly in the Codex app with Chrome connected for LinkedIn and application workflows.

## Required

### Codex App

Install and use the [Codex app](https://developers.openai.com/codex/app), then open this folder in Codex. Codex reads `AGENTS.md`, which points to the project-local skill at:

```text
.agents/skills/jobs/SKILL.md
```

### Codex Chrome Extension

Install and connect the [Codex Chrome extension](https://developers.openai.com/codex/app/chrome-extension). The extension lets Codex use your signed-in Chrome profile for LinkedIn, Gmail, company career pages, and ATS forms.

Setup path:

```text
Codex -> Plugins -> Chrome
```

Follow the setup flow, then open Chrome and confirm the Codex extension shows `Connected`.

The assistant requires this Chrome extension/tool path for LinkedIn preflight. If a Codex session says Chrome-specific tools are not exposed, reconnect Chrome from `Codex -> Plugins -> Chrome`, restart the session or reopen the repo, and rerun the command. Computer Use is not a substitute for the LinkedIn preflight check.

### Chrome + LinkedIn Login

Use the same Chrome profile where the Codex extension is connected, then sign in to LinkedIn. The assistant will compare the LinkedIn profile name with the resume name before browser-dependent workflows.

For file uploads, enable local file URL access for the Codex Chrome extension:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Allow access to file URLs
```

If this is not enabled, Codex should stop before upload steps.

### Git

Git is used for end-of-day commits.

```bash
git --version
```

## Recommended

### CSV Editor

Use any of:

- Numbers
- Excel
- LibreOffice
- VS Code with a CSV editor extension

### Resume Tooling

Choose the backend that matches your resume source:

- LaTeX: `pdflatex` or `latexmk`
- DOCX: Word, LibreOffice, or available document tooling
- Markdown/HTML: browser or local PDF export tooling
- PDF-only: works as an attachment, but editable source is required for true tailoring

## First Run

1. Start with:

```text
jobs setup
```

2. If setup does not find a real resume, upload it in Codex chat, paste an absolute local file path, or drag the file into `resumes/`.
3. Answer the onboarding questions. Codex should create `resumes/search_profile.md`, `profile/personal_info.json`, and `profile/screening_answers.md` for you instead of telling you to create them manually.

The resume is required before LinkedIn profile verification, search matching, referral messaging, or application workflows.
