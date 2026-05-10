# Browser Preflight

Run this at the start of every operational `jobs ...` workflow.

The goal is to make sure Codex can connect through the Codex Chrome extension and that the active LinkedIn account is the right person before any job-search work happens.

If no real resume exists yet, route to `setup.md` resume intake first. The LinkedIn profile-match check requires a resume name.

## Required Tool Path

LinkedIn preflight must use the Codex Chrome extension/tool path. Generic browser control, macOS Computer Use, screenshots, or manual observation do not count as a passing preflight.

If Chrome-specific app tools are not exposed in the active Codex session:

1. If tool discovery is available, search for the Chrome/Codex Chrome tool or skill before declaring it unavailable.
2. If Chrome still is not exposed, stop the workflow.
3. Tell the user that the Codex Chrome extension/tool is not available to this session even if it may be installed on the computer.
4. Walk the user through connecting it:

```text
Codex -> Plugins -> Chrome
Follow the setup flow.
Open Chrome and confirm the Codex extension says Connected.
Then restart this Codex session or reopen the repo and run the command again.
```

Do not say "I will use Computer Use instead" for LinkedIn preflight. Computer Use may help explain where a setting is, but it is not the required Chrome connection check for this assistant.

## Startup Checks

1. Confirm the Codex Chrome extension/tool is exposed and connected in the active Codex session.
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

- Chrome tool unavailable: explain that the Codex Chrome extension/tool is required and that Computer Use is not an acceptable substitute for LinkedIn preflight.
- LinkedIn logged out: ask the user to sign in and rerun.
- Wrong profile: show expected name and observed name.
- Missing resume: route to setup resume intake before trying to verify LinkedIn identity.
- File URL access disabled: give the Chrome extension details path and stop before upload.

## Upload Policy

This assistant may upload local resume and cover-letter files through the Codex Chrome extension after preflight passes. Still require explicit user approval before sending messages, sending emails, or submitting applications.
