# Contributing to Codex LinkedIn Assistant

Thanks for contributing.

## Quick Links

- Workflow overview: [`README.md`](README.md)
- Setup: [`REQUIREMENTS.md`](REQUIREMENTS.md)
- Project rules: [`AGENTS.md`](AGENTS.md)
- Design docs: [`.docs/`](.docs/)
- Job skill instructions: [`.agents/skills/jobs/`](.agents/skills/jobs/)
- Maintainers: [`MAINTAINERS.md`](MAINTAINERS.md)

## How to Contribute

**Pull requests only.** Direct pushes to `main` are blocked for the public project.

1. **Bug fixes / small improvements**: fork, branch, open a focused PR.
2. **Bigger changes** such as new workflows, structural changes, or anything touching multiple `.agents/skills/jobs/references/*.md` files: open an Issue first to align scope.
3. **Questions**: open an Issue.

## Before You Open a PR

- Keep the PR scoped to one concern.
- This is a prompt-first Markdown workspace. There is no app build step and no helper-code test suite.
- If you change workflow behavior, check that `AGENTS.md`, `README.md`, `.docs/PRD.md`, `.docs/plan.md`, `.agents/skills/jobs/SKILL.md`, and the affected reference docs all agree.
- If you add or rename a `jobs ...` workflow, update `.agents/skills/jobs/SKILL.md`, the relevant reference file, and `README.md`.
- Do not add helper programs, automation scripts, app code, or custom renderers for v1.
- Do not commit real resumes, profile data, application folders, outreach logs, or private candidate data.

## PR Description Checklist

Use the template and explain what changed, why, and how you verified it.

For docs-only changes, acceptable validation is:

- read the changed instructions end to end;
- searched for stale terms or contradictions;
- confirmed no private data was added.

## Review Policy

- **CODEOWNERS review is required before merge.** All paths default to [@FarzamHejaziK](https://github.com/FarzamHejaziK).
- **Final merge authority remains with the project owner**. Even with future co-maintainers, the owner has the last word on product direction.

## What to Not Contribute

- Code or instructions that harvest private user data, session data, cookies, passwords, or unrelated browser data.
- LinkedIn scraping at scale or behavior intended to bypass platform blocks, rate limits, or verification prompts.
- Silent sending, emailing, uploading, or submitting without explicit approval gates.
- Fabricated resume experience, credentials, metrics, dates, companies, or personal details.
- Generic browser or Computer Use fallbacks that bypass the required Codex Chrome preflight for LinkedIn workflows.
- Helper scripts or app code for v1.

## Reporting Security Issues

See [`SECURITY.md`](SECURITY.md). Do not open a public Issue for a security report.
