# Implementation Plan: Codex LinkedIn Job Search Assistant Plugin

## 1. Implementation Strategy

Implement this as a generic Codex plugin, not as a direct copy of the Claude command repo.

The private workflow is a feature reference only. The public repo is the baseline behavior reference. The new implementation should be Codex-native, installable, generic, and free of any user-specific content.

Core design principles:

- Plugin ships instructions, templates, scripts, and adapters.
- User workspace stores all private data.
- Tracker schema stays unchanged.
- No standalone `add` or `update` workflow.
- Referral is one orchestrator.
- Daily is one orchestrator.
- Resume generation supports multiple backends.
- Browser automation uses Codex Chrome extension, including file upload when file URL access is enabled.

## 2. Proposed Repository Layout

Create the plugin in this repo:

```text
plugins/codex-linkedin-job-assistant/
  .codex-plugin/
    plugin.json
  skills/
    job-search/
      SKILL.md
      references/
        tracker-schema.md
        workflows.md
        browser-preflight.md
        resume-backends.md
        referral-orchestrator.md
        application-flow.md
  scripts/
    init_workspace.sh
    validate_workspace.sh
    tracker/
      validate_tracker.py
      read_dashboard.py
      update_tracker.py
    resume/
      build_latex.sh
      render_markdown.py
      render_docx.sh
      extract_resume_text.py
    files/
      sanitize_folder_name.py
      make_application_folder.py
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
  assets/
    logo.png
```

Optional marketplace entry:

```text
.agents/plugins/marketplace.json
```

## 3. Plugin Manifest Plan

Use the plugin creator workflow to scaffold:

```text
plugins/codex-linkedin-job-assistant/.codex-plugin/plugin.json
```

Manifest requirements:

- Name: `codex-linkedin-job-assistant`
- Display name: `Codex LinkedIn Job Assistant`
- Category: `Productivity`
- Include the job-search skill.
- Include scripts and templates as plugin assets.
- Do not include private workspace state.

Marketplace entry if needed:

```json
{
  "name": "codex-linkedin-job-assistant",
  "source": {
    "source": "local",
    "path": "./plugins/codex-linkedin-job-assistant"
  },
  "policy": {
    "installation": "AVAILABLE",
    "authentication": "ON_INSTALL"
  },
  "category": "Productivity"
}
```

## 4. Skill Design

Create one primary skill:

```text
skills/job-search/SKILL.md
```

The skill should trigger on:

- "jobs daily"
- "job search"
- "find jobs"
- "apply to jobs"
- "tailor resume"
- "referral"
- "LinkedIn outreach"
- "job tracker"

The skill should define the user-facing workflows:

- `jobs setup`
- `jobs check`
- `jobs find`
- `jobs apply`
- `jobs referral`
- `jobs daily`

There should be no `jobs add` and no `jobs update`.

Implementation note:

- Codex does not need Claude-style `.claude/commands`.
- If command-like docs are useful, keep them as references under the skill, not as `.claude` command files.

## 5. Workspace Initialization

Implement `scripts/init_workspace.sh`.

Responsibilities:

- Create `job_tracker.csv` if missing.
- Create `resumes/README.md`.
- Create optional `resumes/search_profile.example.md`.
- Create `profile/personal_info.example.json`.
- Create `profile/screening_answers.example.md`.
- Create `applications/`, `outreach/`, `base_resumes/`.
- Create or append safe `.gitignore` entries.
- Never overwrite user files without explicit confirmation.

Tracker template:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

## 6. Preflight Design

Preflight runs at the start of setup, daily, apply, find, and referral when needed.

Checks:

1. Workspace root exists.
2. Tracker exists and schema matches exactly.
3. Resume source exists.
4. Resume text is extractable enough to identify user name, role/title, and skills.
5. Optional search profile is loaded.
6. Codex Chrome extension is installed and connected.
7. LinkedIn is logged in.
8. LinkedIn profile matches resume identity or user confirms.
9. Chrome extension has "Allow access to file URLs" enabled.
10. File upload smoke test passes if application/referral file upload will be needed.
11. Resume backend tools are present.
12. Git state is known.

