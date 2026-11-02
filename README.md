<p align="center">
  <a href="https://www.gale.finance/tools/mcp/"><img src="docs/assets/gale-finance-logo.png" alt="Gale Finance" width="420"></a>
</p>

# Gale Finance MCP Server

[![MCP](https://img.shields.io/badge/MCP-streamable%20HTTP-1f6feb)](https://www.gale.finance/tools/mcp/)
[![Auth](https://img.shields.io/badge/auth-none-2ea44f)](#connect)
[![Read only](https://img.shields.io/badge/tools-read--only-2ea44f)](#tools)
[![Docs license: CC BY 4.0](https://img.shields.io/badge/docs-CC%20BY%204.0-lightgrey)](LICENSE)

Risk and return metrics for stocks, ETFs, crypto and commodities, from
[Gale Finance](https://www.gale.finance/), for AI assistants.

Ask your assistant things like "What's the Sortino ratio of gold?" or "How has
gold performed against bitcoin over the long run?" With this server connected,
it answers with the figures Gale publishes and links the Gale page each number
comes from.

```
https://www.gale.finance/mcp
```

The server is free and read-only, and needs no account or API key. It returns
computed metrics, such as Sharpe and Sortino ratios, volatility, drawdowns,
tail risk, correlation and calendar-year returns. It does not return price
quotes or price history.

This repository holds the documentation and the Claude Code plugin. The server
runs on gale.finance; its source is not published here.

## Connect

| Client | How |
|---|---|
| **Claude** (claude.ai, Desktop) | Settings → Connectors → **Add custom connector**. Name: `Gale Finance`, URL: `https://www.gale.finance/mcp`, no authentication |
| **Claude Code** | `claude mcp add --transport http gale-finance https://www.gale.finance/mcp`, or install the plugin below |
| **Cursor** | [One-click install](https://www.gale.finance/tools/mcp/#cursor), or add `{"mcpServers":{"gale-finance":{"url":"https://www.gale.finance/mcp"}}}` to `~/.cursor/mcp.json` |
| **VS Code** | [One-click install](https://www.gale.finance/tools/mcp/#vscode), or add a server with type `http` and the URL above |
| **Other clients** | Any client that supports remote MCP servers over streamable HTTP: use the URL above with no authentication |

### Claude Code plugin

The plugin adds the server and a short skill that tells Claude when to use
each tool.

```
/plugin marketplace add sidko/gale-finance-mcp
/plugin install gale-finance@gale-finance
```

## Tools

| Tool | Answers questions like | Returns |
|---|---|---|
| `get_risk_return_metrics` | "What's the Sortino ratio of gold?" | Return, CAGR, volatility, Sharpe, Sortino and max drawdown for the 1-year window and the asset's long window (5 years or since inception) |
| `get_downside_risk` | "What are the tail risks for bitcoin?" | Daily 5% VaR, expected shortfall, skew, excess kurtosis, the largest drawdowns with recovery dates, and best and worst days |
| `compare_assets` | "How has gold performed vs bitcoin historically?" | The 1-year comparison, a calendar-year comparison, or the long-term scorecard with year-by-year returns. For pairs Gale has not published, it gives each asset's own metrics side by side |
| `rank_assets_by_metric` | "Which crypto had the best Sharpe last year?" | Up to 25 assets ranked by one metric, with an optional asset-type filter |
| `find_assets` | "Do you cover Hyperliquid?" | Matching assets, with the comparisons and scorecards Gale publishes for each |

Every tool is read-only. Each answer starts with a sentence you can quote and
ends with a source line naming the Gale page and the date the data runs through.

## Example questions

| Question | Tool used |
|---|---|
| What's the Sortino ratio of gold? | `get_risk_return_metrics` |
| How has gold performed vs bitcoin historically? | `compare_assets` |
| What are the tail risks for bitcoin? | `get_downside_risk` |
| Did Bitcoin beat the S&P 500 in 2024? | `compare_assets` |
| Which crypto had the best Sharpe ratio over the last year? | `rank_assets_by_metric` |
| Compare Nvidia and Costco on risk over the past year. | `compare_assets` |

## Data and methodology

- **Coverage:** about 200 assets across stocks, ETFs, crypto and
  commodities, with several hundred published pair comparisons and long-term
  scorecards.
- **Freshness:** the server serves the same figures as the published site and
  updates with each site release. Each answer states its data-through date.
- **Method:** crypto is annualized over 365 days and stocks and ETFs over 252.
  Risk-free rates come from US Treasury bill yields (FRED). Single-asset
  metrics use each asset's full window. See the
  [methodology](https://www.gale.finance/methodology/) and the
  [glossary](https://www.gale.finance/glossary/).
- **Not included:** prices, price history, and "what if you had invested"
  calculations.

Gale Finance publishes educational data, not investment advice.

## Privacy

The server logs one line per request: the tool, the Gale page it resolved to,
the length of any search text (never the text itself), the result count, the
timing and the User-Agent. It does not log your conversation, and it needs no
account. See the [privacy policy](https://www.gale.finance/privacy/) and the
[terms](https://www.gale.finance/terms/).

## Support

Email [hello@gale.finance](mailto:hello@gale.finance) or
[open an issue](https://github.com/sidko/gale-finance-mcp/issues). Include the
client you use and the question you asked.

## License

The documentation in this repository is licensed under
[CC BY 4.0](LICENSE). Gale Finance data returned by the server is covered by
the [Gale Finance terms](https://www.gale.finance/terms/).
