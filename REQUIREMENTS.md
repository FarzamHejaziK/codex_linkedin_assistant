# Requirements & Setup

Open this workspace in the Codex desktop app. AGENTS.md points to the compatible `.agents/skills/jobs/SKILL.md` entrypoint; the installable canonical skill lives in `plugins/job-search-assistant/skills/jobs/`. The repository supplies Markdown instructions and local files; it does not install a browser service or automation scripts.

## Default: Codex Built-In Browser

Use the built-in browser exposed to the active Codex session, including through `@Browser`. Codex verifies that it can open and inspect a page before reporting readiness. For LinkedIn work, sign in to LinkedIn in that browser and verify the active profile against the resume. Other services need their own relevant account check.

The built-in browser uses a separate profile from your regular browser; Chrome sign-in does not establish built-in sign-in. Use the browser's normal sign-in UI and permissions. Do not paste passwords or session cookies into workspace files.

Local setup, tracker checks, and preparing materials from saved sources do not require a browser. If a browser is unavailable, Codex explains which step is blocked and continues useful local work.

## Attachments

OpenAI currently documents automated uploads in the built-in browser as unsupported. The normal flow is:

1. Codex prepares the exact resume/attachment and links it locally.
2. You attach it to the identified form in the retained browser tab.
3. Codex reads back the attachment/form state and presents any remaining review before submission.

This is an upload handoff, not an instruction to submit. Use automation only when current browser-specific documentation and the active tool support it. A generic file-chooser API alone does not establish support. See [OpenAI Browser documentation](https://learn.chatgpt.com/docs/browser?surface=app), checked 2026-10-08.

## Optional: Connected Chrome

Use Chrome when the user selects @Chrome or an existing Chrome tab/profile, or when a supported alternative is needed for a capability. Follow the active Chrome tool's connection guidance and [official browser-extension instructions](https://learn.chatgpt.com/docs/chrome-extension). Check connectivity before saying the extension is missing.

When using an extension upload path that requires local file access, enable the extension's **Allow access to file URLs** setting in Chrome's extension details. That setting applies to Chrome, not the built-in browser. Verify upload support, exact file/destination approval, and the resulting attachment.

After switching browsers, reverify the account, role, and form. Do not assume login or draft form state transfers. Do not switch browsers to evade site restrictions or denied access.

## Optional Indeed

Indeed is disabled by default. Ask to enable it later or use `jobs indeed-setup`. Sign in to the selected browser when needed. Public profile optimization is separately optional. An Indeed block affects only Indeed.

## Local Files and Resume Sources

Git is used to clone/update the repository; daily work is not automatically committed. First-use setup creates private, ignored `job_tracker.csv` from the bundled template. Missing state in a registered workspace requires explicit recovery. Follow README migration guidance before updating an older checkout with a tracked working file.

Start with “Help me get started,” attach a resume or give its absolute local path, and answer only missing target-role, location/remote, and dealbreaker questions. Codex creates local files. Choose an editable resume backend when tailoring is needed; PDF-only attachments are supported, with tailoring limitations. Available local document/export tools determine rendering options.

A CSV editor such as Numbers, Excel, or LibreOffice is optional. The normal dashboard appears in chat.

## Local Plugin and File Access

The local package uses skills and existing Codex filesystem/browser tools. No hooks, server, API key, or background service is needed. Install using the README commands, then invoke the plugin in a new local chat. The session needs access to the saved workspace and `<user-home>/.config/codex-job-search-assistant/workspace.json`; installation alone does not grant filesystem access outside a restricted workspace. If access is blocked, open the registered workspace in a local chat or grant only the needed access through Codex's normal controls.

The package can be updated independently of the workspace. This local-only design does not assume cloud chats can access your computer or synchronize files.
