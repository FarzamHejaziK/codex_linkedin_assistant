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

1. Add resume files to `resumes/`.
2. Optionally write `resumes/search_profile.md`.
3. Fill local profile data based on `profile/personal_info.example.json`.
4. Start with:

```text
jobs setup
```
