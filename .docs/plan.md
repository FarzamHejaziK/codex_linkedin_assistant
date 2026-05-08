# Implementation Plan: Prompt-First Codex LinkedIn Assistant Plugin

## 1. Direction Change

For now, build this like the Claude assistant repo: a Codex plugin made primarily of Markdown instructions and templates.

Do not implement helper programs, custom ATS adapters, or resume-rendering programs in v1. The plugin should teach the agent what to do, what files to read/write, what checks to perform, and what decisions to make. Codex will use its normal tools at runtime to inspect files, edit Markdown/CSV, drive Chrome, run available user-local commands, and interact with the user.

Allowed v1 artifacts:

- Plugin manifest.
- Markdown skill files.
- Markdown workflow references.
- Markdown README/docs.
- Template files such as `.gitignore`, `job_tracker.csv`, example profile JSON, and example screening-answer Markdown.
- Optional static assets.

Not allowed in v1:

- Helper program files.
- Shell automation files.
- App source code.
- Custom browser automation libraries.
- Custom resume renderer code.
- Hardcoded private candidate data.

## 2. Plugin Package Shape

Create the plugin in this repo as:

```text
plugins/codex-linkedin-job-assistant/
  .codex-plugin/
    plugin.json
  skills/
    job-search/
      SKILL.md
      references/
        overview.md
        setup.md
        tracker-schema.md
        browser-preflight.md
        resume-backends.md
        check.md
        find.md
        apply.md
        referral.md
        daily.md
        writing-style.md
        workspace-files.md
  templates/
    README.md
    .gitignore
    job_tracker.csv
    resumes/
      README.md
      search_profile.example.md
    profile/
      personal_info.example.json
      screening_answers.example.md
    base_resumes/
      README.md
    applications/
      README.md
    outreach/
      README.md
```

Optional marketplace metadata:

```text
.agents/plugins/marketplace.json
```

The plugin is installable because of the manifest, but the actual product behavior lives in Markdown prompt files.

## 3. Manifest Plan

Create:

```text
plugins/codex-linkedin-job-assistant/.codex-plugin/plugin.json
```

Manifest intent:

- Name: `codex-linkedin-job-assistant`
- Display name: `Codex LinkedIn Job Assistant`
- Category: `Productivity`
- Expose the `job-search` skill.
- Include templates as plugin files.
- No private workspace data.

Keep the manifest minimal. Do not add MCP servers, hooks, scripts, or apps in v1.

## 4. Primary Skill

Create:

```text
plugins/codex-linkedin-job-assistant/skills/job-search/SKILL.md
```

Purpose:

- Main entry point for all job-search work.
- Tells Codex which reference files to read for each workflow.
- Defines command-like user intents without requiring a slash-command system.
- Enforces privacy, generic behavior, approval gates, and tracker schema.

Trigger phrases:

- "jobs setup"
- "jobs daily"
- "jobs check"
- "jobs find"
- "jobs apply"
- "jobs referral"
- "tailor my resume"
- "apply to this job"
- "handle referrals"
- "LinkedIn outreach"
- "job tracker"

The skill should explicitly state:

- There is no standalone `add` workflow.
- There is no standalone `update` workflow.
- Manual job links are handled by `find`/intake.
- Status changes happen through apply/referral outcomes or direct CSV edits by the user.

## 5. Reference Files

Each reference file is a Markdown workflow spec. The skill should load only the relevant reference files for the current task.

### 5.1 `overview.md`

Explain the product:

- Generic local job-search workspace.
- Tracker as source of truth.
- Resume/search profile as user source of truth.
- Referral orchestrator.
- Application folders.
- Persistent memory.
- Browser automation through Codex/Chrome.

### 5.2 `setup.md`

Prompt-only setup instructions:

- Create missing workspace folders/files by editing from templates.
- Never overwrite user files without confirmation.
- Ensure privacy defaults are present in `.gitignore`.
- Ask the user which resume backend they want.
- Help create `profile/personal_info.json` from example if missing.
- Help create `screening_answers.md` from example if missing.