Chrome extension setup instructions to include in README and setup output:

```text
Chrome -> Extensions -> Manage Extensions -> Codex extension -> Details
Enable: Allow access to file URLs
```

File upload smoke test:

- Generate a harmless local text file in a temporary location.
- Attempt to access it through a `file://` URL or upload it into a harmless local test page.
- If blocked, instruct the user to enable file URL access.

## 7. Tracker Module

Implement reusable tracker utilities in:

```text
scripts/tracker/
```

Functions:

- Validate schema.
- Read rows as structured records.
- Preserve row order and untouched fields.
- Write updates atomically.
- Deduplicate by URL first, then company/role/location.
- Compute ready-to-apply queue.
- Compute referral deadline queue.
- Compute follow-up queue.
- Render markdown dashboards.

Important:

- Do not require new columns.
- If a future feature needs more metadata, store it in `Notes` or sidecar files, not schema changes.

## 8. Resume Backend Architecture

Create a backend interface documented in `references/resume-backends.md`.

Common contract:

```text
detect_backend(resumes/, base_resumes/)
extract_text(source)
select_template(job_description, profile)
tailor_source(source, job_description, user_profile)
render_pdf(source_path, output_pdf_path)
validate_pdf(pdf_path)
```

Backends:

### 8.1 LaTeX Backend

Files:

```text
scripts/resume/build_latex.sh
```

Requirements:

- Detect `pdflatex` or `latexmk`.
- Render PDF next to the tailored source.
- Clean temporary build files.
- Surface compiler errors clearly.

### 8.2 Markdown/HTML Backend

Files:

```text
scripts/resume/render_markdown.py
```

Requirements:

- Use a simple Markdown or HTML template.
- Render to PDF using available local tooling.
- Good fallback for users without LaTeX.

### 8.3 DOCX Backend

Files:

```text
scripts/resume/render_docx.sh
```

Requirements:

- Accept `.docx` source/template.
- Edit through a document library or structured replacement strategy.
- Export to PDF through LibreOffice if available.

### 8.4 Existing PDF Backend

Requirements:

- Extract text for profile/search.
- Use PDF as-is for applications.
- Clearly mark tailoring as unavailable unless editable source is added.

Initial recommendation:

- Ship LaTeX plus Markdown/HTML first.
- Add DOCX if local dependencies are reliable.

## 9. Application Folder Module

Implement:

```text
scripts/files/sanitize_folder_name.py
scripts/files/make_application_folder.py
```

Folder format:

```text
applications/<YYYY-MM-DD>_<Company>_<Role>/
```

Create:

- `job_description.md`
- `resume_source.<ext>`
- `resume.pdf`
- `notes.md`
- `contacts.md`
- Optional `cover_letter.md`

Rules:

- Create folder when applying, preparing referral materials, or explicitly tailoring.
- Do not create folders just because a job was discovered.
- Sanitize spaces, slashes, commas, punctuation.

## 10. Application Memory

Files:

```text
profile/personal_info.json
profile/screening_answers.md
```

`personal_info.json` should include generic fields:

- `full_name`
- `email`
- `phone`
- `location`
- `linkedin_url`
- `portfolio_url`
- `github_url`
- `work_authorization`
- `sponsorship_required`
- `salary_expectations`
- `notice_period`
- `relocation`
- `onsite_remote_preferences`
- `demographic_disclosures`
- `work_preferences`

`screening_answers.md` sections:

- Standing answers.
- Company-specific answered questions.
- Date and company context.

Behavior:

- Use stored answers before asking.
- Ask when missing or ambiguous.
- Save new answers.
- Promote stable preferences to `personal_info.json`.

## 11. Find Workflow

Inputs:

- Resume text.
- Search profile.
- Tracker history.

Steps:

1. Build search profile from resume plus optional `resumes/search_profile.md`.
2. Search LinkedIn and web.
3. Extract company, role, location, type, salary, apply URL.
4. Deduplicate.
5. Score against preferences.
6. Determine referral need:
   - `YES` if company size >500 or LinkedIn followers >100K.
   - Else `NO`.
