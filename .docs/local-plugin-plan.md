# Local Plugin and Persistence Plan

## Design

Distribute a self-contained, skills-only plugin at `plugins/job-search-assistant/`. Keep the repository's existing `.agents/skills/jobs/` entrypoint and reference paths as forwarding documents so existing workspace usage still works. Canonical operational instructions and empty templates live inside the plugin. No server, hook, helper program, cloud storage, or background scheduling is introduced.

Keep private user data in a separate user-selected workspace. A local pointer at `~/.config/codex-job-search-assistant/workspace.json` records its absolute path and workspace ID. A matching ignored `profile/workspace.json` identifies the workspace. These are this assistant's configuration files, not a built-in Codex persistence API.

Every operational workflow resolves the workspace before reading or writing. Package references resolve from the skill location; tracker, resumes, profiles, materials, and checkpoints resolve from the workspace. Never use the current chat directory or installed plugin directory as an implicit data location. Preserve the existing tracker schema and files.

## Steps

1. Package the canonical Jobs skill and generic templates; preserve repository entrypoints.
2. Add workspace resolution, first-use registration, invalid/moved-workspace recovery, and safe local write instructions.
3. Align setup, continuation, workspace docs, root rules, and design docs with the two distinct roots.
4. Register this existing repository workspace locally without moving or replacing its data.
5. Register the local marketplace and install the plugin through the supported Codex CLI.
6. Validate skill/package structure, installed discovery, fresh-process workspace resolution, schema preservation, ignore rules, and a clean package containing no personal data.

## Persistence Contract

- New chats load the same workspace from the saved pointer when permissions allow.
- Explicit one-run workspace selection does not silently replace the default.
- Missing directories, mismatched IDs, invalid configuration, unsupported versions, and missing files in an existing workspace are reported; no silent empty replacement is created.
- Setup initializes only a new workspace or files explicitly identified as missing; it does not reset existing tracker rows, profile answers, or pending work.
- Updates and uninstall affect the plugin package, not the external workspace or pointer. Backups and device migration remain local user responsibilities.
- Keep one modifying workflow active per workspace. Re-read changed files before saving and avoid overwriting concurrent changes. This prompt-first version does not provide a transactional multi-writer database.
- Save progress during work; a crash can still interrupt the current write. Use a temporary file and atomic replacement for local structured-state updates where supported, then read back.

## Validation Results

Completed locally on 2026-10-09.

- Packaged Job Search Assistant 0.1.0 with portable and Codex compatibility manifests, the canonical Jobs skill, forwarding repository entrypoints, and four empty templates. No hooks, server, helper code, or personal working files are included.
- Registered the existing repository workspace using the external local pointer and ignored workspace marker. Existing tracker, resume/profile files, and application/outreach materials were preserved byte-for-byte. New pointer/marker files use mode 0600.
- Registered the `codex-job-search-local` marketplace and installed `job-search-assistant@codex-job-search-local` using the supported Codex CLI. Plugin listing confirms installed and enabled.
- Both canonical and forwarding skills pass Skill Creator validation. The validation dependency was provided through an isolated `uv` environment; no project runtime dependency was added.
- A fresh process started outside the repository resolved the saved workspace and read its tracker using the installed package's template. Marker identity and all 16 schema columns match.
- Installed package files exactly match the public source package. Local links and required reference paths resolve; no machine-specific workspace path or private working file is in the package.
- Staged and working diff checks pass. Earlier UX/browser work was preserved. These checks were performed before commit or publication.

Validation covers local installation and filesystem persistence/readback. It does not claim a live model-driven application workflow, browser sign-in, or submission test. A new chat may be needed to refresh the available plugin skills. File permissions still apply in each session.

## Maintaining the Local Install

Edit canonical instructions under plugins/job-search-assistant/skills/jobs/. Keep forwarding entrypoints intact. For a release/update, increment the version in both manifests, validate the package, then reinstall from the registered local marketplace. Verify the installed package version and saved workspace pointer afterward. Do not copy the working repository, private pointer, or marker into the installable package.