Setup must include Chrome extension instructions:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Enable: Allow access to file URLs
```

The file URL setting is required because the assistant needs to upload local resume and cover-letter files through Chrome.

### 5.3 `tracker-schema.md`

Define the exact tracker schema:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

Include:

- Canonical statuses.
- Canonical priorities.
- Canonical referral status values.
- Date format.
- Preservation rules.
- Dedup rules.
- Ready-to-apply logic.
- Referral-deadline logic.

The agent should manipulate CSV carefully with structured parsing where possible, but no custom parser is shipped.

### 5.4 `browser-preflight.md`

Define browser checks:

- Codex Chrome extension is available.
- LinkedIn is logged in.
- LinkedIn profile matches the resume identity or user confirms.
- File URL access is enabled for the Codex extension.
- File upload should be tested before an application or referral send that needs attachments.

The agent can use whatever browser tools are available in the active Codex session. The Markdown should describe the required checks and failure messages, not implement them.

### 5.5 `resume-backends.md`

Define supported user choices:

- LaTeX source to PDF.
- DOCX source to PDF.
- Markdown/HTML source to PDF.
- Existing PDF only.

Important stance:

- The plugin does not ship renderer code in v1.
- Codex uses user-local tools if available.
- LaTeX users may use `pdflatex`, `latexmk`, or their existing workflow.
- DOCX users may use LibreOffice, Word, or available document tools.
- Markdown/HTML users may use available browser/PDF/export tools.
- PDF-only users can apply with the PDF, but true tailoring requires editable source.

The file should define how the agent chooses a base/source, tailors truthfully, validates the resulting PDF exists, and stores artifacts.

### 5.6 `check.md`

Dashboard workflow:

- Read `job_tracker.csv`.
- Render markdown tables.
- Show ready-to-apply jobs.
- Show referral deadlines.
- Show stale outreach.
- Show connection-pending jobs.
- Show newly found no-referral jobs.
- Show applied jobs and next actions.

No code. The agent computes this directly from the CSV.

### 5.7 `find.md`

Discovery/intake workflow:

- Read resume(s).
- Read optional search profile.
- Search LinkedIn and the web using available tools.
- Accept manual links pasted by the user as intake.
- Deduplicate against tracker.
- Score candidates.
- Decide priority.
- Decide referral need:
  - `YES` if company has more than 500 employees or more than 100K LinkedIn followers.
  - Otherwise `NO`.
- Add accepted jobs to tracker.
- Return newly added no-referral jobs for instant apply.

There is no separate add command. This file owns both discovery and manual job intake.

### 5.8 `apply.md`

Application workflow:

- Pick a ready job.
- Enforce referral gates.
- Capture job description.
- Create per-application folder.
- Tailor resume if editable source exists.
- Compile/export/render PDF using the user's selected workflow.
- Generate cover letter if required or explicitly requested.
- Open application URL.
- Fill standard fields from profile memory.
- Use stored screening answers before asking the user.
- Upload resume through Chrome extension after file URL access is enabled.
- Ask the user for sensitive or unknown questions.
- Save new reusable answers.
- Show final review summary.
- Submit only after explicit approval.
- Update tracker after confirmed submission.
- Update notes/contact artifacts.

The agent performs actions from instructions, using normal Codex tools. The plugin does not ship form-filling code.

### 5.9 `referral.md`

Referral must be an orchestrator, not a router.

The orchestrator order:

1. Inspect tracker and outreach files.
2. Check warm replies.
3. Handle contacts who agreed to refer.
4. Send referral materials by LinkedIn unless email is explicitly requested.
5. Handle declined/no-response outcomes.
6. Send follow-up nudges.
7. Check connection-pending acceptances.
8. Run new outreach.
9. Enforce deadlines.
10. Update tracker and logs.

LinkedIn vs email rule:

- Use email only if the referrer explicitly asks for email or gives an email address.
- Otherwise handle resume, role link, and message in LinkedIn.

Attachment behavior:

- File uploads are allowed after Chrome extension file URL access is enabled.
- Still require explicit user approval before sending LinkedIn messages or emails with referral materials.

### 5.10 `daily.md`

Daily orchestrator:

1. Preflight.
2. Check.
3. Apply ready jobs.
4. Referral orchestrator:
   - Handle replies.
   - Send referral materials.
   - Send follow-ups.
   - Check pending acceptances.
   - Run new outreach.
5. Find jobs.
6. Instant-apply newly found no-referral jobs.
7. Commit if in a git repo.
8. Summary.

No separate add/update.

### 5.11 `writing-style.md`

Generic writing rules:

- No fabricated experience.
- Outgoing messages should sound like the user.
- Avoid AI tells.
- Avoid saying "tailored resume" to contacts.
- Use first person for user-sent messages.
- Use concise referral and follow-up language.
- Let the user approve externally visible messages.

Do not include private wording from the old private workspace.

### 5.12 `workspace-files.md`

Define file layout:

```text
job_tracker.csv
resumes/
base_resumes/
profile/
applications/
outreach/
```

Include file creation rules and privacy defaults.

## 6. Templates

Templates should be plain files the agent can copy or recreate by reading instructions.

### 6.1 `templates/job_tracker.csv`

Header only.

### 6.2 `templates/.gitignore`

Privacy-first defaults:

```gitignore
# Personal data
resumes/*
!resumes/README.md
profile/*
!profile/*.example.*
base_resumes/*
!base_resumes/README.md

# Generated/private job-search state
applications/
outreach/*.md
!outreach/README.md

# Local app/editor noise
.DS_Store
.vscode/
.idea/
```

### 6.3 `templates/README.md`

Workspace README for users:

- What this workspace is.
- How to add resume materials.
- How to choose resume backend.
- How to enable Chrome extension file URL access.
- How to run `jobs daily`, `jobs check`, `jobs find`, `jobs apply`, `jobs referral`.
- Privacy explanation.

### 6.4 `templates/profile/personal_info.example.json`

Generic fields only:

- Full name.
- Email.
- Phone.
- Location.
- LinkedIn URL.
- Portfolio/GitHub URL.
- Work authorization.
- Sponsorship.
- Salary expectations.
- Notice period.
- Relocation.
- Remote/onsite preference.
- Demographic disclosure preferences.
- Other work preferences.

No real values.

### 6.5 `templates/profile/screening_answers.example.md`

Sections:

- Standing answers.
- Saved custom answers.
- Company-specific notes.

No real values.

## 7. Prompt-Only Workflow Details

### 7.1 Setup

When the user asks to set up:

1. Inspect current folder.
2. Create missing template files directly.
3. Ask which resume backend they use.
4. Ask for resume files or paths.
5. Ensure `.gitignore` protects private data.
6. Walk the user through Chrome extension file URL access.
7. Verify tracker header.

No setup script.

### 7.2 Check

When the user asks to check:

1. Read tracker.
2. Render dashboard.
3. Surface queues.
4. Suggest next action.

No dashboard code.

### 7.3 Find

When the user asks to find or provides a job link:

1. Read resume/search profile.
2. Search or inspect supplied link.
3. Score jobs.
4. Add accepted rows to tracker.
5. Mark referral fields.
6. Return instant-apply candidates.

No add command.

### 7.4 Apply

When the user asks to apply:

1. Determine eligible jobs.
2. Enforce referral deadline/status.
3. Tailor/generate materials.
4. Use browser to apply.
5. Upload files through Chrome extension.
6. Ask approval before submit.
7. Update tracker and folders.

No application automation code shipped by plugin.

### 7.5 Referral

When the user asks referral:

1. Run the orchestrator.
2. Handle warm replies first.
3. Use LinkedIn unless email explicitly requested.
4. Send follow-ups.
5. Continue outreach.
6. Enforce deadlines.

No router subcommands.

### 7.6 Daily

When the user asks daily:

1. Run check.
2. Apply ready jobs.
3. Run referral orchestrator.
4. Find jobs.
5. Instant-apply new no-referral jobs.
6. Commit if possible.

## 8. Resume Backend Prompt Design

The Markdown should tell Codex how to handle each backend without shipping code.

### LaTeX

- Look for editable `.tex` sources.
- Copy the chosen source into the application folder.
- Edit truthfully for the JD.
- Run available local LaTeX command if present.
- If unavailable, explain setup and ask user how to proceed.

### DOCX

- Look for `.docx` source/template.
- Use available document tools if present.
- If exact editing/export is unavailable, draft content changes and ask the user whether to keep DOCX manual or convert to another backend.

### Markdown/HTML

- Use Markdown/HTML source where provided.
- Edit source directly.
- Render with available local/browser export tools.
- If export is unavailable, produce source and ask user to export manually.

### Existing PDF Only

- Use PDF as application attachment.
- Extract profile/search signals where possible.
- Do not claim it is tailored.
- Explain that editable source is needed for full tailoring.

## 9. Referral Orchestrator Prompt Design

The referral file should be especially detailed because this replaces multiple commands.

It should define queues:

- Warm replies.
- Agreed-to-refer contacts.
- Email-requested referrals.
- LinkedIn-materials-needed referrals.
- Stale follow-ups.
- Connection pending.
- New outreach pending.
- Deadline expired.

It should define state transitions:

- `Outreach Pending` -> `Connection Pending`
- `Outreach Pending` -> `Outreach Sent`
- `Connection Pending` -> `Outreach Sent`
- `Outreach Sent` -> `Got Referral`
- `Outreach Sent` -> `Declined`
- `Outreach Sent` -> `No Referral`
- Any expired waiting state -> `No Referral`

It should define contact log format:

```markdown
# Outreach - <Company>

| Date | Name | Title | LinkedIn URL | Relationship | Status | Notes |
|---|---|---|---|---|---|---|
```

## 10. Daily Orchestrator Prompt Design

`daily.md` should be strict about sequence:

```text
preflight
check
apply ready jobs
referral orchestrator
find jobs
instant apply no-referral jobs
commit
summary
```

The daily flow should not ask the user to choose internal subcommands. It can ask for approval before messages, uploads, application submissions, and ambiguous decisions.

## 11. Approval Gates

Require explicit approval before:

- Sending LinkedIn referral messages.
- Sending emails.
- Submitting applications.
- Answering sensitive questions.
- Uploading materials to a third-party site if the user has not reviewed the target.
- Changing `.gitignore` to allow private data commits.

Do not require approval before:

- Reading local files.
- Creating local workspace files from templates.
- Tailoring local resume source.
- Rendering dashboard.
- Preparing drafts.
- Updating local logs after confirmed actions.

## 12. Git Behavior

For daily:

- If the workspace is a git repo, inspect status.
- Commit changed public/planned files and generated state only if not ignored.
- Never force-add ignored private data.
- If not a git repo, skip commit and say so.

This behavior is described in Markdown only.

## 13. Manual Verification Plan

Since v1 is prompt-first, verification is walkthrough-based rather than unit-test based.

Check these manually:

1. Plugin manifest is valid.
2. Skill file references only existing Markdown/template files.
3. No helper programs, shell automation files, or app source files exist in the plugin.
4. Setup instructions create a sensible workspace.
5. Tracker template has the exact schema.
6. README explains Chrome file URL access.
7. Resume backend docs explain LaTeX, DOCX, Markdown/HTML, and PDF-only paths.
8. Referral is documented as one orchestrator.
9. Daily sequence matches the requested order.
10. No private user-specific data appears anywhere.

## 14. Implementation Phases

### Phase 1: Plugin Skeleton

- Create plugin folder.
- Create manifest.
- Create skill folder.
- Create reference folder.
- Create template folder.
- Add marketplace entry only if needed.

Output:

- Installable prompt-first plugin shell.

### Phase 2: Core Markdown Specs

- Write `SKILL.md`.
- Write `overview.md`.
- Write `tracker-schema.md`.
- Write `workspace-files.md`.
- Write `writing-style.md`.

Output:

- Agent has global rules and data model.

### Phase 3: Setup and Preflight Specs

- Write `setup.md`.
- Write `browser-preflight.md`.
- Write template README and `.gitignore`.
- Write profile/resume template docs.

Output:

- Agent can initialize a clean workspace from prompts.

### Phase 4: Workflow Specs

- Write `check.md`.
- Write `find.md`.
- Write `apply.md`.
- Write `referral.md`.
- Write `daily.md`.
- Write `resume-backends.md`.

Output:

- Agent can run all requested workflows from Markdown.

### Phase 5: Documentation Polish

- Review for generic language.
- Remove any accidental private references.
- Remove any helper-program references.
- Verify no add/update command exists.
- Verify daily/referral designs match the requested order.

Output:

- v1 prompt-only plugin design ready for implementation.

## 15. Acceptance Criteria

- Repo contains a Codex plugin plan that is prompt-first.
- No proposed helper program files.
- No proposed shell automation files.
- No standalone add/update workflows.
- Referral is an orchestrator.
- Daily flow is:
  - check
  - apply ready jobs
  - referral
  - find jobs
  - instant-apply no-referral jobs
  - commit
- README/setup docs require enabling Chrome extension file URL access.
- Resume backend docs offer LaTeX, DOCX, Markdown/HTML, and PDF-only options.
- No user-specific private data.
