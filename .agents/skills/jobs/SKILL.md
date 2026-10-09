---
name: jobs
description: Manage a local job-search workspace, including setup, tracker checks, job discovery, applications, referrals, and resuming saved progress.
---

# Jobs Workspace Entrypoint

This repository entrypoint forwards to the canonical Jobs skill packaged in this repository. Before an operational job-search request, read [the canonical skill](../../../plugins/job-search-assistant/skills/jobs/SKILL.md) and follow its workspace-resolution contract. Resolve that link relative to this file, not the current chat directory.

The installed plugin uses the same canonical skill directly. If both entrypoints are available, run the workflow once. The plugin package contains instructions and empty templates; private state belongs to the resolved user workspace.

For development of the assistant itself, use the repository's `.docs/PRD.md` and `.docs/plan.md`; do not initialize a candidate workspace merely to edit the plugin.
