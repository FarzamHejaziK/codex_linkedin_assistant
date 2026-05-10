# Security Policy

## Reporting a Vulnerability

If you find a security issue, **do not open a public GitHub Issue.** Instead, contact the maintainer directly:

- [@FarzamHejaziK](https://github.com/FarzamHejaziK)

Include:

- A clear description of the issue.
- Reproduction steps.
- The specific commit, file, or line if applicable.
- Your assessment of impact.

We aim to acknowledge reports within 5 business days and respond with a timeline once triaged.

## Scope

This repo is a Markdown-first Codex workspace. The security surface is small but real:

- **Instructions Codex follows** under `.agents/skills/jobs/`. A malicious PR could add instructions that exfiltrate resume/profile data, send messages without approval, or take unsafe browser actions.
- **Project rules** in `AGENTS.md`, `.docs/`, and `README.md`.
- **Privacy defaults** in `.gitignore`, which protect real resumes, profile files, applications, and outreach logs from accidental commits.
- **Browser workflow instructions**, especially anything involving LinkedIn, email, file uploads, or application submission.

If you spot something that could harm a downstream user of this repo, please report it.

## What's Out of Scope

- OpenAI Codex app or Codex Chrome extension vulnerabilities. Report those upstream to OpenAI.
- LinkedIn's own platform rules or Terms of Service. This repo requires user approval gates, uses the user's browser session, and stops on platform blocks, CAPTCHA, rate limits, or unusual verification prompts.
- Product-design disagreements that do not create a user-safety or data-exposure issue.
