# Job Search Assistant: Directory Release

Release: 0.1.2. Prepared 2026-10-09.

## Scope

Submit a skills-only package to OpenAI's shared ChatGPT/Codex directory. The release includes the existing local job-search workflows, portable and Codex compatibility manifests, an original square SVG icon, a data-handling notice, and generic templates. It contains no MCP server, hooks, helper code, account system, candidate files, or background service.

Keep the existing local workspace pointer, marker, and 16-column tracker unchanged. Supported execution needs local filesystem access; browser workflows also require available tools and the relevant user account. Local storage is not a claim that model processing happens offline.

## Package

Build from a reviewed commit, using only its plugin subtree. Run from the repository root:

```sh
mkdir -p dist
git archive --format=zip --output=dist/job-search-assistant-0.1.2.zip HEAD:plugins/job-search-assistant
shasum -a 256 dist/job-search-assistant-0.1.2.zip
unzip -l dist/job-search-assistant-0.1.2.zip
```

The archive root contains plugin.json, .codex-plugin/plugin.json, LICENSE, PRIVACY.md, assets/, and skills/. Do not ZIP the whole workspace. dist/ is ignored and does not belong in the plugin.

The portable manifest owns OpenAI listing metadata under extensions.com.openai.interface. Keep the compatibility interface and both version fields identical. Both manifests link privacyPolicyURL to the public GitHub page for the exact bundled policy revision, so the link survives release-branch removal. The existing Jobs skill is the onboarding entrypoint; its setup workflow asks the user to select a local workspace and preserves existing data.

## Validation

Before submission:

- Validate both manifests and their referenced files, skill frontmatter/reference closure, field length limits, icon dimensions, and the header-only tracker template.
- Extract the final ZIP into a fresh temporary directory and compare its contents to the reviewed source; reject symlinks, unexpected files, credentials, private records, and machine-specific paths.
- Install the release through the supported local marketplace; verify installed source and version. Confirm the saved workspace pointer and private files are unchanged.
- Test setup and a returning chat in ChatGPT Work with a disposable workspace. Confirm that a cloud-only session without local access explains the limitation and creates no replacement workspace. Keep unperformed behavioral checks marked pending.
- Resolve the dashboard's actual findings. Local structure checks do not substitute for OpenAI's safety scans or human review.

## Submission

Open the [Plugins dashboard](https://platform.openai.com/plugins). Select the intended organization/project and verified developer identity, upload the ZIP, inspect Metadata & Skills, resolve required findings, then submit for review. Publish the approved version when ready. Identity verification and any account-specific requirements must be completed by the publisher.

The current submission flow does not support adding an MCP server to an existing skills-only listing. Revisit the distribution plan if a future version adds a hosted service.

## Status

- Package preparation and local validation: complete. Both manifests agree, the portable manifest passes its published JSON Schema, listing limits and local references pass, and the original 512px SVG was rendered and visually checked.
- Archive: 0.1.2 is being rebuilt after the privacy-policy URL update. The previous 0.1.1 archive passed extraction and byte comparisons for all 27 public package files.
- Local installation: 0.1.1 is installed and enabled through the existing marketplace; all 27 installed files match source. Eight existing private files, including the saved pointer, are unchanged. A process started outside the repository resolved the same workspace and matching 16-column tracker schema.
- Fresh ChatGPT Work behavioral tests: pending; not claimed by static package validation.
- Portal: 0.1.1 was uploaded under the verified individual identity. Metadata reported a non-blocking privacy-policy URL finding; 0.1.2 adds the public repository policy URL to address it. Revised upload, scan results, review, and publication remain pending.

## Sources

Requirements checked 2026-10-09:

- [Package your plugin](https://developers.openai.com/plugins/build/plugins)
- [Upload and submit](https://developers.openai.com/plugins/deploy/submission)
- [Submission validation errors](https://developers.openai.com/plugins/deploy/submission-errors)
- [Local files in ChatGPT Work](https://learn.chatgpt.com/docs/get-started-with-work)
