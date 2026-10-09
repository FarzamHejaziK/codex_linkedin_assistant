# PRD: Codex Job Search Assistant

## Product

A generic, prompt-first local job-search plugin with a compatible repository workspace entrypoint. Markdown instructions and local files support discovery, referrals, truthful resume tailoring, applications, and follow-up. No app code, helper scripts, custom renderers, hidden services, or required optional platforms.

## User Experience Goals

- Reach useful matches after resume intake and essential search preferences.
- Accept natural language as well as existing jobs commands.
- Lead with up to three actionable recommendations, with urgency and blockers.
- Respect time/count limits and prepare-only requests; resume without repeating completed work.
- Explain fit, gaps, unknowns, and preference changes using source evidence.
- Make external approvals concrete and scoped; never infer authorization from saved progress.
- Keep local work available when browser access is unavailable; use the built-in browser by default and Chrome as an optional alternative.
- Keep all candidate state private, including the working tracker.

## Workspace and State

`AGENTS.md` points to `.agents/skills/jobs/SKILL.md`, which forwards to the canonical packaged skill at `plugins/job-search-assistant/skills/jobs/SKILL.md`. The installed plugin invokes that skill directly. It resolves the stable local workspace before loading experience.md and relevant workflow references. Canonical designs live here and in plan.md.

`job_tracker.csv` remains the sole source of truth for job state, but is local and ignored. `job_tracker.example.csv` publishes only the canonical header. Authorized first-use setup initializes the tracker from the bundled template without overwriting rows; missing files in registered state require explicit recovery. Existing checkouts preserve their working tracker before removing it from Git tracking; history is not rewritten automatically.

resumes/ holds candidate sources/preferences; profile/ holds answers and private session.md progress; applications/ holds materials; outreach/ holds contact logs. Checkpoints record steps and evidence links, never a competing job database or durable blanket approval.

## Local Plugin Persistence

The package is isolated under plugins/job-search-assistant/ and contains only instructions, manifests, original presentation assets, a license, a data-handling notice, and generic templates. The source repository's local marketplace points only to that subtree. Legacy workspace entrypoints forward to the canonical package rather than duplicating operational rules.

Persist workspace location/identity in `<user-home>/.config/codex-job-search-assistant/workspace.json`, with a matching ignored profile/workspace.json marker. This is a plugin-owned local convention, not a Codex config key. Every new operational chat resolves and validates it, then reads the selected workspace. Never treat the plugin cache or current chat directory as the implicit data root. Permission failures, moved paths, ID mismatches, and unsupported versions are explicit recovery cases.

Keep CSV job state authoritative and progress in profile/session.md. No automatic initialization on plugin version changes, uninstall deletion of private files, cloud backend, or scheduling. Use one modifying workflow at a time; prompt-first file operations do not provide transactional multi-writer guarantees.

## Public Plugin Distribution

Prepare the same skills-only package for the shared ChatGPT/Codex directory. Public listing metadata and icons live in the portable manifest's OpenAI extension, with matching Codex compatibility fields. Ship only the plugin subtree; generated ZIPs remain ignored. Directory installation does not grant file/browser/account access. Local persistence requires a supported session with access to the selected computer's files, including ChatGPT Work on desktop. Publishing does not add cloud storage or a hosted MCP service.

Keep local validation, live ChatGPT Work behavior, OpenAI scans, review, and publication as separate statuses. Do not claim public availability or tested platform support beyond observed evidence. See plugin-publication.md for the release procedure and current evidence.

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

## Workflows

### Setup

Read existing sources, obtain a real resume, and ask only for missing target roles/level, location/remote preference, and dealbreakers. Compensation is optional. Create local files for the user and distinguish local readiness from browser readiness. Ask application-specific questions, choose a resume backend, and verify upload access when needed. Indeed is disabled by default and offered later on request.

### Check

Local-only, usable without browser access or a resume. Show up to three distinct recommended actions with reasons, dates, and blockers, plus concise counts. Full queues are available on request. Empty trackers get one useful next step, not empty tables.

### Find

Run LinkedIn-first browser discovery; company boards/web search supplement it. If LinkedIn is blocked, fallback discovery needs explicit approval. Manual-link intake inspects the supplied job without unrelated searches. Default standalone discovery targets three qualified matches; user limits take precedence. Deduplicate, explain fit/gaps/unknowns, and save concise evidence in Notes. Feedback changes durable preferences only when intent is explicit.

### Apply and Referral

Prepare from local sources without browser preflight. Browser-dependent work needs a verified supported browser and relevant account. Built-in uploads default to a retained-tab manual handoff under current documented limitations; a supported Chrome path is optional. Apply uses truthful materials, known profile answers, and confirmed submission evidence. Referral handles replies, materials, follow-ups, and deadlines; an agreement to refer or sent resume alone is not a completed referral.

Both honor prepare-only, time/count limits, and selected jobs. The shared review shows destination, exact content/answers, and file versions before approval. Uploads, sends, connection requests, submissions, and public edits need explicit scoped authorization; changed payloads or destinations need renewed review. Unknown/sensitive answers require user input. Approval already given for the exact reviewed action remains valid.

### Daily and Resume

Default daily scope is up to three distinct jobs and at most one proposed outreach action per selected job. Prioritize urgent existing work; discover only when requested or batch capacity remains. Respect explicit limits, checkpoint between steps and around external actions, and summarize confirmed versus prepared outcomes. Time budgets are best effort. No automatic daily commits or implicit background scheduling.

“Continue” reconciles private progress with tracker/contact evidence before resuming. Uncertain external outcomes are inspected before retrying. Browser failures block only browser steps; local work continues within scope. Indeed failure affects only Indeed.

### Browser Contract

Prefer the Codex built-in browser, honor explicit browser/tab choices, and retain connected Chrome as an optional alternative. Codex browser-bound Computer Use is supported. Check actual connectivity and per-step capabilities before claiming readiness. For LinkedIn work, verify the active profile against the resume; for other authenticated services, verify the relevant account. Public job-page reading needs no unrelated LinkedIn sign-in. Local work is exempt. Recheck identity after reconnecting or changing browsers/accounts. Separate browser profiles do not imply shared logins or forms. Never evade site restrictions by switching providers. Follow browser-preflight.md for capability checks, manual attachment handoffs, and current documentation sources.

## Acceptance

Follow the concrete scenarios in ux-improvement-plan.md and local-plugin-plan.md. Validate consistency across root rules, skill routing, references, and README; unchanged tracker schema; ignored and untracked private state; header-only public template; no private candidate data or helper programs in the patch.
