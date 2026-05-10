# Setup Workflow

Use this when the user asks for `jobs setup`, opens a new workspace, or a required file is missing.

## Steps

1. Inspect the current folder.
2. Create missing workspace files from templates without overwriting user files.
3. Ensure `.gitignore` protects private job-search data.
4. Check whether `resumes/` contains at least one real resume file, excluding README/example files.
5. If no real resume exists, stop setup progress and ask the user to provide one using one of these options:
   - Attach/upload the resume in Codex chat.
   - Paste the absolute local file path.
   - Drag/copy the file into `resumes/` and reply `done`.
6. If the user provides an attachment or path, copy it into `resumes/` unless the user explicitly says not to. Keep the original filename unless there is a collision.
7. Help the user choose a resume backend: LaTeX, DOCX, Markdown/HTML, or PDF-only.
8. Ask onboarding questions and create missing setup files for the user.
9. Verify `job_tracker.csv` has the exact schema.
10. Run browser preflight before browser-dependent workflows.

## Resume Intake Gate

Do not say setup is complete or mostly done while the resume is missing. The resume is required for profile-name verification, job matching, search keywords, referral messaging, and resume tailoring.

When the resume is missing, say exactly what the user can do next:

```text
I do not see a real resume in resumes/ yet.

Send it one of three ways:
1. Attach/upload the resume here.
2. Paste the absolute file path, for example /Users/name/Desktop/resume.pdf.
3. Drag the file into resumes/ and reply done.

If you attach it or give me a path, I will copy it into resumes/ for you.
```

After the resume is present, continue setup from the remaining checklist.

## Guided File Creation

Do not tell the user to manually create setup files as the default path. Codex should ask questions and write the files itself.

Create or update these files when missing:

- `resumes/search_profile.md`
- `profile/personal_info.json`
- `profile/screening_answers.md`

Ask only for information needed to make a useful first version. Keep the question batch short enough for the user to answer in one message:

```text
I can create the setup files for you. Answer what you know and leave anything blank:

1. Target roles/titles:
2. Locations or remote preference:
3. Minimum or preferred compensation:
4. Work authorization and sponsorship needs:
5. Dealbreakers, such as industries, company stages, commute, travel, or role types:
6. Preferred resume backend: LaTeX, DOCX, Markdown/HTML, or PDF-only:
7. Basic application info I should remember: email, phone, LinkedIn URL, portfolio/GitHub URL, current location:
```

Use the resume to infer non-sensitive defaults when possible, then ask the user to confirm uncertain details. Do not fabricate anything. Leave unknown fields blank rather than guessing.

For `profile/screening_answers.md`, seed reusable answers from the user's responses, such as work authorization, sponsorship, relocation, salary range, notice period, and remote/onsite preference. Do not invent answers for demographic disclosures or sensitive questions.

After creating files, summarize what was created and what remains unknown. The next suggested action should be `jobs setup` again only when setup was blocked; otherwise suggest `jobs find` or `jobs daily` depending on readiness.

## Chrome Extension Setup

For uploads, the user must enable file URL access:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Enable: Allow access to file URLs
```

If this is not enabled, stop before application/referral file upload and walk the user through the setting.

## Safe Creation

Never overwrite:

- Existing resume files
- Existing profile files
- Existing tracker rows
- Existing outreach logs
- Existing application folders

If a file exists but appears malformed, report the issue and ask whether to repair it.
