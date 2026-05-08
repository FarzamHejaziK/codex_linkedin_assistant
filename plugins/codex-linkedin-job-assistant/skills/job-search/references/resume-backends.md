# Resume Backends

Users may use different resume sources. Support the user's chosen workflow without shipping renderer code in the plugin.

## Backends

### LaTeX

- Look for editable `.tex` files in `base_resumes/` or `resumes/`.
- Copy the chosen source into the application folder before editing.
- Tailor truthfully to the job description.
- Render PDF with available local tools such as `pdflatex`, `latexmk`, or the user's existing workflow.
- If rendering is unavailable, stop and explain what is missing.

### DOCX

- Look for `.docx` files in `base_resumes/` or `resumes/`.
- Use available document tools when present.
- If exact editing/export is not available, draft the content changes and ask the user whether to export manually or use another backend.

### Markdown/HTML

- Look for `.md`, `.html`, or template files.
- Edit source directly.
- Export with available local/browser/document tools.
- If export is unavailable, save the source and ask the user to export manually.

### Existing PDF Only

- Use the PDF as the attachment.
- Extract resume signals for search and profile verification where possible.
- Do not claim the PDF was tailored.
- Explain that true tailoring requires editable source.

## Tailoring Rules

- Never fabricate experience.
- Preserve truthful dates, companies, titles, credentials, and metrics.
- Rewrite for relevance, clarity, and keyword alignment.
- Save source and PDF inside the application folder.
- Validate that `resume.pdf` exists and is readable before upload or referral send.
