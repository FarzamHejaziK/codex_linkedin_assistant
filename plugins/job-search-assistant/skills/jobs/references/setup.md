# Setup Workflow

Use for jobs setup, “help me get started,” or missing prerequisites. Read experience.md. Setup is progressive; completing all application fields is not required to discover jobs.

## First Search Readiness

1. Resolve/register the local workspace using local-persistence.md before any data writes. For authorized first-use initialization, create a missing tracker from the bundled template using workspace-files.md. Missing files in a registered workspace require recovery. Never overwrite data or alter the schema.
2. Verify privacy defaults before writing personal information. Existing files remain intact.
3. If no real resume exists, ask the user to attach it, paste its absolute path, or copy it into resumes/. Copy provided files locally without overwriting an existing filename. Pause resume-dependent work until it exists.
4. Extract supported non-sensitive facts from the resume. Preserve existing profile/preferences; unknown fields remain blank.
5. Ask only what is missing for useful matches, in one short batch: target roles/level, location or remote preference, and dealbreakers. Compensation is optional unless the user makes it a constraint. Do not ask again for clear existing preferences.
6. Create or update resumes/search_profile.md. Seed profile/personal_info.json and profile/screening_answers.md from known facts and explicit answers only. Mark unknown eligibility as unknown; never infer work authorization, sponsorship, or demographic disclosures.
7. Keep Indeed disabled by default. Mention that it can be enabled later; do not make an opt-in question a first-run blocker.
8. Default to the built-in browser when browser work is requested; do not ask users to install Chrome or choose between browsers unless needed. Report local search inputs as ready, incomplete, or missing. Suggest the first three matches. Before actually searching LinkedIn, run browser-preflight.md and report browser readiness separately.

If browser access is unavailable, local setup can complete. Say “Local setup is ready; LinkedIn search needs a working browser connection,” rather than declaring the whole workflow ready. Follow preflight repair guidance without blocking local checks or drafting.

## Just-in-Time Application Intake

When preparing an application, use existing sources first. Ask only for missing fields required by that application. Explain why a sensitive answer is needed and obtain an explicit answer. Save reusable answers only when appropriate; leave voluntary demographic fields unknown unless explicitly provided.

Choose the resume backend when tailoring is needed: use an existing editable source, or explain PDF-only limitations. Do not require a first-time user to understand LaTeX or select an export backend before finding jobs. Never invent experience or silently convert a PDF into an allegedly verified editable resume.

Check upload capabilities just before the first needed attachment. In the built-in browser, use the documented manual handoff when automation is unsupported. Check extension file URL access only for a Chrome path that requires it. Read approvals.md before any external action.

## Optional Indeed

When the user asks to enable Indeed or requests additional sources, follow indeed.md. Discovery and public profile optimization are separate choices. A user declining optimization may still enable discovery. Preserve other preferences; keep LinkedIn primary. An Indeed block must not stop other work.

## Safe Creation

Never overwrite resumes, profile answers, tracker rows, outreach logs, application folders, or unfinished session notes. For a malformed file, explain the problem and propose a precise repair while preserving original content. End with one useful next action, not a request to repeat setup that already succeeded.
