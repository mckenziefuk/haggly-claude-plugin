# Release validation and publication

This directory is the complete plugin repository. Its MCP config points to Haggly's existing public service. Publishing or installing these files does not deploy a new server.

## Get the source

Clone the plugin repository to prepare a release:

```sh
git clone https://github.com/mckenziefuk/haggly-claude-plugin.git
cd haggly-claude-plugin
```

Keep the repository URL in `.claude-plugin/plugin.json` and the README's installation command aligned with the repository that distributes the plugin.

## Review the package

The package license is MIT. Check the Haggly publisher details, contact email and permanent plugin and marketplace names before a release.

Run `claude plugin validate --strict .`. Try `claude --plugin-dir .` and check `/mcp`, then exercise representative requests:

- Find web-search options under $0.01 per call. Check that recommendations identify unit prices and any additional fees.
- Compare weather options by cheapest price. Check that the skill uses the HTTP category route because MCP has no explicit sort parameter.
- Check an exact tracked endpoint URL. Check that warnings and dated probe evidence appear in the explanation.
- Ask for a task outside the catalogue. Check that category suggestions or coverage limitations are reported without invented providers.
- Discover a category's platform and task IDs with `list_categories`. Check that only live IDs are used and that discovery counts are described as category-wide.
- Compare Instagram or TikTok comments with `platform: ["instagram", "tiktok"]`, `task: ["comments"]` and `rail: ["x402"]`, after confirming those IDs are available. Check that alternatives within a facet use OR, platform and task restrictions use AND, and paid rows use the requested rail. Separately listed MCP alternatives remain category-wide.
- Repeat a filtered category comparison over HTTP. Check that JSON uses comma-separated IDs, category pages use repeated parameters, and returned `selected` fields and row rails confirm the restriction.
- Ask `find_best` for a specific platform and task with no matches. Check that any automatic widening is disclosed and broader results are not presented as satisfying the original restrictions.
- Try an unknown sub-category ID or a server exposing the old tool schema. Check that unsupported filtering is reported; ignored parameters must not be treated as successful filtering.

These are manual review examples, not automated evals. Install the plugin from the GitHub marketplace once to verify the repository packaging as well as the local folder. Installing it creates Claude plugin settings on the reviewer's machine.

## Version 0.2.0 backend requirements

The platform, task and rail contract matches the Codex plugin's v0.2.0 guidance. It was checked against the Haggly feature worktree `worktree-more-sources` at commit `6230538`, including `src/mcp/server.ts`, `src/api/routes.ts` and `src/catalog/subcategories.ts`. Updating this plugin does not merge or deploy that backend feature.

Before exercising filters, confirm the connected server advertises `platform`, `task` and `rail` in `compare_category`. For HTTP comparisons, confirm the live OpenAPI parameters and returned `selected` fields. An older service can ignore unknown query parameters and return an unrestricted comparison. The skill explicitly checks support and applied filters.

## Distribution and directory submission

The GitHub repository distributes the custom `haggly-plugins` marketplace. Users add it with the README's installation command. Publishing the repository does not submit the plugin to Anthropic automatically.

For a Claude directory listing, use [the developer portal](https://claude.ai/directory/manage), choose **Plugin bundle**, and select this repository and its root folder. Follow the portal's validation and review process. A directory listing requires a public repository before it goes live and an eligible paid Claude account.

Submit the remote Haggly MCP server separately as an **MCP connector** from the same organization. Use the exact URL `https://hagglyai.com/mcp` in both submissions. This package already includes that URL. Follow the portal's pairing steps when both listings are available.

Official references:

- [Publish a Claude plugin](https://code.claude.com/docs/en/plugins/publish)
- [Plugin pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist)
- [Publish to the Claude directory](https://claude.com/docs/directory/publish)

## Updates

Keep `haggly` as the permanent plugin name and `haggly-plugins` as the marketplace name. Increment `version` in `.claude-plugin/plugin.json` for each release. Update the skill and HTTP reference when the Haggly tool contract or pricing semantics change; the live OpenAPI schema remains on the service.

To update an installed marketplace copy:

```sh
claude plugin update haggly@haggly-plugins
```