7. Write accepted/high-confidence jobs to tracker.
8. Return list of newly added no-referral jobs for instant apply.

No standalone add:

- If user pastes a job link, run it through find/intake logic.
- If user wants a manual row, the assistant can capture it through the find workflow.

## 12. Check Workflow

Dashboard sections:

- Ready to apply now.
- Referral deadlines due soon.
- Stale outreach follow-ups.
- Connection pending.
- Newly discovered / no-referral jobs.
- Applied jobs and next actions.

Output:

- Markdown tables.
- Short next-action summary.

## 13. Apply Workflow

Inputs:

- Ready-to-apply queue or explicit company/role.

Eligibility:

```text
Status=To Apply
AND (
  Referral Needed=NO
  OR Referral Status=Got Referral
  OR Referral Status=No Referral
  OR Referral Status=Declined
  OR Referral Deadline <= today
)
```

Steps:

1. Pick job.
2. Enforce referral gate.
3. Fetch/capture JD.
4. Create application folder.
5. Tailor resume if editable backend exists.
6. Render and validate PDF.
7. Generate cover letter only if required or requested.
8. Open application URL.
9. Detect LinkedIn Easy Apply vs external ATS.
10. Fill standard fields from profile memory.
11. Upload resume file using Codex Chrome extension.
12. Upload or paste cover letter if required.
13. Ask user for missing/sensitive answers.
14. Save new answers.
15. Show review summary.
16. Submit after explicit approval.
17. Update tracker:
    - `Status=Applied`
    - `Applied Date=today`
    - `Next Action=Follow up in 1 week`
    - `Apply Via=<method>`
18. Update notes.
19. Return summary.

ATS support phases:

- Phase 1: LinkedIn Easy Apply and generic form fill.
- Phase 2: Greenhouse, Lever, Ashby.
- Phase 3: Workday and account-heavy ATS flows.

## 14. Referral Orchestrator Design

User invokes:

```text
jobs referral
```

The orchestrator owns the lifecycle and does not ask the user to choose between subcommands.

Order:

1. Read tracker and outreach logs.
2. Check LinkedIn/email for warm replies.
3. Process contacts who agreed to refer.
4. Draft/send referral materials.
5. Process declines.
6. Convert expired no-response rows to `No Referral`.
7. Send follow-up nudges for stale outreach.
8. Check accepted connection requests.
9. Run new outreach for pending companies.
10. Update tracker/logs.

### 14.1 Warm Reply Handling

Classifications:

- Agreed to refer.
- Asked for resume/CV.
- Asked for role link.
- Explicitly requested email.
- Asked clarifying question.
- Declined.
- Acknowledged only.
- No response and deadline reached.

### 14.2 LinkedIn vs Email Rule

Use email only if explicitly requested.

Explicit email request examples:

- "Email me."
- "Send to my work email."
- "Use name@company.com."

Otherwise, handle in LinkedIn:

- "Send it here."
- "Share it here."
- "Drop the link."
- "Send it over."

### 14.3 Referral Materials Flow

For agreed referrals:

1. Verify tracker URL points to the intended role.
2. Ensure application folder and resume PDF exist.
3. Tailor/render resume if missing.
4. Draft short message.
5. Attach PDF through Codex Chrome extension.
6. Send after approval.
7. Update `Referral Status=Got Referral`.
8. Log contact state.

### 14.4 Follow-Up Nudge

Candidate criteria:

```text
Referral Status=Outreach Sent
last_message_date <= today - 3 days
today < Referral Deadline
```

Draft:

- Short.
- No filler.
- No misleading claims.
- Ask for approval before sending.

### 14.5 Outreach

Candidate criteria:

```text
Referral Needed=YES
Referral Status=Outreach Pending
```

Behavior:

- Search LinkedIn contacts.
- Prioritize 1st-degree contacts, recruiters, hiring managers, relevant peers.
- Send DMs to 1st-degree contacts.
- Send connection requests without notes to 2nd-degree contacts.
- Log every contact.

Connection request policy:

