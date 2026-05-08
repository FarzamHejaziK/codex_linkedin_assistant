# Workspace Files

The user workspace should contain:

```text
job_tracker.csv
resumes/
  README.md
  search_profile.md          # optional
base_resumes/
  README.md
profile/
  personal_info.json
  screening_answers.md
applications/
  README.md
  <YYYY-MM-DD>_<Company>_<Role>/
outreach/
  README.md
  <Company>_contacts.md
```

## Privacy Defaults

User-specific files should remain local by default:

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
