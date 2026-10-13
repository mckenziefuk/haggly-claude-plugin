# Private review and publication

This directory is the complete plugin repository. Its MCP config points to Haggly's existing public service. Publishing or installing these files does not deploy a new server.

## Create the private review repository

If it has not already been created, run these commands from this directory after reviewing the files:

```sh
git init -b main
git add .
git commit -m "Prepare Haggly Claude plugin for review"
gh repo create haggly-claude-plugin --private --source=. --remote=origin --push
```

The `gh` command uses the currently signed-in GitHub account. If a different organization should own the repo, use its full `OWNER/haggly-claude-plugin` name. Keep the repo private while reviewing it. Confirm its visibility in GitHub's repository settings.

After creation, add the actual GitHub repository URL to `repository` in `.claude-plugin/plugin.json`, and replace `OWNER` in the README's installation example. The manifest deliberately omits a repository URL until there is a real destination.

## Review the package

The proposed package license is MIT. Confirm that license, the Haggly publisher details and contact email, and the permanent plugin and marketplace names before public release.

Run `claude plugin validate --strict .`. Try `claude --plugin-dir .` and check `/mcp`, then exercise representative requests:

- Find web-search options under $0.01 per call. Check that recommendations identify unit prices and any additional fees.
- Compare weather options by cheapest price. Check that the skill uses the HTTP category route because MCP has no explicit sort parameter.
- Check an exact tracked endpoint URL. Check that warnings and dated probe evidence appear in the explanation.
- Ask for a task outside the catalogue. Check that category suggestions or coverage limitations are reported without invented providers.

These are manual review examples, not automated evals. Install the plugin from the private GitHub marketplace once to verify the repository packaging as well as the local folder. Installing it creates Claude plugin settings on the reviewer's machine.

## Publish later

When the review is complete, make the repository public through GitHub's settings. That exposes the plugin files and makes the custom `haggly-plugins` marketplace available to anyone who can clone it; it does not submit the plugin to Anthropic automatically.

For a Claude directory listing, use [the developer portal](https://claude.ai/directory/manage), choose **Plugin bundle**, and select this repository and its root folder. Follow the portal's validation and review process. A directory listing requires a public repository before it goes live and an eligible paid Claude account.

Submit the remote Haggly MCP server separately as an **MCP connector** from the same organization. Use the exact URL `https://hagglyai.com/mcp` in both submissions. This package already includes that URL. Follow the portal's pairing steps when both listings are available.

Official references:

- [Publish a Claude plugin](https://code.claude.com/docs/en/plugins/publish)
- [Plugin pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist)
- [Publish to the Claude directory](https://claude.com/docs/directory/publish)

## Updates

Keep `haggly` as the permanent plugin name and `haggly-plugins` as the marketplace name. Increment `version` in `plugin.json` for each release. Update the skill and HTTP reference when the Haggly tool contract or pricing semantics change; the live OpenAPI schema remains on the service.

To update an installed marketplace copy:

```sh
claude plugin update haggly@haggly-plugins
```
