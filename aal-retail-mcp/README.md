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

## What's included — 23 tools

- **Deep-dive tracks** grounded in cited primary and institutional research: Venture Capital & Private Equity, PE/VC Secondaries & Continuation Funds, Tokenized Real-World Assets, Gender Lens Investing, and Gross Domestic Regeneration (an emerging, explicitly caveated "beyond GDP" model). Locked to lesson titles and key takeaways without a subscription.
- **Two calculators**: a retirement Monte Carlo estimator (not built around the "4% rule"; it reports several named forward-looking scenarios) and an illiquidity stress-test / commitment-pacing model (Takahashi-Alexander framework). A free preview of each runs one scenario; the full multi-scenario comparison needs a subscription.
- **A registration lookup** (`advisors.registration_check`) against the public SEC IAPD and FINRA BrokerCheck records. It needs no account.
- **A directory of advisor-search networks and fee-only firms** that publicly state a focus on women clients, each with the public statement it is based on.
- **The 351-term glossary** (no account needed; also an `aal://glossary` MCP Resource), nine learning modules, a curated reading list, sourced facts, the investing brain map, the art learning track and art library, a holding explainer, and legal disclosures.
- **`account.sign_in`**: starts an OAuth sign-in with a retail access key and reports whether the account has an active subscription.
- Two guided Prompts (`explore_alt_asset_class`, `build_my_alt_literacy_plan`).

### Signing in

Free tools and previews need no account. To unlock full content, create an account at `https://alternativeassetliteracy.com/mcp/retail/signup`, then either enter the access key when Claude Code asks for the optional subscription key at install time, or call `account.sign_in` and enter the key when prompted. If you created an account but have not paid, finish at `https://alternativeassetliteracy.com/mcp/retail/billing/start`.

## Not investment advice

Every deep-dive track and calculator carries a risk disclosure; content about venture capital, private equity, and PE secondaries additionally carries an accredited-investor disclosure, since those are the only two subjects that are legally Reg D-gated private placements. See `disclosures.get`.

## Data and network use

Everything runs on a remote server; nothing executes on your machine. Each tool call sends the tool name and its arguments (for example a glossary term, a module ID, or calculator inputs you type) over HTTPS to `alternativeassetliteracy.com`, operated by Untitled_ LuxPerpetua Technologies, Inc. If you add a subscription key, it is sent as a Bearer token with each request so the server can recognise your subscription. The service stores your email address, access key, subscription status and Stripe customer and subscription identifiers; payments are handled by Stripe. It does not store tool arguments or conversation content and keeps no usage log. The registration lookup tool additionally sends the name or CRD number you enter to the public SEC IAPD and FINRA BrokerCheck services to return the result. No other third-party services receive your input. The plugin does not read local files, run shell commands, or access other MCP servers. See the [privacy policy](https://alternativeassetliteracy.com/privacy) and [terms](https://alternativeassetliteracy.com/terms).

## License

See [LICENSE](LICENSE). This repository is a connector/discovery listing, not the plugin's implementation source.
