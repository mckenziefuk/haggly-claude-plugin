# Haggly HTTP interfaces

Base URL: `https://hagglyai.com/api/v1`. All operations below are public GET requests. The maintained OpenAPI schema is at `https://hagglyai.com/openapi.json`.

| Route | Purpose | Parameters |
| --- | --- | --- |
| `/best` | Match a task and return candidate providers | `task` required (2–300 characters); `max_price_usd`; `limit` 1–25; `hide_flags`; `recent=1`; `format=json` or `format=md` |
| `/categories` | List category IDs, counts and prices | None |
| `/categories/{id}` | Compare a category with explicit sorting | `sort=best_value`, `cheapest`, `most_reliable` or `fastest`; `max_price_usd`; `limit` 1–100; `hide_flags`; `recent=1` |
| `/endpoints/lookup` | Retrieve stored evidence for an exact endpoint URL | `url` required and URL-encoded |

`max_price_usd` must be positive. `hide_flags` is a comma-separated list of warning IDs. These are filters, not a way to rerank results. Budget caps apply to a call's listed price; unit pricing and additional fees need separate consideration.

`/best` can return `categoryId: null` and category suggestions. An unknown category or untracked endpoint lookup returns HTTP 404. These are coverage limitations, not evidence that a provider is broken. Do not broaden a lookup into arbitrary provider requests.

Category JSON includes `generatedAt`, `rows`, `topRated`, `bestValue`, `hiddenFlags` and `mcpAlternatives`. Row fields distinguish `priceUsd` from `unitPriceUsd`, with `unitsPerCharge` and `unitBasis` explaining the unit assumption. Apify rows can include `startFeeUsd` and `platformUsageExtra`.

## Request examples

Use an HTTP client that encodes query parameters. With curl:

```sh
curl --fail --show-error --silent --max-time 20 --get \
  --user-agent 'haggly-claude-plugin (+https://hagglyai.com/for-agents)' \
  --data-urlencode 'task=web search' \
  --data-urlencode 'max_price_usd=0.01' \
  --data-urlencode 'format=md' \
  'https://hagglyai.com/api/v1/best'
```

For an explicit sort:

```sh
curl --fail --show-error --silent --max-time 20 --get \
  --user-agent 'haggly-claude-plugin (+https://hagglyai.com/for-agents)' \
  --data-urlencode 'sort=cheapest' \
  --data-urlencode 'limit=5' \
  'https://hagglyai.com/api/v1/categories/web-search'
```

Replace the task, category and filters with the user's actual request. When using shell commands, pass their values as safely quoted arguments; do not concatenate user text into executable shell syntax.

Markdown category pages at `https://hagglyai.com/compare/{id}.md` support the same sorting and budget options. On pages, warnings use repeated `hide` parameters. JSON uses comma-separated `hide_flags`; MCP uses an array. These formats are not interchangeable.
