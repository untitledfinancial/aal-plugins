# Alternative Asset Literacy — Claude Plugins

A marketplace listing for two MCP (Model Context Protocol) plugins from [Alternative Asset Literacy](https://alternativeassetliteracy.com), bringing alternative-asset investor education — DeFi, ESG, art, behavioral economics, gender-lens investing, venture capital & private equity, tokenized real-world assets, and PE secondaries — directly into Claude, alongside live calculators and a free advisor-registration lookup.

## Plugins

| Plugin | Audience | Billing | |
|---|---|---|---|
| [`aal-retail-mcp`](./aal-retail-mcp) | Retail investors | Free previews; subscription for full content | [README](./aal-retail-mcp/README.md) |
| [`aal-advisor-mcp`](./aal-advisor-mcp) | Financial advisors | Per-seat, 14-day trial | [README](./aal-advisor-mcp/README.md) |

## Install

Add this repository as a marketplace source, then install either plugin by name — see each plugin's own README for manual `.mcp.json` setup if you'd rather not use the marketplace flow.

## What this repository is (and isn't)

This repository is a **connector and discovery listing only** — plugin manifests, MCP connection definitions, and workflow skills. Both plugins are remote MCP servers: all actual tool logic runs on Alternative Asset Literacy's own infrastructure at `alternativeassetliteracy.com`, not on your machine. This repository doesn't contain that implementation, and isn't where feature or content changes are made — it's the thin layer that lets Claude discover and connect to the live service.

## Questions

`hello@alternativeassetliteracy.com`, or see [alternativeassetliteracy.com](https://alternativeassetliteracy.com).
