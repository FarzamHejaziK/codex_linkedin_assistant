# Setup Workflow

Use this when the user asks for `jobs setup`, opens a new workspace, or a required file is missing.

## Steps

1. Inspect the current folder.
2. Create missing workspace files from templates without overwriting user files.
3. Ensure `.gitignore` protects private job-search data.
4. Ask the user to place resume files in `resumes/`, or accept a path/attachment and move/copy it there if asked.
5. Help the user choose a resume backend: LaTeX, DOCX, Markdown/HTML, or PDF-only.
6. Create `profile/personal_info.json` from the example if missing.
7. Create `profile/screening_answers.md` from the example if missing.
8. Verify `job_tracker.csv` has the exact schema.
9. Run browser preflight before browser-dependent workflows.

## Chrome Extension Setup

For uploads, the user must enable file URL access:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Enable: Allow access to file URLs
```

If this is not enabled, stop before application/referral file upload and walk the user through the setting.

## Safe Creation

Never overwrite:

- Existing resume files
- Existing profile files
- Existing tracker rows
- Existing outreach logs
- Existing application folders

If a file exists but appears malformed, report the issue and ask whether to repair it.
