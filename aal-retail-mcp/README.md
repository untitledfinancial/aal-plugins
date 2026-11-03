# Alternative Asset Literacy — Retail MCP

An MCP (Model Context Protocol) connector for **retail investors** — alternative-asset education, delivered directly in Claude. Free previews are available without an account; full content needs a subscription.

This repository contains only the connector definition (`.mcp.json`), plugin manifest, and workflow skill needed to install and use it. The actual server runs at `alternativeassetliteracy.com`; nothing here executes locally.

## Install

**Via the marketplace:**
```shell
claude plugin marketplace add untitledfinancial/aal-plugins
claude plugin install aal-retail-mcp@aal-plugins
```

**Manually**, add to your MCP configuration:
```json
{
  "mcpServers": {
    "aal-retail": {
      "type": "http",
      "url": "https://alternativeassetliteracy.com/mcp/retail",
      "headers": {
        "Authorization": "Bearer <optional subscription key>"
      }
    }
  }
}
```

No signup required to start — every tool works immediately with locked previews (titles, key takeaways, free-tier content). For full content, sign up at `https://alternativeassetliteracy.com/mcp/retail/signup` and enter the returned key when Claude Code asks for the optional subscription key at install time (for a manual setup, add it as the `Authorization` header shown above). Leaving it empty keeps the free previews.

## What's included — 22 tools

- **9 deep-dive tracks**, each grounded in cited primary/institutional research: DeFi, ESG & Climate, Art, Behavioral Economics, Gender Lens Investing, Gross Domestic Regeneration (an emerging, explicitly-caveated "beyond GDP" model), Venture Capital & Private Equity, Tokenized Real-World Assets, and PE/VC Secondaries & Continuation Funds.
- **Calculators, not templated text**: a regime-switching retirement Monte Carlo estimator (deliberately not built around the "4% rule" — runs multiple forward-looking market scenarios by default) and an illiquidity stress-test / commitment-pacing model (Takahashi-Alexander framework). Free preview on both; full multi-scenario comparison with a subscription.
- **A live SEC IAPD / FINRA BrokerCheck registration lookup** that works without a subscription, since it's a protective utility rather than premium content.
- **A directory of verified, fee-only, women-focused advisory firms** — durable self-updating advisor-search networks plus individually-verified firms with a genuine, publicly-stated focus, each grounded in a real citation.
- The 351-term glossary (available without a subscription, and as an `aal://glossary` MCP Resource), module previews, a personal risk/holding explainer, the investing brain map, and the app's art learning track and library.
- Two guided Prompts (`explore_alt_asset_class`, `build_my_alt_literacy_plan`) that chain several tools into one call.

## Not investment advice

Every deep-dive track and calculator carries a risk disclosure; content about venture capital, private equity, and PE secondaries additionally carries an accredited-investor disclosure, since those are the only two subjects that are legally Reg D-gated private placements. See `disclosures.get`.

## Data and network use

Everything runs on a remote server; nothing executes on your machine. Each tool call sends the tool name and its arguments (for example a glossary term, a module ID, or calculator inputs you type) over HTTPS to `alternativeassetliteracy.com`, operated by Untitled_ LuxPerpetua Technologies, Inc. If you add a subscription key, it is sent as a Bearer token with each request so the server can recognise your subscription. The registration lookup tool additionally sends the name or CRD number you enter to the public SEC IAPD and FINRA BrokerCheck services to return the result. No other third-party services receive your input. The plugin does not read local files, run shell commands, or access other MCP servers. See the [privacy policy](https://alternativeassetliteracy.com/privacy) and [terms](https://alternativeassetliteracy.com/terms).

## License

See [LICENSE](LICENSE). This repository is a connector/discovery listing, not the plugin's implementation source.
