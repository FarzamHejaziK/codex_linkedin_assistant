# Browser Preflight

Run this at the start of every operational `jobs ...` workflow.

The goal is to make sure Codex can connect through the Codex Chrome extension and that the active LinkedIn account is the right person before any job-search work happens.

If no real resume exists yet, route to `setup.md` resume intake first. The LinkedIn profile-match check requires a resume name.

## Required Tool Path

LinkedIn preflight must use the Codex Chrome extension/tool path. Generic browser control, macOS Computer Use, screenshots, or manual observation do not count as a passing preflight.

Use Codex-facing terminology. Do not mention `mcp__chrome`, MCP namespaces, or Claude-style tool namespaces in user-facing messages. Do not use the absence of an `mcp__chrome...` namespace as evidence in Codex. In Codex, talk about the Chrome skill/tool, the Codex Chrome extension, and whether the active session exposes Chrome.

There are two separate requirements:

1. The Codex Chrome Extension is installed/enabled in Chrome.
2. The active Codex session exposes the Chrome skill/tool so the agent can communicate with that extension.

The extension can be installed on the computer while the active Codex session still does not expose Chrome tools. In that case, the agent cannot complete LinkedIn preflight until the session is reconnected/reopened with Chrome available.

## Explain The State Clearly

When preflight fails, distinguish these states in plain language:

- Extension not installed/enabled in Chrome.
- Extension installed, but Chrome is closed or on the wrong Chrome profile.
- Extension installed, but native host/setup communication is broken.
- Chrome skill/tool exists in Codex, but the connection check failed.
- Chrome skill/tool is not exposed to this Codex session at all.

Do not tell the user "you do not have Chrome installed" or "you do not have the extension" unless the Chrome checks prove that. If the user says it is installed, acknowledge that and explain that the missing part may be session exposure or extension communication, not installation.

## Connection Attempt Order

Before declaring Chrome unavailable:

1. If a Chrome skill or plugin is available in the current session, use it.
2. Try a lightweight Chrome connection check first, such as listing open tabs or opening/claiming a harmless tab.
3. If the first connection check fails, wait briefly and retry once.
4. If the Chrome skill reports extension/native-host/setup problems, follow the Chrome skill's repair guidance.
5. Only after those attempts fail should the workflow say Chrome-specific tools are unavailable or not communicating.

If Chrome-specific app tools are not exposed in the active Codex session:

1. If tool discovery is available, search for the Chrome/Codex Chrome tool or skill before declaring it unavailable.
2. If a Chrome skill is discovered, use that skill's connection checks before stopping.
3. If Chrome still is not exposed or cannot communicate after the retry path, stop the workflow.
4. Tell the user that the Codex Chrome extension/tool is not available to this session even if it may be installed on the computer.
5. Walk the user through connecting it:

```text
Codex -> Plugins -> Chrome
Follow the setup flow.
Open Chrome and confirm the Codex extension says Connected.
Then restart this Codex session or reopen the repo and run the command again.
```

Do not say "I will use Computer Use instead" for LinkedIn preflight. Computer Use may help explain where a setting is, but it is not the required Chrome connection check for this assistant.

## User-Facing Failure Template

If Chrome is installed but the workflow cannot get a working Chrome connection, say:

```text
I believe Chrome/the extension may be installed. The blocker is narrower: this Codex session does not currently have a working Chrome connection for me to use.

I need the Codex Chrome skill/tool to pass preflight, because I have to verify the signed-in LinkedIn profile against the resume before running job workflows.

Please check:
1. Chrome is open in the profile where the Codex extension is installed.
2. The Codex extension is enabled and says Connected.
3. In Codex, go to Plugins -> Chrome and reconnect if needed.
4. Reopen this repo/session and rerun the command.

I will not use Computer Use as a workaround for LinkedIn preflight.
```

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
