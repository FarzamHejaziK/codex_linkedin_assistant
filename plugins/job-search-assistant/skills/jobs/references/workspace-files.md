# Workspace Files

Resolve the workspace with local-persistence.md first. Every data path in this reference is relative to that verified workspace, not the plugin or chat directory. The user workspace should contain:

```text
job_tracker.csv              # private working tracker
resumes/
  README.md
  search_profile.md          # optional preferences and platform config
base_resumes/
  README.md
profile/
  workspace.json            # persistent identity marker
  personal_info.json
  screening_answers.md
  session.md                # private progress; not job-state authority
applications/
  README.md
  <YYYY-MM-DD>_<Company>_<Role>/
outreach/
  README.md
  <Company>_contacts.md
```

## Privacy Defaults

User-specific files should remain local by default:

- Working job_tracker.csv and session checkpoints
- Resume files
- Profile files
- Screening answers
- Application folders
- Outreach contact logs
- Base resume sources, unless the user explicitly wants to version them

Never force-add ignored private data to git.

## Folder Naming

Use:

```text
applications/<YYYY-MM-DD>_<Company>_<Role>/
```

Replace spaces with underscores. Strip slashes, commas, and path-unsafe punctuation. Keep names readable.

## Application Folder Contents

Create these when applying, preparing referral materials, or explicitly tailoring:

```text
job_description.md
resume_source.<ext>          # when editable source exists
resume.pdf
notes.md
contacts.md
cover_letter.md              # optional
```

Do not create application folders for every discovered job. Discovery belongs in the tracker until the job becomes application/referral-material ready.

## Templates, Initialization, and Migration

The header-only template lives at skill-root `assets/templates/job_tracker.example.csv`. The package also includes generic profile/search examples. A workspace does not need its own template copy. Existing repository examples remain documentation examples; keep their schemas aligned with the bundled templates when maintaining the package.

Follow local-persistence.md to select/register a workspace. During authorized first-use setup, copy the bundled header to a missing workspace-root job_tracker.csv. For an already registered workspace, missing tracker/marker files require recovery; do not silently create empty replacements. Existing tracker rows and file contents must be preserved.

For new workspaces, merge these privacy entries into .gitignore without removing user rules:

```gitignore
/job_tracker.csv
/profile/*
/resumes/*
/base_resumes/*
/applications/*
/outreach/*
```

In the source repository, preserve its existing exceptions for public README/example files. Verify private working paths are ignored. A tracked file is not protected merely by an ignore rule. Before recording personal job state in an older checkout, preserve a private backup, remove only a tracked working tracker's index entry with `git rm --cached -- job_tracker.csv` from the workspace, then verify retained contents and ignore behavior. Do not untrack unrelated files. Report other tracked personal paths before changing them.

If updating an older checkout could delete its tracked working file, preserve it outside the checkout before updating and restore afterward. This does not remove previously committed data from Git history. Never rewrite history or publish private backups during routine setup.
