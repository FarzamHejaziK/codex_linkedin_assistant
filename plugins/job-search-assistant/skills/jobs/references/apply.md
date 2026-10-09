# Apply Workflow

Use this for `jobs apply`, scoped preparation of newly found jobs, and application submissions.

Read experience.md, session.md, and approvals.md. Honor job/count/time scope. In prepare-only mode, stop after materials and local review; do not open forms, upload, send, or submit. Saved job descriptions can support local preparation without a browser. Fetching a missing live description or entering a form requires browser preflight.

## Eligibility

Submit only rows that are ready under `tracker-schema.md`. Local prepare-only work may proceed for an explicitly selected job while a referral is pending; preserve referral state. Before submission, if the referral is still pending and its deadline has not passed, route to `jobs referral` instead.

## Steps

1. Select the target job from ready rows or the user's explicit company/role, within the requested limit. Use saved job descriptions for local-only preparation; label live availability as unverified until checked.
2. Before browser-dependent work, run browser preflight and verify the job URL is live and points to the intended role. In local-only preparation, defer this live verification until the browser phase.
   - For `Apply Via=Indeed` or notes containing `Indeed-only posting`, search for the matching role on the company careers site when possible.
   - Prefer a live company careers URL before applying.
   - If only the Indeed posting is live and credible, continue with the Indeed URL after noting that it is an Indeed-only application path.
3. Capture the job description. If the page is unreadable, ask the user to paste it.
4. Create the application folder.
5. Choose the resume backend and source.
6. Tailor resume source when editable source exists.
7. Render/export/obtain `resume.pdf`.
8. Validate the PDF before upload.
9. Generate `cover_letter.md` only if required or explicitly requested.
10. If prepare-only, link the materials, record pending live verification, checkpoint, and stop here. Otherwise run browser preflight before opening the application URL.
11. Fill standard fields using `profile/personal_info.json`.
12. Use `profile/screening_answers.md` before asking repeated questions.
13. Show exact files and destination using approvals.md and check browser upload capabilities. Use the built-in browser manual handoff when needed; keep the form available and verify the user-attached file afterward. Automate uploads only through a supported provider with scoped approval. Upload approval does not authorize submission.
14. Ask the user for sensitive, ambiguous, or unknown answers.
15. Save new reusable answers.
16. Show the completed application review using approvals.md; identify exact attachments, destination, answers, and unresolved fields.
17. Submit only after explicit approval.
18. Update tracker after confirmed submission and save confirmation evidence in application notes. Checkpoint uncertain outcomes; inspect before retrying to avoid duplicate submissions.

## Standard Field Sources

Use `profile/personal_info.json` for name, email, phone, location, LinkedIn URL, portfolio/GitHub URL, work authorization, sponsorship, salary expectations, notice period, relocation, remote/onsite preference, and disclosure preferences.

## Tracker Update After Submission

Set:

- `Status=Applied`
- `Applied Date=today`
- `Next Action=Follow up in 1 week`
- `Apply Via=<method>`

Update the application folder notes with the submission date, method, referral status, and any important blockers or manual steps.

## Approval Gates

Use approvals.md consistently. Respect prepare-only scope. Ask only for missing information needed for this application, using known profile answers first. Never infer sensitive answers or carry approval to changed content/destinations.
