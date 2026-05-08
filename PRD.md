# PRD: Codex LinkedIn Job Search Assistant Plugin

## 1. Summary

Build a generic, installable Codex plugin for managing a full job-search workflow from discovery through referral outreach, resume tailoring, application submission, follow-up, and tracking.

This product should inherit the public, user-generic behavior of `claude-linkedin-assistant`, then extend it with the richer workflow capabilities proven in the private job-search workspace. It must not include any user-specific identity, resume content, application history, contact history, names, companies, or profile assumptions from the private workspace.

The plugin should work as a reusable local job-search operating system:

- User places resume/profile materials in a workspace.
- Codex verifies browser and LinkedIn readiness.
- Codex finds and tracks jobs using the existing tracker schema.
- Codex manages referrals as an orchestrated lifecycle.
- Codex tailors resumes and generates cover letters using the user's chosen document pipeline.
- Codex fills application forms, uploads files through the Codex Chrome extension, and submits only at approved checkpoints.
- Codex commits the day's changes when running inside a git workspace.

## 2. Goals

- Package the assistant as a Codex plugin that can be installed.
- Keep the workflow generic and user-configurable.
- Preserve the existing `job_tracker.csv` schema exactly.
- Support end-to-end daily automation without standalone `add` or `update` commands.
- Add resume tailoring and PDF generation using multiple possible resume backends.
- Add application submission flow with local file upload support through the Codex Chrome extension.
- Add persistent application memory for reusable personal details and screening answers.
- Add per-application folders for generated artifacts.
- Add a referral orchestrator that handles warm replies, referral emails/messages, follow-up nudges, and outreach.
- Add referral deadline enforcement.
- Add cover letter generation when required or explicitly requested.
- Add learning of custom screening answers and reusable work preferences.
- Keep private user data local and gitignored by default.

## 3. Non-Goals

- No user-specific content from any private workspace.
- No hardcoded candidate name, spouse name, profile, email, resume details, target companies, or role specialization.
- No standalone `/jobs add` command.
- No standalone `/jobs update` command.
- No migration of the private job corpus.
- No committed resumes, outreach logs, profile files, application folders, or contacts in the plugin package.
- No dedicated automation-limitation document copied from the private workflow.
- No fixed assumption that every user uses LaTeX.
- No replacement for user judgment on sensitive fields or irreversible external actions.

## 4. Target Users

- Job seekers using Codex locally.
- Users who want a structured tracker plus browser-assisted LinkedIn workflow.
- Users applying to roles where referrals, custom resumes, and repeated screening answers matter.
- Users comfortable keeping job-search state in local files.

## 5. Product Shape

The product should be delivered as a Codex plugin.

Planned package shape:

```text
plugins/codex-linkedin-job-assistant/
  .codex-plugin/plugin.json
  skills/
    job-search/
      SKILL.md
  scripts/
    init_workspace.sh
    validate_workspace.sh
    resume/
      build_latex.sh
      render_docx.sh
      render_markdown.sh
  templates/
    job_tracker.csv
    README.md
    .gitignore
    profile/
      personal_info.example.json
      screening_answers.example.md
    resumes/
      README.md
  assets/
```

Optional marketplace entry:

```text
.agents/plugins/marketplace.json
```

The plugin should expose workflows through a Codex skill rather than Claude-specific slash-command files. The user-facing intents are:

- `jobs daily`
- `jobs check`
- `jobs find`
- `jobs apply`
- `jobs referral`
- `jobs setup`

There should be no `jobs add` or `jobs update` workflow.

## 6. Existing Tracker Schema

Keep the tracker columns exactly as-is:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

Canonical values:

- `Priority`: `HIGH`, `MEDIUM`, `LOW`
- `Status`: `To Apply`, `Applied`, `Recruiter Call`, `Phone Screen`, `Onsite`, `Offer`, `Rejected`, `Withdrew`
- `Referral Needed`: `YES`, `NO`
- `Referral Status`: `Not Needed`, `Outreach Pending`, `Connection Pending`, `Outreach Sent`, `Got Referral`, `Declined`, `No Referral`

