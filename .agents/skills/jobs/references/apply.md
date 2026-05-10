# Apply Workflow

Use this for `jobs apply`, instant-apply no-referral jobs, and application submissions.

## Eligibility

Apply only to rows that are ready under `tracker-schema.md`. If referral is still pending and the deadline has not passed, route to `jobs referral` instead.

## Steps

1. Select the target job from ready rows or from the user's explicit company/role.
2. Verify the job URL is live and points to the intended role.
3. Capture the job description. If the page is unreadable, ask the user to paste it.
4. Create the application folder.
5. Choose the resume backend and source.
6. Tailor resume source when editable source exists.
7. Render/export/obtain `resume.pdf`.
8. Validate the PDF before upload.
9. Generate `cover_letter.md` only if required or explicitly requested.
10. Open the application URL.
11. Fill standard fields using `profile/personal_info.json`.
12. Use `profile/screening_answers.md` before asking repeated questions.
13. Upload resume and required files after browser preflight passes.
14. Ask the user for sensitive, ambiguous, or unknown answers.
15. Save new reusable answers.
16. Show a final review summary.
17. Submit only after explicit approval.
18. Update tracker after confirmed submission.

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

Always ask before:

- Submitting an application
- Sending an email
- Sending a LinkedIn message with materials
- Answering sensitive questions
- Uploading materials to an unexpected third-party site
