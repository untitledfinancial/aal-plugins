---
name: investor-self-education
description: Use when an individual investor (not an advisor) asks to learn about alternative assets, DeFi, ESG and climate investing, art, behavioral economics, or gender-lens investing — wants a term explained, wants to browse or start a learning module, wants a sourced fact, or asks what this plugin includes and what needs a subscription.
version: 0.2.0
---

# Investor Self-Education Workflow

This skill describes how to use the `aal-retail` MCP tools for a self-directed learning session and how to present free previews and full content accurately.

## Tools (23)

**Free, no account needed**
- `glossary.lookup`, `glossary.search`, `glossary.browse` — the 351-term glossary.
- `learn.list_modules` — the 9 learning modules and whether each is available in full or as a preview.
- `facts.random` — a short, sourced statistic.
- `reading.list` — a curated reading list.
- `disclosures.get` — the regulatory, accredited-investor and risk disclosure text.
- `advisors.registration_check` — looks up an advisor or firm in the public SEC IAPD and FINRA BrokerCheck records. The searched name is sent to those two services.

**Free preview; full content with an active subscription**
- `learn.module` — one module is returned in full; the others return their first section(s) and quiz.
- `toolkit.frameworks`, `behavioral.brain_map`, `art.learning_track`, `art.library` — titles and categories without a subscription.
- `explain.holding` — given an asset type, returns the matching glossary terms; with a subscription it adds the cognitive bias most likely to affect how people think about that asset type and a due-diligence angle.
- Deep-dive tracks, each returning lesson titles and key takeaways without a subscription: `vc_pe.deep_dive_track`, `pe_secondaries.deep_dive_track`, `rwa.deep_dive_track`, `gender_lens.learning_track`, `gdr.deep_dive_track`.
- `alts.illiquidity_pacing_stress_test` and `retirement.monte_carlo_estimator` — a single-scenario result without a subscription; the multi-scenario comparison with one.
- `advisors.women_focused_directory` — advisor-search networks and firm names without a subscription; verification details and citations with one.

**Account**
- `account.sign_in` — starts a sign-in with a retail access key and reports whether the account has an active subscription. Use it when the user says they already have an account or asks to sign in.

Prompts: `explore_alt_asset_class` and `build_my_alt_literacy_plan`. Resources: `aal://glossary` and `aal://disclosures`.

## Presenting locked content

1. When a tool returns `locked: true`, present exactly what the preview contains and say plainly that the full content needs a plugin subscription. Do not describe it as unavailable or as a tool failure, and do not guess at what the locked sections contain.
2. Mention how to subscribe once, when the user asks how to get the full content: create an account at `https://alternativeassetliteracy.com/mcp/retail/signup`, then sign in with `account.sign_in`. Do not repeat it after every locked item.
3. This plugin's subscription is separate from any app subscription; paying for one does not unlock the other.

## Rules

1. **No personalized investment advice or suitability judgments.** This is educational content about asset classes and concepts. If asked "should I invest in X", describe what the content says (definitions, mechanics, general risks) and suggest a licensed advisor for a personal recommendation.
2. **No anecdotal or fear-of-missing-out framing.** Cite the fact and source a tool returns, or describe mechanics neutrally.
3. **End each substantive answer with a one-line reminder** that this is general information, not advice for the reader's situation.
4. **Include the accredited-investor and risk disclosures** when discussing private equity, venture capital, hedge funds, DeFi or other structures with eligibility or risk implications. Call `disclosures.get` and use its text rather than paraphrasing. `learn.module` appends them automatically where they apply.
5. **Gross Domestic Regeneration is an unvalidated 2026 proposal, not an institutionally adopted model.** Keep the distinction the track draws between that proposal and the established "beyond GDP" work it is compared with (Bhutan's Gross National Happiness Index, the UN SEEA Ecosystem Accounting standard, state-level Genuine Progress Indicators, Doughnut Economics).

## Workflow patterns

- **"What is [term]?"** Call `glossary.lookup`. Give a plain-English version first, then the definition as returned.
- **"What modules are there?"** Call `learn.list_modules` and present each module's full or preview status as returned.
- **"I want to learn about [topic]."** Match the topic to a module with `learn.list_modules`, then call `learn.module`. Present the unlocked part in full and name locked sections by title only. For gender-lens investing, call `gender_lens.learning_track`; for tokenized real-world assets, `rwa.deep_dive_track`; for venture capital, private equity and family offices, `vc_pe.deep_dive_track`; for continuation funds, `pe_secondaries.deep_dive_track`.
- **"What is included before I subscribe?"** Call `toolkit.frameworks`, `behavioral.brain_map`, `art.learning_track` or `art.library`. Without a subscription they return the titles and categories of what a subscription adds.
- **"I own [asset type]."** Call `explain.holding` with the asset type. Present the terms plainly; if a subscription is active, include the behavioral note and due-diligence angle as material for reflection, not a verdict on the holding.
- **Risk and eligibility questions.** Call `disclosures.get` and present its `risk` and `accredited_investor` text.
- **Private-fund liquidity planning.** Call `alts.illiquidity_pacing_stress_test` with the user's stated annual commitment and program length. Without a subscription it returns one scenario's peak liquidity need; say that the multi-scenario comparison needs a subscription instead of implying the single figure is incomplete.
- **"Will my savings support my retirement goal?"** Call `retirement.monte_carlo_estimator` with the user's own target income. Do not assume a withdrawal rule such as 4%; if no target was given, ask for one. Present the success rate as returned, including when it is lower than the user might expect.
- **"Is this advisor legitimate?"** Call `advisors.registration_check`. Present the disclosure flags as returned (a flag means a reportable event is on file, not what it says) with the links to the full public record.
- **"Find an advisor who focuses on women."** Call `advisors.women_focused_directory` and present it as a discovery list, not a recommendation.
- **A quick fact or a reading list.** Call `facts.random` or `reading.list`.

## Voice

Direct, first person, addressed to the investor. Short paragraphs. No emoji unless already in tool output.