The plugin may derive temporary in-memory queues, but it must not require schema changes for v1.

## 7. Workspace Data Layout

The user's workspace should be initialized with this generic local layout:

```text
job_tracker.csv
resumes/
  README.md
  search_profile.md          # optional
base_resumes/                # optional, depends on chosen resume backend
profile/
  personal_info.json
  screening_answers.md
applications/
  <YYYY-MM-DD>_<Company>_<Role>/
    job_description.md
    resume_source.<ext>
    resume.pdf
    cover_letter.md          # optional
    notes.md
    contacts.md
outreach/
  <Company>_contacts.md
```

Privacy defaults:

- `resumes/*` should be gitignored except `resumes/README.md`.
- `base_resumes/*` should be gitignored unless the user explicitly wants source resumes versioned.
- `profile/*` should be gitignored.
- `applications/` should be gitignored by default for public/shared repos.
- `outreach/*.md` should be gitignored.

## 8. Preflight Setup

`jobs setup` and the first step of `jobs daily` must verify:

1. Workspace initialized.
2. `job_tracker.csv` exists with the exact schema.
3. At least one usable resume exists or the user chooses to create/import one.
4. Optional `resumes/search_profile.md` is read if present.
5. Codex Chrome extension is installed and connected.
6. User is signed into LinkedIn in the Chrome profile used by the extension.
7. The LinkedIn profile name matches the resume name or the user explicitly confirms the mismatch.
8. Chrome extension has local file URL access enabled:
   - User must open Chrome extension details for the Codex extension.
   - Enable "Allow access to file URLs".
   - Preflight should test access by opening or uploading a harmless local file if possible.
9. Resume build backend is selected.
10. Required local tools for that backend are available.
11. Git is available if the user wants daily commits.

Resume backend setup options:

- **LaTeX**: user provides `.tex` base resumes. Plugin renders PDF with `pdflatex` or `latexmk`.
- **DOCX**: user provides `.docx` template/source. Plugin edits and exports PDF through LibreOffice or a document library.
- **Markdown/HTML**: plugin writes Markdown or HTML resume source and renders PDF through a local renderer.
- **Existing PDF only**: plugin can use the PDF for applications, but tailoring is limited unless the user also provides editable source.

The README generated by the plugin must explain these options clearly.

## 9. Core Workflows

### 9.1 Check

The check workflow reads `job_tracker.csv` and presents a dashboard:

- Ready to apply now.
- Referral deadlines approaching.
- Outreach needing follow-up.
- Connection pending.
- Jobs discovered but not acted on.
- Recently applied jobs and next actions.

It should render markdown tables, not raw CSV.

### 9.2 Find Jobs

The find workflow should:

- Read resume(s) and optional search profile.
- Build search queries from role targets, skills, locations, constraints, and deal-breakers.
- Search LinkedIn and web results through Codex browser/web tools.
- Deduplicate against `job_tracker.csv`.
- Score matches.
- Determine `Referral Needed` using the existing threshold:
  - `YES` if company has more than 500 employees or more than 100K LinkedIn followers.
  - Otherwise `NO`.
- Set `Referral Status`:
  - `Not Needed` for `Referral Needed=NO`.
  - `Outreach Pending` for `Referral Needed=YES`.
- Add accepted/high-confidence jobs directly to the tracker as part of find.

There is no standalone add command. Manual links pasted by the user should be handled by the find/intake workflow.

### 9.3 Resume Tailoring and Compilation

The apply workflow should tailor a resume before submission when editable source is available.

Required behavior:

- Capture or fetch the job description.
- Choose the best resume source/template based on the JD and user profile.
- Rewrite only truthful bullets and skills.
- Never fabricate experience.
- Generate `applications/<YYYY-MM-DD>_<Company>_<Role>/`.
- Save the JD.
- Save tailored source.
- Render `resume.pdf`.
- Validate the PDF exists and is readable before application or referral send.

Resume backend abstraction:

```text
read_source()
select_template(job_description, profile)
tailor_source(job_description, source)
render_pdf(source_path)
validate_pdf(pdf_path)
```