- Default to no note.
- Stop on LinkedIn-side blocks, rate limits, CAPTCHA, or suspicious behavior warnings.

## 15. Referral Deadline Enforcement

Function:

```text
enforce_referral_deadlines(today, tracker_rows)
```

Rules:

- `Referral Needed=NO`: `Referral Status=Not Needed`.
- `Outreach Pending` past deadline: mark `No Referral`, eligible to apply.
- `Connection Pending` past deadline: mark `No Referral`, eligible to apply.
- `Outreach Sent` past deadline: mark `No Referral`, eligible to apply.
- `Declined`: eligible to apply.
- `Got Referral`: eligible to apply.

Default:

- `Referral Deadline=Discovered Date + 5 days`.

## 16. Daily Orchestrator

User invokes:

```text
jobs daily
```

Sequence:

1. Preflight.
2. Check.
3. Apply ready jobs.
4. Referral orchestrator.
5. Find jobs.
6. Instant-apply newly found no-referral jobs.
7. Commit.
8. Final summary.

Details:

- Empty tracker: skip check details and start with find.
- Apply ready jobs before referral to clear old ready queue.
- Referral may create newly ready jobs; those are summarized and can be picked up in the next apply sweep or same-run if the user approves extending the run.
- Newly found no-referral jobs are instant-apply candidates in the same run.
- Commit only at the end.

## 17. Commit Design

If git repo:

```text
daily YYYY-MM-DD: <summary>

Applied:
- Company: Role via method

Referral:
- Company: outcome

New jobs:
- Company: Role [Priority]

Outreach:
- Company: N DMs, M connection requests

Generated:
- applications/<folder>/
```

If not git repo:

- Skip commit.
- Print: `No git repo detected, commit skipped.`

Never force-add ignored private data.

## 18. README Work

Create root/plugin README from template.

Must include:

- What the plugin does.
- Installation.
- Workspace setup.
- Chrome extension setup.
- Required file URL access toggle.
- Resume backend choices.
- Tracker schema.
- Daily workflow.
- Referral orchestrator.
- Application flow.
- Privacy defaults.
- Approval gates.
- Troubleshooting.

Important README language:

- "If you use LaTeX, the plugin can tailor `.tex` and compile PDFs."
- "If you use DOCX or Markdown/HTML, choose that backend during setup."
- "If you only have a PDF, the plugin can apply with it, but true tailoring requires editable source."

## 19. Testing Plan

### 19.1 Static Tests

- Validate plugin manifest JSON.
- Validate marketplace JSON if present.
- Validate tracker template header.
- Validate `.gitignore` template includes private paths.
- Validate skill file references existing relative files.

### 19.2 Unit Tests

Tracker:

- Schema validation.
- Deduping.
- Ready-to-apply queue.
- Referral deadline enforcement.
- Follow-up queue.

Files:

- Folder name sanitizer.
- Application folder creation.
- No overwrite behavior.

Resume:

- Backend detection.
- LaTeX render happy path.
- Missing dependency error.
- PDF-only fallback.

Memory:

- Screening answer lookup.
- Add new answer.
- Promote work preference.

### 19.3 Integration Tests

- Initialize empty workspace.
- Run check on empty tracker.
- Ingest a pasted job link through find/intake.
- Create no-referral job and instant apply dry run.
- Create referral-needed job and run referral dry run.
- Tailor sample LaTeX resume and compile.
- Upload smoke test with Chrome extension file URL access enabled.

### 19.4 Manual Browser Tests

- LinkedIn profile verification.
- LinkedIn job search.
- LinkedIn Easy Apply file upload.
- External ATS file upload.
- LinkedIn referral message with attachment.
- Email referral with attachment.

## 20. Implementation Phases

### Phase 0: Docs and Scaffold

- Add plugin scaffold.
- Add plugin manifest.
- Add marketplace entry if desired.
- Add skill skeleton.
- Add templates.
- Add README draft.

Deliverable:

- Installable plugin shell.

### Phase 1: Workspace and Tracker

- Implement workspace init.
- Implement tracker schema validation.
- Implement dashboard.
- Implement ready-to-apply and referral-deadline queues.

