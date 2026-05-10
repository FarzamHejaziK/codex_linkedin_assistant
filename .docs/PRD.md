# PRD: Codex LinkedIn Job Search Assistant

## 1. Summary

Build a generic repo-first Codex assistant for managing a local job-search workflow from discovery through referral outreach, resume tailoring, application submission, follow-up, and tracking.

The user opens this repository directly in Codex. Codex reads `AGENTS.md`, then uses the project-local skill at `.agents/skills/jobs/SKILL.md` and its Markdown references. The product should behave like a normal workspace repo, not as an installable bundle.

The assistant should:

- Track jobs in `job_tracker.csv`.
- Read resumes and a search profile from `resumes/`.
- Maintain reusable application memory in `profile/`.
- Store per-application materials in `applications/`.
- Store outreach/contact logs in `outreach/`.
- Support resume tailoring through the user's chosen source format.
- Orchestrate referrals, follow-ups, and application submission with explicit approval gates.

## 2. Goals

- Make this repository directly usable in Codex without a separate installation step.
- Keep the workflow generic and free of private candidate data.
- Preserve the existing tracker schema exactly.
- Support `jobs setup`, `jobs check`, `jobs find`, `jobs referral`, `jobs apply`, and `jobs daily`.
- Keep all instructions prompt-first and Markdown-only.
- Add root workspace files and docs modeled after the public LinkedIn assistant pattern.
- Use `.agents/skills/jobs/SKILL.md` as the primary instruction surface.
- Keep `.docs/PRD.md` and `.docs/plan.md` as canonical design references.

## 3. Non-Goals

- No app code, helper programs, automation scripts, or custom ATS libraries.
- No standalone `jobs add` or `jobs update` workflow.
- No private candidate identity, resume content, application history, contact history, or company history.
- No committed real resumes, profile files, outreach logs, or application folders.
- No fixed assumption that every user uses LaTeX.
- No automated sending or submitting without explicit approval.

## 4. Product Shape

The repository root is the product.

```text
AGENTS.md
README.md
REQUIREMENTS.md
job_tracker.csv
.gitignore
.docs/
  PRD.md
  plan.md
.agents/
  skills/
    jobs/
      SKILL.md
      references/
resumes/
profile/
base_resumes/
applications/
outreach/
```

The user opens the repo in Codex and invokes job-search intents in chat. `AGENTS.md` points Codex to `.agents/skills/jobs/SKILL.md`, which loads the relevant reference docs.

## 5. Tracker Schema

Keep the tracker header exactly:

```csv
Priority,Company,Role,Location,Type,Salary,Status,Applied Date,Next Action,URL,Notes,Discovered Date,Referral Needed,Referral Status,Referral Deadline,Apply Via
```

Canonical values:

- `Priority`: `HIGH`, `MEDIUM`, `LOW`
- `Status`: `To Apply`, `Applied`, `Recruiter Call`, `Phone Screen`, `Onsite`, `Offer`, `Rejected`, `Withdrew`
- `Referral Needed`: `YES`, `NO`
- `Referral Status`: `Not Needed`, `Outreach Pending`, `Connection Pending`, `Outreach Sent`, `Got Referral`, `Declined`, `No Referral`

## 6. Core Workflows

### Startup Preflight

Every operational `jobs ...` workflow starts by confirming Codex can connect to Chrome, opening LinkedIn, and checking that the active LinkedIn profile name matches the name extracted from the user's resume. If the resume is missing, setup must run resume intake before profile matching. If Chrome is unavailable, LinkedIn is logged out, or the profile name does not match, the workflow stops and asks the user to correct the issue before continuing.

### Setup

`jobs setup` verifies workspace files, tracker schema, resume presence, application-memory examples, privacy defaults, resume backend choice, and browser preflight. If no real resume is present, setup must pause and ask the user to attach/upload a resume, paste an absolute local file path, or drag/copy the file into `resumes/`. After resume intake, setup should ask onboarding questions and create `resumes/search_profile.md`, `profile/personal_info.json`, and `profile/screening_answers.md` for the user instead of telling the user to create those files manually.

### Check

`jobs check` reads the tracker and outreach logs, then renders a dashboard for ready-to-apply jobs, referral deadlines, stale outreach, connection-pending rows, active outreach, and next actions.

### Find

`jobs find` reads resume(s) and optional `resumes/search_profile.md`, searches LinkedIn/web or intakes a pasted job link, dedupes against the tracker, scores candidates, and adds qualified rows. There is no separate add workflow.

### Referral

`jobs referral` is an orchestrator. It handles warm replies, agreed-to-refer contacts, LinkedIn-vs-email decisions, referral materials, follow-up nudges, connection checks, new outreach, and deadline enforcement.

### Apply

`jobs apply` handles ready-to-apply rows. It captures the job description, creates an application folder, tailors resume material when editable source exists, generates cover letters when required/requested, fills forms using profile memory, uploads files through browser tooling after preflight, and submits only after approval.

### Daily

`jobs daily` runs:

```text
check -> apply ready jobs -> referral -> find jobs -> instant-apply no-referral jobs -> commit -> summary
```

## 7. Privacy

The root `.gitignore` must protect:

- Real resume files except `resumes/README.md` and examples.
- Real profile files except examples.
- Base resume sources unless the user opts in.
- Application folders.
- Outreach contact logs.

Codex must never force-add ignored private data.

## 8. Acceptance Criteria

- The repo can be opened directly in Codex and understood through `AGENTS.md`.
- `.agents/skills/jobs/SKILL.md` exists and routes all job intents to reference docs.
- Root README explains setup, workflows, browser file access, resume backends, privacy, and tracker schema.
- No package-only structure remains.
- No app/helper/script files are introduced.
- No private user-specific data is committed.
