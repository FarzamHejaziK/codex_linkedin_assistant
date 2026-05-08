# Codex LinkedIn Job Assistant Workspace

This folder is a local job-search workspace managed by Codex. It tracks jobs in a CSV, reads your resume and search preferences, helps with LinkedIn referrals, prepares application materials, and keeps generated artifacts organized.

## First Setup

1. Put resume files in `resumes/`.
2. Optionally create `resumes/search_profile.md` with must-haves, interests, deal-breakers, locations, and salary expectations.
3. Fill in `profile/personal_info.json`.
4. Keep `profile/screening_answers.md` updated as applications reveal reusable answers.
5. In Chrome, open Extensions, manage the Codex extension, and enable `Allow access to file URLs`.

## Workflows

- `jobs setup`: initialize and verify this workspace.
- `jobs check`: show the dashboard.
- `jobs find`: discover jobs or intake a pasted job link.
- `jobs referral`: handle replies, referrals, follow-ups, outreach, and deadlines.
- `jobs apply`: tailor materials and submit applications after approval.
- `jobs daily`: run the daily sequence.

## Resume Backends

You can use LaTeX, DOCX, Markdown/HTML, or PDF-only. Editable sources allow true tailoring. PDF-only works as an attachment but cannot be truthfully described as tailored unless editable source is available.

## Privacy

Private files are ignored by default: resumes, profile details, application folders, and outreach logs. Do not commit private data unless you intentionally change `.gitignore`.
