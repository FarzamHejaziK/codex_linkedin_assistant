# Browser Preflight

Run this at the start of every operational `jobs ...` workflow.

The goal is to make sure Codex can connect to Chrome and that the active LinkedIn account is the right person before any job-search work happens.

If no real resume exists yet, route to `setup.md` resume intake first. The LinkedIn profile-match check requires a resume name.

## Startup Checks

1. Confirm a Chrome/browser automation tool is available and connected in the active Codex session.
2. Check that `resumes/` contains a real resume file, excluding README/example files.
3. Read the user's resume and extract the likely full name.
4. Open LinkedIn in Chrome and verify the user is signed in.
5. Compare the active LinkedIn profile name with the resume name.
6. If names differ, stop and ask the user to correct the active LinkedIn account or confirm the mismatch before continuing.

## Upload Checks

Run these before application/referral uploads:

1. Confirm the Codex Chrome extension has file URL access enabled.
2. If possible, perform a harmless local file upload/access smoke test before the first real upload.

## Failure Messages

- Browser unavailable: explain that browser automation is required for LinkedIn/application workflows.
- LinkedIn logged out: ask the user to sign in and rerun.
- Wrong profile: show expected name and observed name.
- Missing resume: route to setup resume intake before trying to verify LinkedIn identity.
- File URL access disabled: give the Chrome extension details path and stop before upload.

## Upload Policy

This assistant may upload local resume and cover-letter files through the Codex Chrome extension after preflight passes. Still require explicit user approval before sending messages, sending emails, or submitting applications.
