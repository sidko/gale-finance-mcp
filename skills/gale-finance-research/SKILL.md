---
name: gale-finance-research
description: Answer questions about the risk and return of stocks, ETFs, crypto and commodities with Gale Finance's published metrics, and cite the Gale page each figure comes from. Use for questions such as "what's the Sortino ratio of gold", "how has gold done vs bitcoin historically", "what are the tail risks for bitcoin", "which crypto had the best Sharpe last year", or "how volatile is NVDA". Not for live prices, price history or portfolio advice.
---

# Gale Finance research

The `gale-finance` MCP server returns computed metrics that Gale Finance has
already published. Each tool response starts with a sentence you can quote and
ends with a `Source:` line naming the Gale page and the date the data runs
through.

## Pick the tool by the question

| The user asks about | Call |
|---|---|
| One asset's return, volatility, Sharpe, Sortino, CAGR or max drawdown | `get_risk_return_metrics`. Pass `metrics` to narrow the answer, for example `["sortino"]` |
| One asset's tail risk, crashes, worst days or recovery times | `get_downside_risk` |
| Two assets against each other | `compare_assets`. Use `horizon: "long_term"` for "historically" or "over the long run", a year such as `"2024"` for that calendar year, and leave it at `"1y"` otherwise |
| "Which asset had the best or worst X" | `rank_assets_by_metric`, with `asset_type` when the user names a class (crypto, stock, ETF) |
| Whether Gale covers an asset, or which comparisons exist | `find_assets` |

Asset inputs accept tickers, names and common aliases ("gold", "BTC",
"Nvidia"). When a tool returns candidates instead of figures, pick the one the
user meant and call again, or ask the user.

## Answer with care

- **Cite.** Quote the figure, then link the `Source:` URL from the response.
- **State the window and date.** Say whether a figure is the 1-year or the long
  window, and give the data-through date. Long windows differ by asset (5 years
  or since inception). Do not present them as like-for-like when the response
  says they are not.
- **Respect missing pair data.** When `compare_assets` says Gale has not
  published the pair, report each asset's own figures and say that pair figures
  such as correlation are not available. Do not invent them.
- **No prices.** The server does not return prices, price history or
  "what if you had invested" values. Say so and suggest a price source. If you
  derive a dollar outcome from a published return yourself, label it as your
  own estimate, not a Gale figure.
- **Not advice.** Gale publishes educational data, not investment advice.