For PDF-only users:

- Use the existing PDF.
- Mark resume mode as `unchanged`.
- Tell the user that richer tailoring requires editable source.

### 9.4 Application Submission Flow

The apply workflow should:

- Identify whether the application is LinkedIn Easy Apply or external ATS/company site.
- Fill standard fields from `profile/personal_info.json`.
- Use `profile/screening_answers.md` before asking the user repeated questions.
- Upload `resume.pdf` through the Codex Chrome extension, relying on file URL access.
- Generate or upload a cover letter if required.
- Stop for sensitive or unknown high-risk questions.
- Save newly answered custom questions for reuse.
- Show a final review summary.
- Submit only after explicit user approval for that application.
- Update tracker after confirmed submission.
- Commit generated artifacts when in a git workspace.

Standard fields include:

- Name
- Email
- Phone
- Location
- LinkedIn URL
- Portfolio/GitHub URL
- Work authorization
- Sponsorship
- Years of experience
- Salary expectation
- Start date / notice period
- Relocation / commute / onsite preference
- Demographic disclosures if the user has chosen standing answers

### 9.5 Persistent Application Memory

The plugin should maintain:

```text
profile/personal_info.json
profile/screening_answers.md
```

The assistant should:

- Fill known answers silently where safe.
- Ask only when no stored answer exists.
- Save new answers with source company/date context.
- Promote stable preferences into `personal_info.json`.
- Keep all profile files local and gitignored by default.

### 9.6 Per-Application Folders

Every applied or referral-ready job should get:

```text
applications/<YYYY-MM-DD>_<Company>_<Role>/
  job_description.md
  resume_source.<ext>
  resume.pdf
  notes.md
  contacts.md
```

Optional:

```text
cover_letter.md
```

Application folders should be created when:

- The user applies.
- A referrer agrees and needs a resume before the application is submitted.
- The user explicitly asks to tailor materials for a role.

### 9.7 Referral Orchestrator

Referral must be an orchestrator, not a router.

The user should be able to run:

```text
jobs referral
```

The orchestrator should automatically inspect the tracker, outreach logs, LinkedIn, and email context, then process referral work in priority order:

1. Warm replies.
2. Contacts who agreed to refer.
3. Referral emails/messages needing resume and role link.
4. Declines or no-referral outcomes.
5. Stale outreach needing a follow-up nudge.
6. Connection-pending contacts that accepted.
7. New outreach for `Outreach Pending` companies.

It should not ask the user to choose a sub-command. It can ask for judgment when content is ambiguous or externally visible.

#### Warm Reply Handling

The orchestrator should classify replies:

- Agreed to refer.
- Asked for resume/CV and role link.
- Asked a clarifying question.
- Asked to email materials.
- Declined.
- Acknowledged only.
- No response / deadline passed.

#### Referral Message vs Email Rule

If the referrer explicitly asks for email, use email.

Examples:

- "Email me your resume."
- "Send it to name@company.com."
- "Can you email me the role link?"

If the referrer does not explicitly ask for email, handle everything in the LinkedIn message.

Examples:

- "Send it here."
- "Share your resume and the role."
- "Drop the link."
- "Send it over."

In the LinkedIn path:

- Verify role URL.
- Ensure a role-specific resume PDF exists.
- Type the message.
- Upload the PDF using Codex Chrome extension file upload support.
- Send only after user approval.

In the email path:

- Draft email to the provided address.
- Attach the PDF using file upload support.
- Send only after user approval.

#### Follow-Up Nudge

The orchestrator should find stale outreach:

- `Referral Status=Outreach Sent`
- Last DM at least 3 days ago
- Today before `Referral Deadline`

It should draft concise follow-ups and send after approval.

#### Outreach

For `Referral Status=Outreach Pending`, the orchestrator should:

- Search for relevant contacts.
- Prefer recruiters, hiring managers, team peers, and 1st-degree connections.
- Send first DMs to 1st-degree contacts.
- Send connection requests without notes to 2nd-degree contacts when appropriate.
- Log all contacts to `outreach/<Company>_contacts.md`.

