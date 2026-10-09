# Local Workspace Persistence

Read this first for every operational job-search request, including setup, check, and continue. This plugin is local-only and needs filesystem access to the user's workspace. If the session cannot access that computer/path, explain the limitation; do not create a new workspace in a temporary sandbox or claim cloud synchronization.

## Two Roots

- **Skill root:** the directory containing this SKILL.md. Resolve `references/` and `assets/templates/` relative to that directory. The installed plugin is an instruction/template package, not a user data directory.
- **Workspace root:** the canonical absolute directory selected below. Resolve every tracker, resume, profile, application, outreach, and checkpoint path in workflow references from here, regardless of the chat's current working directory. Use absolute paths for file tools, browser attachments, and user-facing file links.

Do not put working files inside the installed plugin, its versioned cache, or its source package directory. The repository root can remain an existing user workspace because its packaged plugin is in the separate `plugins/job-search-assistant/` subtree.

## Stable Local Pointer

Resolve the actual OS user home directory through the environment/runtime; never hardcode a username, use the plugin owner's home, or redefine HOME. The pointer is:

```text
<user-home>/.config/codex-job-search-assistant/workspace.json
```

This is this plugin's documented local configuration convention, not an OpenAI configuration setting. It works independently of the current chat directory and plugin version.

The pointer has this JSON shape (values below are placeholders):

```json
{
  "schema_version": 1,
  "workspace_id": "<generated UUID>",
  "workspace_path": "<canonical absolute path>"
}
```

The corresponding workspace marker at `profile/workspace.json` contains:

```json
{
  "schema_version": 1,
  "workspace_id": "<same UUID>"
}
```

Store only location/identity in the pointer; candidate details stay in the workspace. Restrict new configuration directories/files to the user where the filesystem supports it (directory 0700, file 0600 on Unix). Never print full profile contents during a connection/readiness check.

## Resolve Before Reading or Writing

1. Honor an explicit workspace path in the current request. Treat it as a one-run selection unless the user asks to make it the default. Validate/register that workspace before use; do not replace the saved default implicitly.
2. Otherwise read the saved pointer and use its workspace, even when the current chat is in an unrelated directory. Do not auto-switch based on a tracker found in the current directory.
3. If there is no pointer, look only at the current project for an existing workspace marker or canonical tracker plus job-search folders. Offer to reuse it, or let the user choose/create a separate local folder. For setup, propose `<user-home>/Documents/Job Search` when no existing workspace is selected. Do not initialize the current directory merely because a plugin was invoked there. An explicit setup request naming a folder authorizes using it.
4. Validate pointer/marker JSON, version 1, UUID format, matching IDs, canonical absolute path, directory existence, and access needed for the requested operation. Reject an installed/source plugin subtree as the workspace, including symlinks resolving into it. Validate the tracker header with tracker-schema.md. Read-only checks need read access; mutations also need write access.
5. If the pointer is malformed, references a missing/moved directory, has an unsupported version, or mismatches the marker, stop workspace-dependent operations and explain the precise repair. Do not fall back to the current directory, create an empty tracker, or silently reset registration. Local preparation independent of that workspace can continue only when explicitly scoped.
6. With a valid workspace, read only the files needed for the requested workflow. For continuation or a multi-step run, also reconcile profile/session.md and confirmed tracker/contact evidence. Briefly identify the workspace on first use in a new chat; do not make the user reselect it on every command.

## Register or Initialize

Register only after the user selects an existing workspace or authorizes creation. Inspect before writing. When adopting existing data, verify the exact tracker header and preserve every row, resume, answer, application, and log. Never seed over existing content.

For a new workspace:

1. Create the workspace and needed data folders. Add privacy ignore entries from workspace-files.md before writing candidate data; preserve any existing .gitignore.
2. Initialize only missing files from skill-root assets/templates/: job_tracker.example.csv becomes workspace-root job_tracker.csv; profile and search examples can seed guided setup when needed. The template names are not live data paths. Do not require a public example CSV to exist in the workspace.
3. Create profile/workspace.json with version 1 and a generated UUID. If a valid marker already exists, retain its ID.
4. Save the default pointer only when establishing the first default or when the user asks to change it. Write to a sibling temporary file, validate, and replace the pointer atomically where supported. Keep a private backup when changing an existing pointer; never overwrite malformed config without an explicit repair decision.
5. Read back the pointer, marker, and header. Report persistence as configured separately from resume/search readiness or browser readiness.

For an existing registered workspace, a missing tracker or marker is a recovery issue, not first-run initialization. Ask whether to restore a backup or explicitly recreate the missing file. Never describe a missing tracker as zero applications.

## Writes, Continuation, and Lifecycle

- The CSV is the only authority for job state. Session notes are progress, not a second tracker. Approval, credentials, browser cookies, and active login sessions are not stored as reusable authorization.
- Keep one modifying workflow active per workspace. Before replacing a tracker/profile/checkpoint, re-read it and reconcile any intervening changes. Do not overwrite another run's work. This skills-only version does not implement transactional multi-writer locking.
- For structured local state, prepare a sibling temporary file, validate syntax/schema, atomically replace when supported, and read back. Preserve existing data on failure. Save external outcomes as soon as verified; on an uncertain outcome, inspect before retrying.
- Plugin updates replace instructions/templates, not user files. Never rerun first-use initialization just because the plugin version changed. Unsupported data-schema versions require a reviewed migration and backup; do not change the 16-column tracker schema.
- Plugin uninstall must not remove the external workspace or pointer. Local persistence survives chats/app restarts as long as these files remain accessible. It is not a backup, device synchronization, or a guarantee that the current interrupted write completed.
- For a moved workspace, locate the user-specified new path, verify the same marker ID and existing tracker, then update the pointer after the user asks to relink it. For a different workspace, register its own ID and change the default only on request.
- Unlinking a default removes only the pointer after the user requests it. Deleting resumes/history requires a separate explicit request. Never bundle personal data into a plugin release.
