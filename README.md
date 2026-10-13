# Haggly for Claude

Compare API providers for your agent's next task using Haggly's listed prices, recent availability, latency and usage evidence. Haggly covers x402 pay-per-call APIs, Apify actors and MCP alternatives, with a comparison skill and a connection to its hosted MCP server.

Querying Haggly is free and requires no account or API key. The plugin helps you choose a provider; provider subscriptions, paid requests and payments are separate.

## Try the local plugin

From this repository's root, start Claude Code with:

```sh
claude --plugin-dir .
```

Then ask Claude:

```text
/haggly:compare Find web-search providers under $0.01 per call and explain the tradeoffs.
```

The skill can also activate when a request matches API-provider comparison. In Claude Code, `/mcp` shows whether the Haggly connection is available. If you already configured Haggly manually, use one connection when reviewing the plugin to avoid duplicate tools.

## Install from the private review repo

Install from the private GitHub repository:

```sh
claude plugin marketplace add mckenziefuk/haggly-claude-plugin
claude plugin install haggly@haggly-plugins
```

Only users with access to the private repository can install from it. GitHub authentication must already be available to their git client. The marketplace name is `haggly-plugins`; the plugin name is `haggly`.

Local validation uses Claude Code's plugin and marketplace validator:

```sh
claude plugin validate --strict .
```

Use Claude Code 2.1.281 or newer to validate the directory listing fields and remote MCP configuration. See [docs/RELEASING.md](docs/RELEASING.md) for private-repo setup and later publication.

## What the plugin adds

- `/haggly:compare`: a skill for discovering, comparing and checking providers, including per-call budgets, unit-price differences, extra fees and evidence limitations.
- `haggly` MCP connection: `https://hagglyai.com/mcp`, using Streamable HTTP.
- An HTTP reference for JSON and markdown access when explicit sorting is needed or MCP is unavailable.

The hosted server exposes four read-only tools:

| Tool | Use |
| --- | --- |
| `find_best` | Find candidates for a plain-language task, optionally within a per-call budget |
| `compare_category` | Return a category's comparison using Haggly's best-value ordering |
| `check_endpoint` | Read stored price, warnings and recent checks for an exact endpoint URL |
| `list_categories` | List supported category IDs and their counts and prices |

Haggly's MCP tools return markdown. The JSON API additionally supports explicit `cheapest`, `most_reliable` and `fastest` sorts. The [live OpenAPI schema](https://hagglyai.com/openapi.json) is the maintained contract; this repository does not duplicate the service implementation or a snapshot of the catalogue.

## Reading comparisons

Haggly's comparisons distinguish the amount charged per call from the price per category unit. A call covering several results may have a low unit price but a larger per-call charge. Apify actors can have a start fee and additional platform usage costs.

Haggly's unpaid checks inspect availability and payment-challenge metadata. They do not establish the correctness of a provider's paid answer. Apify reliability is reported by Apify; MCP alternatives have unpublished pricing and are not scored. The skill keeps these limits visible when recommending a provider.

## Data handling

The plugin connects to Haggly's public service. Task text, endpoint URLs and query filters supplied to its tools are sent to `hagglyai.com`; use only the information needed to find or compare providers. The connection sends a descriptive plugin User-Agent and has no authorization header.

Haggly handles service requests under its [Privacy Policy](https://hagglyai.com/privacy) and [Terms of Service](https://hagglyai.com/terms). The package has no hooks, background jobs, local server, dependencies or installation scripts. Claude may use an available HTTP tool for the skill's documented fallback requests.

## Links and support

- [Haggly agent guide](https://hagglyai.com/for-agents)
- [Machine-readable guide](https://hagglyai.com/llms.txt)
- [Methodology](https://hagglyai.com/how-we-test)
- Support: [hello@hagglyai.com](mailto:hello@hagglyai.com)

## License

The plugin files are provided under the [MIT License](LICENSE). This license does not grant access rights to listed providers or a license to Haggly's service data. Haggly's name and logo identify the service and are not a grant of trademark rights.
