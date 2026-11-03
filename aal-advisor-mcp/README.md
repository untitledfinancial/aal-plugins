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
      "type": "http",
      "url": "https://alternativeassetliteracy.com/mcp",
      "headers": {
        "Authorization": "Bearer <optional seat key>"
      }
    }
  }
}
```

## Get a seat key

1. Register your firm — `POST https://alternativeassetliteracy.com/mcp/admin/signup` (free, no payment for this step).
2. Start billing from the returned admin dashboard link — opens a Stripe Checkout with a 14-day trial.
3. Issue yourself a seat from the same dashboard — this returns your seat key.
4. `tools/list` works without a key so you can see the full catalog before signing up. Most `tools/call` requires the seat key — enter it when Claude Code asks for the optional seat key at install time, or add the `Authorization` header above for a manual setup — a small set of onboarding tools work without it (see below).

## What's included — 32 tools

- **9 deep-dive tracks**, each grounded in cited primary/institutional research: DeFi, ESG & Climate, Art, Behavioral Economics, Gender Lens Investing (advisor-practice angle), Gross Domestic Regeneration (an emerging, explicitly-caveated "beyond GDP" model), Venture Capital & Private Equity, Tokenized Real-World Assets, and PE/VC Secondaries & Continuation Funds. Requires an active seat.
- **Calculators, not templated text**: a regime-switching retirement Monte Carlo estimator (deliberately not built around the "4% rule" — runs multiple forward-looking market scenarios by default) and an illiquidity stress-test / commitment-pacing model (Takahashi-Alexander framework). Requires an active seat.
- **Client-education generation with a built-in compliance self-check**: `advisor.post_meeting_followup` runs the same pattern-scan `advisor.compliance_scan` exposes manually, automatically, on every generated message. Requires an active seat.
- Institutional research papers, a 53-question competency check, the investing brain map, and the app's art learning track and library. Requires an active seat.
- Two guided Prompts (`build_client_deep_dive`, `pre_meeting_prep`) that chain several tools into one call. Requires an active seat.

### No seat required

A small set of onboarding tools work without a key, so a prospective firm can try the plugin before signing up:

- The 351-term glossary and curated reading list (`glossary.lookup`/`search`/`browse`, `reading.list`) — also available as MCP Resources (`aal://glossary`, `aal://disclosures`) for clients that prefer reading over a tool call.
- A live SEC IAPD / FINRA BrokerCheck registration lookup (`advisor.registration_check`) — useful for due diligence on a referral partner, not just a client.
- The Investing Primer module in full, and a locked preview (first section, first quiz) of the other 8 (`modules.get_content`) .
- A sourced statistic (`facts.random`), a ready-to-use meeting opener (`advisor.meeting_icebreaker`), and a client gift bundle (`advisor.gift_bundle`).
- Legal disclosure text (`disclosures.get`).

## Compliance posture

This plugin never gives investment advice to an end investor directly and never asserts suitability — content is for the advisor's own practice use. Disclosure language is split by audience: a `regulatory` disclosure meant for the advisor's own records (never forward it to a client — it names this plugin's operator), and a deliberately un-branded `client_facing` disclosure meant to be appended to anything actually sent to a client. See `disclosures.get`.

## Data and network use

Everything runs on a remote server; nothing executes on your machine. Each tool call sends the tool name and its arguments (for example a glossary term, a module ID, or calculator inputs you type) over HTTPS to `alternativeassetliteracy.com`, operated by Untitled_ LuxPerpetua Technologies, Inc. If you add a seat key, it is sent as a Bearer token with each request so the server can check your firm's seat and log usage against it. The registration lookup tool additionally sends the name or CRD number you enter to the public SEC IAPD and FINRA BrokerCheck services to return the result. No other third-party services receive your input. The plugin does not read local files, run shell commands, or access other MCP servers. See the [privacy policy](https://alternativeassetliteracy.com/privacy) and [terms](https://alternativeassetliteracy.com/terms).

## License

See [LICENSE](LICENSE). This repository is a connector/discovery listing, not the plugin's implementation source.
