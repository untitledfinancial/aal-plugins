# Alternative Asset Literacy — Advisor MCP

A per-seat-billed MCP (Model Context Protocol) connector for **financial advisors** — client-education content and practice tools for alternative assets, delivered directly in Claude.

This repository contains only the connector definition (`.mcp.json`), plugin manifest, and workflow skill needed to install and use it. The actual server runs at `alternativeassetliteracy.com`; nothing here executes locally.

## Install

**Via the marketplace:**
```shell
claude plugin marketplace add untitledfinancial/aal-plugins
claude plugin install aal-advisor-mcp@aal-plugins
```

**Manually**, add to your MCP configuration:
```json
{
  "mcpServers": {
    "aal-advisor": {
      "url": "https://alternativeassetliteracy.com/mcp",
      "headers": {
        "Authorization": "Bearer <your seat key>"
      }
    }
  }
}
```

## Get a seat key

1. Register your firm — `POST https://alternativeassetliteracy.com/mcp/admin/signup` (free, no payment for this step).
2. Start billing from the returned admin dashboard link — opens a Stripe Checkout with a 14-day trial.
3. Issue yourself a seat from the same dashboard — this returns your seat key.
4. `tools/list` works without a key so you can see the full catalog before signing up; every `tools/call` requires the `Authorization` header above.

## What's included — 32 tools

- **9 deep-dive tracks**, each grounded in cited primary/institutional research: DeFi, ESG & Climate, Art, Behavioral Economics, Gender Lens Investing (advisor-practice angle), Gross Domestic Regeneration (an emerging, explicitly-caveated "beyond GDP" model), Venture Capital & Private Equity, Tokenized Real-World Assets, and PE/VC Secondaries & Continuation Funds.
- **Calculators, not templated text**: a regime-switching retirement Monte Carlo estimator (deliberately not built around the "4% rule" — runs multiple forward-looking market scenarios by default) and an illiquidity stress-test / commitment-pacing model (Takahashi-Alexander framework).
- **A free, live SEC IAPD / FINRA BrokerCheck registration lookup** — useful for due diligence on a referral partner, not just a client.
- **Client-education generation with a built-in compliance self-check**: `advisor.post_meeting_followup` runs the same pattern-scan `advisor.compliance_scan` exposes manually, automatically, on every generated message.
- The 351-term glossary, institutional research papers, a 53-question competency check, the investing brain map, and the app's art learning track and library — also available as MCP Resources (`aal://glossary`, `aal://disclosures`, `aal://toolkit-frameworks`) for clients that prefer reading over a tool call.
- Two guided Prompts (`build_client_deep_dive`, `pre_meeting_prep`) that chain several tools into one call.

## Compliance posture

This plugin never gives investment advice to an end investor directly and never asserts suitability — content is for the advisor's own practice use. Disclosure language is split by audience: a `regulatory` disclosure meant for the advisor's own records (never forward it to a client — it names this plugin's operator), and a deliberately un-branded `client_facing` disclosure meant to be appended to anything actually sent to a client. See `disclosures.get`.

## License

See the repository's [LICENSE](../LICENSE). This repository is a connector/discovery listing, not the plugin's implementation source.