Deliverable:

- `jobs setup` and `jobs check`.

### Phase 2: Resume Backends

- Implement resume text extraction.
- Implement LaTeX backend.
- Implement Markdown/HTML backend.
- Add PDF-only fallback.
- Add backend setup docs.

Deliverable:

- Tailored resume PDF generation for editable sources.

### Phase 3: Find Workflow

- Implement search profile reading.
- Implement job discovery/intake.
- Implement dedupe and scoring.
- Implement referral-needed classification.
- Write tracker rows from accepted findings.

Deliverable:

- `jobs find` with no standalone add command.

### Phase 4: Application Flow

- Implement application folder creation.
- Implement JD capture.
- Implement profile memory reads.
- Implement standard field filling guidance.
- Implement file upload via Codex Chrome extension.
- Implement review and submit approval.
- Update tracker after confirmed submission.

Deliverable:

- `jobs apply`.

### Phase 5: Referral Orchestrator

- Implement outreach log format.
- Implement warm reply classification.
- Implement LinkedIn vs email routing rule.
- Implement referral materials flow.
- Implement follow-up nudges.
- Implement connection acceptance checks.
- Implement new outreach.
- Implement deadline enforcement.

Deliverable:

- `jobs referral` as orchestrator.

### Phase 6: Daily Orchestrator

- Compose preflight, check, apply, referral, find, instant-apply, commit.
- Add summary.
- Add empty tracker behavior.
- Add git/no-git handling.

Deliverable:

- `jobs daily`.

### Phase 7: Hardening

- Add tests.
- Add sample workspaces.
- Add browser verification checklist.
- Improve ATS adapters.
- Refine resume backend docs.

Deliverable:

- v1 candidate.

## 21. Migration Notes from Existing Repos

From public repo:

- Keep generic resume-driven identity.
- Keep search profile.
- Keep tracker schema.
- Keep LinkedIn preflight concept.
- Keep find/check/outreach baseline.
- Keep privacy-first `.gitignore`.

From private repo:

- Bring over feature ideas only:
  - Resume tailoring.
  - Application flow.
  - Persistent memory.
  - Application folders.
  - Warm replies.
  - Referral email/message handling.
  - Follow-up nudges.
  - Deadline enforcement.
  - Cover letters.
  - Screening answer reuse.
  - Daily orchestration.

Do not bring over:

- Candidate identity.
- Company/application history.
- Private profile files.
- Private contact logs.
- Private resumes.
- Standalone update.
- Standalone add.
- Claude-specific command structure.

## 22. Risks and Mitigations

Risk: ATS forms vary widely.

Mitigation:

- Start with LinkedIn Easy Apply and generic form support.
- Add ATS adapters incrementally.

Risk: Resume tailoring differs by backend.

Mitigation:

- Define backend interface.
- Ship LaTeX and Markdown/HTML first.
- Treat DOCX as optional if dependency behavior is unstable.

Risk: File upload blocked by extension settings.

Mitigation:

- Make "Allow access to file URLs" mandatory in preflight.
- Add smoke test.
- Provide exact Chrome setup instructions.

Risk: Plugin accidentally commits private data.

Mitigation:

- Strong `.gitignore` defaults.
- Never force-add ignored paths.
- Commit summaries only include paths and high-level outcomes.

Risk: No standalone add/update may frustrate users.

Mitigation:

- Manual links are handled by find/intake.
- Status changes are direct CSV edits or happen automatically through apply/referral.

## 23. Acceptance Criteria

- Plugin installs locally.
- `jobs setup` initializes a clean workspace.
- README explains Chrome file URL access and resume backend choices.
- Tracker schema remains unchanged.
- `jobs check` works on empty and populated trackers.
- `jobs find` can add jobs without a separate add command.
- `jobs apply` can create application folder, tailor/render resume, upload files, and update tracker.
- `jobs referral` runs as an orchestrator and handles replies, emails/messages, follow-ups, outreach, and deadlines.
- `jobs daily` runs the requested sequence.
- No private user-specific data exists in the repo.
