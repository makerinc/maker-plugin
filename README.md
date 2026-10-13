# Maker

Maker connects your existing account so you can build, inspect, preview, and publish websites from an AI conversation. The plugin includes two small workflows: identify the connected organization and project, and publish an approved website. It uses Maker's remote MCP server, authenticated with OAuth. Publishing requires an eligible Maker account and explicit confirmation.

The package sends requested tool inputs to Maker and returns relevant project files, previews, account details, and publication results. Preview screenshots are stored at publicly readable URLs. Maker handles storage and processing under its [privacy policy](https://www.maker.co/privacy-policy) and [terms](https://www.maker.co/terms). It contains no executable hooks or local MCP server. Source code remains in Maker projects; this package does not grant access to unrelated local files.

For support, email [support@maker.co](mailto:support@maker.co). See the plugin homepage for the live tool contract and authentication setup.

## Local validation and packaging

From the plugin folder (the root of the dedicated distribution repository):

```sh
claude plugin validate --strict .
```

For local Claude Code testing, pass this folder's absolute path to `claude --plugin-dir`. Connect the Maker account through the host's OAuth UI. The portable `plugin.json` and `mcp.json` serve OpenAI; `.claude-plugin/plugin.json` and `.mcp.json` serve Claude. Both share the same skills and brand assets. The license covers this package, not the hosted Maker service.

The application repository maintains the package and ZIP builder. From that repository's root, run:

```sh
node --test scripts/package-maker-plugin.test.mjs
node scripts/package-maker-plugin.mjs /tmp/maker-plugin.zip
```

The dedicated distribution repository contains the plugin folder unchanged; it does not contain the application or its build scripts. Upload the ZIP through the host's local plugin testing flow.

## Reviewer fixtures and cases

The OpenAI manifest contains five positive and three negative review cases. These are reproducible test instructions, not claims that hosted evaluations have passed. Before recording results, prepare a dedicated reviewer organization with an active manual credit grant, both Maker scopes, and a project named **Plugin review draft** containing a valid React page and `src/main.tsx`. Verify screenshot capture and TypeScript diagnostics with the Maker editor closed. Other cases follow their individual setup; the stale-token case requires an edit in Maker’s editor. Prepare an inaccessible project ID from a second organization for the tenant-isolation case and keep that ID in private review instructions.

Run each case in an installed plugin using that account. Record the tool calls, actual results, and any user approvals. The publishing case makes the sample page public only after approval; the stale-token case changes the draft after obtaining the quote. Run the direct stale-token tool call through an MCP client to verify the server rejection independently of model behavior. The positive and negative expectations are in `extensions.com.openai.review.test_cases` in `plugin.json`.

## Submission handoff

This folder is the distributable package source. Before submission:

- Deploy and test the final server at the configured production endpoint; local package validation does not verify the hosted OAuth flow or tools.
- Choose a dedicated GitHub repository for Claude's plugin source; it may remain private during review and must be public for publication. Keep the application repository private. Add the actual source URL to both manifests once it exists.
- Prepare a populated reviewer account that works without MFA, email/SMS codes, or magic links. Enter its credentials privately in the review portal.
- Run the eight review cases, record a walkthrough, and add its accessible URL as `extensions.com.openai.review.demo_recording_url`. That field is intentionally absent until an actual recording exists. Capture the three to five actual UI screenshots required for Claude's listing.
- Verify the publisher identity and submission permissions: OpenAI business verification with a global-residency project, and a paid Claude seat. The OpenAI portal generates the domain-challenge token; configure that exact token when preparing the submission.
- Check the public support, privacy, and terms URLs and complete the platform's policy attestations from verified product behavior.

Claude requires separate connector and plugin listings. OpenAI lists one package across ChatGPT and Codex. Platform review and publication remain separate from building this ZIP; nothing here submits or publishes a listing.
