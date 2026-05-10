# Browser Preflight

Run this before workflows that need LinkedIn, email, application forms, or file uploads.

## Checks

1. Confirm a browser automation tool is available in the active Codex session.
2. Open LinkedIn and verify the user is signed in.
3. Read the user's resume and extract the likely full name.
4. Compare the LinkedIn profile name with the resume name.
5. If names differ, stop and ask the user to confirm whether the active profile is correct.
6. For application/referral uploads, confirm the Codex Chrome extension has file URL access enabled.
7. If possible, perform a harmless local file upload/access smoke test before the first real upload.

## Failure Messages

- Browser unavailable: explain that browser automation is required for LinkedIn/application workflows.
- LinkedIn logged out: ask the user to sign in and rerun.
- Wrong profile: show expected name and observed name.
- File URL access disabled: give the Chrome extension details path and stop before upload.

## Upload Policy

This plugin may upload local resume and cover-letter files through the Codex Chrome extension after preflight passes. Still require explicit user approval before sending messages, sending emails, or submitting applications.
