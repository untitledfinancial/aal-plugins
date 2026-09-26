# Alternative Asset Literacy — Retail MCP

An MCP (Model Context Protocol) connector for **retail investors** — alternative-asset education, delivered directly in Claude. Mirrors the free/paid boundary of the Alternative Asset Literacy iOS app.

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
      "url": "https://alternativeassetliteracy.com/mcp/retail"
    }
  }
}
```

No signup required to start — every tool works immediately with locked previews (titles, key takeaways, free-tier content). For full content, sign up at `https://alternativeassetliteracy.com/mcp/retail/signup` and add the returned key as an `Authorization: Bearer <key>` header.

## What's included — 22 tools

- **9 deep-dive tracks**, each grounded in cited primary/institutional research: DeFi, ESG & Climate, Art, Behavioral Economics, Gender Lens Investing, Gross Domestic Regeneration (an emerging, explicitly-caveated "beyond GDP" model), Venture Capital & Private Equity, Tokenized Real-World Assets, and PE/VC Secondaries & Continuation Funds.
- **Calculators, not templated text**: a regime-switching retirement Monte Carlo estimator (deliberately not built around the "4% rule" — runs multiple forward-looking market scenarios by default) and an illiquidity stress-test / commitment-pacing model (Takahashi-Alexander framework). Free preview on both; full multi-scenario comparison with a subscription.
- **A free, live SEC IAPD / FINRA BrokerCheck registration lookup** — never gated, since it's a protective utility, not premium content.
- **A directory of verified, fee-only, women-focused advisory firms** — durable self-updating advisor-search networks plus individually-verified firms with a genuine, publicly-stated focus, each grounded in a real citation.
- The 351-term glossary (always free, also available as an `aal://glossary` MCP Resource), module previews, a personal risk/holding explainer, the investing brain map, and the app's art learning track and library.
- Two guided Prompts (`explore_alt_asset_class`, `build_my_alt_literacy_plan`) that chain several tools into one call.

## Not investment advice

Every deep-dive track and calculator carries a risk disclosure; content about venture capital, private equity, and PE secondaries additionally carries an accredited-investor disclosure, since those are the only two subjects that are legally Reg D-gated private placements. See `disclosures.get`.

## License

See the repository's [LICENSE](../LICENSE). This repository is a connector/discovery listing, not the plugin's implementation source.
