# Job Search Assistant: Data Handling

Version 0.1.1, 2026-10-09. Publisher: Farzam Hejazi.

This skills-only package provides instructions, generic templates, and presentation assets. It includes no publisher-operated server, telemetry, analytics, account system, or background service.

## Local workspace

The assistant stores the working job tracker, resumes, profile answers, application materials, outreach logs, and progress in a local folder the user selects. A separate local configuration file records the folder location and its workspace identity. Working candidate files are excluded from Git by default. Git ignores do not erase previously committed files or history.

Plugin updates and removal do not delete that external folder or its configuration pointer. Users control local retention, backups, relocation, and deletion. The package does not provide cloud backup or device synchronization.

## Model and service processing

Local storage does not mean offline processing. File content the assistant reads may become part of the ChatGPT or Codex conversation and is handled under the user's OpenAI plan, settings, and applicable terms. Browser and connected-tool interactions may send data to the websites or services involved. Their own policies and permissions apply.

The workflow requests a concrete, scoped review before uploading files, sending messages, submitting applications, or saving public profile changes. Approved actions disclose the reviewed information to the selected recipient or service. Do not store passwords, API keys, or browser cookies in workspace files.

## Support

Use the source repository's Issues page for public bug reports. Do not post resumes, profile answers, credentials, or private job-search records. Follow the repository's SECURITY.md for security reports.

Job Search Assistant is an independent project. It is not affiliated with or endorsed by OpenAI, LinkedIn, Indeed, or an employer.