### 9.8 Referral Deadline Enforcement

For `Referral Needed=YES`, the plugin should enforce deadlines:

- If outreach has not started, prioritize outreach.
- If `Connection Pending` and deadline not reached, wait or continue checking.
- If `Outreach Sent` and deadline not reached, allow follow-up nudges.
- If deadline reached with no referral, set `Referral Status=No Referral`.
- Once `No Referral`, `Declined`, or `Got Referral`, the job is eligible for apply.

Default referral deadline:

- `Discovered Date + 5 days`

This should be configurable later, but v1 can use 5 days.

### 9.9 Cover Letter Generation

The plugin should generate cover letters when:

- User explicitly asks.
- Application form requires one.

Default behavior:

- Skip optional cover letters unless configured otherwise.
- Keep cover letters short and role-specific.
- Save to `applications/<folder>/cover_letter.md`.
- Render/upload if the application requires a file.

### 9.10 Daily Orchestrator

The updated daily workflow should run:

1. Preflight.
2. Check dashboard.
3. Apply ready jobs.
4. Referral orchestrator:
   - Handle replies.
   - Send referral materials.
   - Send follow-up nudges.
   - Check accepted connections.
   - Run new outreach.
5. Find jobs.
6. Instant-apply newly found no-referral jobs.
7. Commit.
8. Summary.

Ready-to-apply jobs:

- `Status=To Apply`
- And one of:
  - `Referral Needed=NO`
  - `Referral Status=Got Referral`
  - `Referral Status=No Referral`
  - `Referral Status=Declined`
  - `Referral Deadline <= today`

Instant-apply no-referral jobs:

- Any newly found job with `Referral Needed=NO` should be applied in the same daily run unless the user opts out.

### 9.11 Commit Behavior

If workspace is a git repo:

- Commit tracker and generated files at the end of `jobs daily`.
- Use a structured commit message.
- Do not commit ignored personal files unless user explicitly changed ignore policy.

If workspace is not a git repo:

- Skip commit.
- Print a summary and suggest `git init` only if the user wants versioning.

## 10. Safety and Approval

The plugin can automate repetitive browser work, including file uploads when the Codex Chrome extension has file URL access enabled.

The plugin must still ask for explicit approval before:

- Sending a LinkedIn DM with referral materials.
- Sending an email.
- Submitting an application.
- Answering sensitive or ambiguous screening questions.
- Changing gitignore policy to commit private data.

The plugin should not ask for approval before:

- Reading local tracker/profile files.
- Rendering dashboards.
- Creating local application folders.
- Compiling resume PDFs.
- Logging outreach state.
- Preparing drafts.

## 11. README Requirements

The future README must include:

- Plugin installation instructions.
- Workspace initialization instructions.
- Chrome extension setup.
- Required step: enable "Allow access to file URLs" for the Codex Chrome extension.
- Resume backend options:
  - LaTeX
  - DOCX
  - Markdown/HTML
  - Existing PDF only
- Tracker schema.
- Daily workflow.
- Privacy and gitignore defaults.
- What is automated.
- What requires user approval.

## 12. Success Metrics

- A new user can install the plugin and initialize a clean job-search workspace.
- `jobs daily` can run without standalone add/update flows.
- The tracker schema remains unchanged.
- Resume tailoring works for at least LaTeX and one non-LaTeX backend in v1.
- File upload works when Chrome extension file URL access is enabled.
- Referral orchestrator can process warm replies, follow-ups, and new outreach without requiring the user to choose a sub-flow.
- Application folders are generated consistently.
- Screening answers are reused across applications.
- No user-specific private data is shipped in the plugin.

## 13. Open Decisions

- Which non-LaTeX resume backend should ship first: DOCX or Markdown/HTML?
- Should first-DM outreach require approval per message or support an autonomous safe mode?
- Should the plugin support Gmail/Outlook connectors directly, or browser-based email only in v1?
- Should the plugin initialize git automatically when no git repo exists, or only suggest it?
- How much ATS-specific logic should be included in v1 versus generic form filling?
