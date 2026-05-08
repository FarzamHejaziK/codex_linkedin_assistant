# Referral Orchestrator

Use this for `jobs referral`. This is one orchestrator, not a router.

## Order Of Operations

1. Read tracker and outreach logs.
2. Enforce expired referral deadlines.
3. Check warm replies in available LinkedIn/email context.
4. Handle contacts who agreed to refer.
5. Send referral materials through LinkedIn unless email is explicitly requested.
6. Handle declines and no-response outcomes.
7. Send follow-up nudges for stale outreach.
8. Check connection-pending acceptances.
9. Run new outreach for `Outreach Pending` companies.
10. Update tracker and logs.

## LinkedIn vs Email

Use email only when the referrer explicitly asks for email or provides an email address.

Otherwise handle everything in LinkedIn, including the role link, message, and resume attachment after file upload preflight and user approval.

## Warm Reply Classification

Classify replies as:

- Agreed to refer
- Asked for resume/CV
- Asked for role link
- Explicitly requested email
- Asked a clarifying question
- Declined
- Acknowledged only
- No response and deadline reached

## Referral Materials

Before sending:

- Verify the tracker URL points to the intended role.
- Ensure an application folder and `resume.pdf` exist.
- Tailor/render the resume if needed.
- Draft the message in the user's voice.
- Ask for approval before sending.

After confirmed send:

- Update contact log.
- Set `Referral Status=Got Referral` when appropriate.
- Append concise tracker notes.

## Follow-Up Nudges

Follow up when:

- `Referral Status=Outreach Sent`
- Last message was at least 3 days ago
- Today is before the referral deadline

Keep follow-ups short, low-pressure, and specific to the role. Ask approval before sending.

## New Outreach

For `Referral Status=Outreach Pending`:

- Search LinkedIn for contacts at the company.
- Prefer 1st-degree connections, recruiters, hiring managers, and relevant peers.
- Send first DMs to 1st-degree contacts after approval.
- Send no-note connection requests to 2nd-degree contacts when useful.
- Stop on platform blocks, rate limits, CAPTCHA, or unusual verification prompts.

## Contact Log Format

```markdown
# Outreach - <Company>

| Date | Name | Title | LinkedIn URL | Relationship | Status | Notes |
|---|---|---|---|---|---|---|
```

## State Transitions

- `Outreach Pending` -> `Connection Pending`
- `Outreach Pending` -> `Outreach Sent`
- `Connection Pending` -> `Outreach Sent`
- `Outreach Sent` -> `Got Referral`
- `Outreach Sent` -> `Declined`
- `Outreach Sent` -> `No Referral`
- Any expired waiting state -> `No Referral`
