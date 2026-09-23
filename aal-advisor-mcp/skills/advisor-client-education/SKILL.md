---
name: advisor-client-education
description: Use when a financial advisor asks for help preparing for a client conversation about alternative assets (art, DeFi, ESG/climate, behavioral economics, alternative investing generally), wants a quick meeting opener or icebreaker, wants a risk breakdown across asset categories for a client with a stated risk tolerance, wants something to send a client before or after a meeting, or asks how to explain a specific term or concept to a client. Trigger phrases include "help me prep for a client meeting about...", "how do I explain X to a client", "my client is asking about...", "give me a risk breakdown for...", "meeting icebreaker", "something to send my client", "gift bundle for a client".
version: 0.1.0
---

# Advisor Client Education Workflow

This skill turns the `aal-advisor` MCP tools into an actual advisor workflow: which tool(s) to call for a given request, in what order, and how to format the result. The tools themselves are dumb data — this skill is what makes them useful in a real conversation.

## The tools available (via the `aal-advisor` MCP connector) — 32 total, all purpose-built for advisor use

- `modules.get_content` — full content for a module, respecting the app's free/paid boundary (one free module in full; every other module capped at its first section(s)/quiz, remainder listed by title only). Accepts a module id OR a title/topic string directly — there is no separate routing tool.
- `glossary.lookup` / `glossary.search` / `glossary.browse` — the 351-term glossary
- `research.papers` — institutional research by category (CBDC, ESG, gender lens, etc.) — the primary "further research" destination
- `facts.random` — a short, sourced statistic, optionally filtered by category
- `advisor.meeting_icebreaker` — a fact + related term + suggested opening line, combined; a complete deliverable on its own
- `advisor.daily_prep` — batch meeting prep: given a list of the day's meetings, returns a compact briefing (topic, key terms, opener fact, risk-alignment snapshot) per meeting in one call
- `advisor.risk_conversation_guide` — general risk characteristics of each alt-asset category, checked against a stated client risk tolerance
- `advisor.gift_bundle` — a curated 3-module learning path + key terms + suggested client message, matched to a client description
- `advisor.competency_check` — the app's 53-question "Questions for Your Financial Advisor" set across 7 categories (DeFi, ESG, alternative investing, art, behavioral, gender lens, general), each with inadequate vs. competent answer signals and why it matters — for advisor self-prep against exactly the pushback a sophisticated client might raise
- `toolkit.frameworks` — due-diligence/risk/tax/legal frameworks spanning every asset class
- `behavioral.brain_map` — the neuroscience of investing decisions (4 brain regions), for explaining *why* a bias happens
- `art.learning_track` — 9 lessons across 3 art-specific tracks, distinct from the main Art module
- `art.library` — curated books/podcasts/papers on art markets and history
- `reading.list` — 5 curated books on behavioral economics, VC, economic history, and policy
- `disclosures.get` — two distinct disclosure tracks (2026-09-21): `regulatory` (for the advisor's own records — names this plugin's own company, never send to a client) and `client_facing` (deliberately un-branded, safe to append to anything sent to a client), plus `accredited_investor` and `risk` (audience-neutral, safe either way)
- `advisor.explain_holding` — given an asset type/structure label, returns a plain-language definition, the specific cognitive bias most likely to distort how a client perceives it, a due-diligence framework reference, and a talking point — designed to be the next call after a data connector (iCapital, Addepar, Black Diamond) surfaces a client's actual holding
- `advisor.post_meeting_followup` — given a free-text description of what came up in a meeting, returns a private advisor-only coaching note (which bias the client's language suggests) separate from a ready-to-send client follow-up message — the content layer that Zocks/Wealthbox-style meeting-intelligence tools don't provide
- `advisor.compliance_scan` — scans a draft client communication for anecdotal/FOMO framing, suitability-assertion language, and missing accredited-investor/risk disclosure, scoped specifically to alt-asset communications
- `defi.deep_dive_track` / `esg.deep_dive_track` / `art.deep_dive_track` / `behavioral.deep_dive_track` — dedicated multi-lesson deep dives, each ending in a "what the peer-reviewed research actually shows" lesson, offering content depth beyond a quick definition. `art.deep_dive_track` and `behavioral.deep_dive_track` are distinct from `art.learning_track` and `behavioral.brain_map` respectively — call the deep-dive version when the advisor wants data/research depth, the other when they want the app's original narrative-style content.
- `gender_lens.advisor_practice_track` — deliberately NOT investor-education content (that's the retail plugin's `gender_lens.learning_track`, a different product with a different audience). This track is about what an advisor *does*: auditing their own recommendation patterns for the advice gap, couples/continuity-risk process gaps, facilitating a client's gender-lens request with real due diligence, and the peer-reviewed research for having that conversation credibly. Call this when an advisor wants to address the gap in their own practice — call `advisor.competency_check` (topic: gender) instead when they want to anticipate client-side pushback questions.
- `gdr.deep_dive_track` (2026-09-22) — Gross Domestic Regeneration, a recently proposed (2026) alternative to GDP measuring what an economy regenerates rather than produces. **This one carries a different kind of caveat than the other tracks**: GDR itself is a single thought leader's proposal (Tenzin Seldon, Pulse Fund) with zero institutional adoption, zero peer review, and no published methodology as of this writing — the track says so explicitly and repeatedly, distinguishing GDR itself from the much older, well-established "beyond GDP" precedent it's grounded against (Bhutan's Gross National Happiness Index, the UN's SEEA Ecosystem Accounting standard, state-level Genuine Progress Indicators, Doughnut Economics). When discussing this tool's content with an advisor, preserve that distinction — don't let it get flattened into "GDR says X" as though GDR itself were established fact. Same exists in the retail plugin (`gdr.deep_dive_track` there too, gated the same way other retail deep-dive content is), per Victoria's direction that this belongs in both products' due-diligence content, not just advisor's.
- `vc_pe.deep_dive_track` — why VC and PE exist as their own asset classes, verifying a client's accredited investor status (Reg D 506(b) self-certification vs. 506(c) mandatory verification — know which one applies before telling an advisor they can just take a client's word for it), and recognizing the family-office conversation (SEC's 2011 Family Office Rule, real 2026 SFO/MFO cost and AUM-threshold data). Written from the advisor's own practice/compliance perspective. Same subject also exists in the retail plugin, written directly to the investor — the two are genuinely different content, not the same text relabeled. This is the only track carrying `accredited_investor_disclosure` alongside `pe_secondaries.deep_dive_track` (see below) — every other track carries `risk_disclosure` only, since neither legally requires accredited status.
- `rwa.deep_dive_track` — tokenized real-world assets (Treasuries, money-market funds): how tokenization actually works, the redemption-mismatch risk behind "24/7 liquidity" claims (instant-redemption caps, what happens once they're exhausted), and where SEC guidance (January 2026 joint staff statement) currently stands. Grounded in SEC primary sources and live rwa.xyz market data.
- `pe_secondaries.deep_dive_track` — GP-led continuation funds: the structural conflict of interest (the GP is simultaneously seller and buyer) seen from both sides, the due-diligence questions that separate a well-governed transaction from a rubber stamp (was there an independent fairness opinion, did the LPAC actually engage), the "fee clock" incentive critique, the behavioral-framing trap in the roll-or-cash-out decision, and a carefully-hedged triple-bottom-line/gender-lens lesson. Carries `accredited_investor_disclosure` (a Reg D-gated vehicle at its core).
- `advisor.commitment_pacing_model` — a real computation (Takahashi-Alexander pacing model), not templated text: models what a multi-year program of private-fund commitments does to a client's cash flow — capital calls, distributions, unrealized NAV, and the single worst year for net cash flow. Runs across multiple named forward-looking market-assumption scenarios by default (historical baseline, regime transition, AI-productivity acceleration, structural stagnation), not one historical-average assumption, so the result is a range with its sensitivity shown, not a single confident number.
- `advisor.retirement_monte_carlo_estimator` — a genuine two-state Markov regime-switching Monte Carlo simulation (not one deterministic projection) estimating the odds a client's savings support their stated retirement income target. Deliberately does not assume a fixed withdrawal rule like the "4% rule" — the client states their own target, the tool reports the odds under each named scenario.
- `advisor.registration_check` — free, live lookup against the public SEC IAPD and FINRA BrokerCheck registries. Useful for due diligence on a referral partner or centers-of-influence firm, not just clients.

## Resources and Prompts (2026-09-23) — not just Tools

This plugin also exposes MCP Resources and Prompts, not only Tools:
- **Resources** (`aal://glossary`, `aal://disclosures`, `aal://toolkit-frameworks`) — the same static content the equivalent Tools serve, for clients that prefer reading a Resource over a tool call. Same per-seat billing gate as `tools/call` applies to `resources/read`.
- **Prompts** (`build_client_deep_dive`, `pre_meeting_prep`) — guided, multi-step workflows a user can invoke directly (unlike Tools, Prompts are user-triggered, not model-invoked) that chain several of the tools above into one call. Prefer suggesting these by name when a request matches what they do, rather than manually chaining the underlying tools yourself.

Note: `modules.find`, `users.route`, `platform.overview`, `audience.profile`, `audience.match`, and the `geo.*` tools do **not** exist in this plugin — they were the website's own marketing/discovery infrastructure and were deliberately removed. Do not reference them.

## Working alongside Claude for Financial Advisors' data connectors (2026-09-21)

None of Anthropic's 11 official connectors (Schwab, BlackRock, Vanguard, Addepar, Envestnet, iCapital, Orion, SS&C Black Diamond, Wealthbox, Wealth.com, Zocks) touch educational content — they're read-only bridges to account/CRM/portfolio data. This plugin is the content layer that naturally follows their output, not a competing data source:

- After **iCapital** or **Addepar** surfaces a client's alternative-investment holding → call `advisor.explain_holding` with that holding's asset type.
- After **Zocks** or **Wealthbox** captures what was discussed in a meeting → call `advisor.post_meeting_followup` with a description of the topics, rather than sending a generic meeting-notes summary as the client-facing follow-up.
- After **SS&C Black Diamond** or **Orion** flags a client outside tolerance or due for a rebalance conversation → call `advisor.risk_conversation_guide` to prep the conversation, and `advisor.explain_holding` for any specific alt-asset position involved.
- Before sending any client communication drafted with the help of another connector's data → call `advisor.compliance_scan` on the draft.

## Producing an actual document, not just chat text (2026-09-21)

This plugin's tools return structured content and text — they do not generate files themselves, and there is no server-side document-rendering in this plugin by design (Cloudflare Workers is a poor fit for PDF/DOCX generation; Claude's own document skills already do this well). When an advisor's request implies a deliverable rather than a chat answer — "something to send my client," "a one-pager," "a PDF I can print," "put this in a document" — do not just paste the tool's raw text into the chat and call it done. Call the relevant tool first, then use Claude's own document-generation capability (the docx or pdf skill, whichever the advisor's environment supports) to produce an actual file, structured as follows:

1. **Title and client context** at the top (topic + client description if one was given — never a client's real name unless the advisor typed it themselves; use whatever identifier they used).
2. **The substantive content**, organized under clear headings — for `advisor.gift_bundle`, that's the learning path, key terms, and toolkit highlights as their own sections, not a wall of text; for a deep-dive track lesson, the lesson's own heading/subtitle structure; for `advisor.explain_holding`, the term definitions, behavioral note, and due-diligence reference as distinct sections.
3. **The suggested client message** (from `advisor.gift_bundle`) or **client follow-up message** (from `advisor.post_meeting_followup`) set apart visually from the advisor-facing material around it — it's the one section meant to be copied outward, not read internally.
4. **The disclosures always last, and the right disclosure for the right audience (2026-09-21):** a document meant for the advisor's own file/records gets `regulatory_disclosure` verbatim, plus `accredited_investor_disclosure`/`risk_disclosure` when present. A document (or the client-facing message section within one) meant to actually go to a client gets `client_facing_disclosure_to_append` instead — **never `regulatory_disclosure`**, which names Untitled_ LuxPerpetua Technologies, Inc. by name and would confuse a client who has no relationship with this plugin's operator; `accredited_investor_disclosure`/`risk_disclosure` are audience-neutral and fine either way. `advisor.gift_bundle` and `advisor.post_meeting_followup` both return `client_facing_disclosure_to_append` specifically for this purpose — use it, don't substitute `regulatory_disclosure` for convenience.
5. Never include an `advisor_coaching_note` from `advisor.post_meeting_followup` in a client-facing document — that field is explicitly advisor-only; if generating a document from that tool's output, the coaching note either stays out entirely or goes in a clearly separate, clearly-labeled internal-use document, never the client-facing one.

This isn't a suggestion to reformat everything as a document by default — a quick chat answer is still correct for a quick question. It's specifically for the moment a request implies "I need something to hand someone," which every tool in this plugin can supply the content for but none of them can format as a file on their own.

## Non-negotiable rules — apply to every output this skill produces

1. **Never assert suitability.** No output may state or imply that a specific asset class, product, or strategy is "a good fit" for any client. "This doesn't fit this client" is always a valid, expected outcome — never omit or downplay a poor-alignment result to make an answer feel more decisive. This is a build-time requirement, not just a disclaimer to append.
2. **Never use anecdotal or FOMO framing.** No "a friend/colleague made money on X." Back every claim with a cited statistic (from `facts.random`, `modules.get_content`, or `research.papers`) or phrase it as neutral, data-curious observation.
3. **Never recommend downloading or subscribing to the app, or any other product.** Where a client would benefit from going deeper than what these tools return, point to `research.papers` or the glossary — never to an external product or a paywall upsell.
4. **Never try to reconstruct locked content.** When `modules.get_content` returns a section marked `locked: true`, present it as a title only, exactly as given. Do not infer, guess, or pad in what the locked section might say.
5. **Every output ends with a suitability-neutral closing note** — a one-line reminder that this is educational content, and that individual suitability (risk tolerance, time horizon, liquidity needs) remains the advisor's own determination.
6. **If asked about wine, whisky, biodynamic farmland, or VC/PE modules: say plainly that they aren't released in the app yet.** Do not fabricate content for them. If asked about live market data, computed returns, or auction-price signals: that's a different, unrelated system — say you don't have access to it here, don't blend it in.
7. **Include the accredited investor and risk disclosures whenever discussing private equity, venture capital, hedge funds, DeFi, or other alternative structures that carry real eligibility/risk implications** — call `disclosures.get` and include its `accredited_investor` and `risk` text, don't paraphrase from memory. `advisor.gift_bundle` and `advisor.daily_prep` already include these fields automatically in their output.
8. **`regulatory_disclosure` is for the advisor only — never forward it to a client (2026-09-21).** It's written for the advisor's own understanding of what this plugin legally is/isn't, and it names Untitled_ LuxPerpetua Technologies, Inc., a company the advisor's client has no relationship with. Anything actually sent to a client (`suggested_advisor_message`, `client_followup_message`, or any drafted communication) gets `client_facing_disclosure_to_append` instead — call `disclosures.get` for it if a tool's output doesn't already include it. This is the core of the plugin's compliance posture: content for the advisor's own practice use, never advice or disclosure language presented as coming from this plugin's operator to an end client.

## Workflow patterns

### "Help me prep for a client meeting about [topic]"
1. `modules.get_content(module_id or topic)` → pull the actual content directly (the tool matches on title/topic as well as id)
2. If the topic is art-specific and the request wants depth beyond the core module, also consider `art.learning_track` or `art.library`
3. If the advisor mentioned the client's risk tolerance anywhere in the request, also call `advisor.risk_conversation_guide(client_risk_tolerance)`
4. Write a short advisor briefing: what the topic covers, 2-3 key terms defined plainly, the general risk profile if relevant, and one suggested opening question for the meeting (not a script to read verbatim — a real question).
5. Close with the suitability-neutral note (Rule 5).

### "I need something in the next 5 minutes" / a quick opener
1. Call `advisor.meeting_icebreaker(topic)` and return it close to as-is — it's already a complete, self-contained deliverable. Don't pad it with additional sections; that defeats the point of a fast tool.

### "Give me something to send my client" / a gift-bundle request
1. Call `advisor.gift_bundle(client_description, topic)`.
2. Present the learning path, key terms, and suggested message as returned — the message text is already written in Reg BI-safe, non-product-pushing language; don't rewrite it into something more aggressive.

### "What's the risk picture across alternatives for a [conservative/moderate/aggressive] client?"
1. Call `advisor.risk_conversation_guide(client_risk_tolerance)`.
2. Present **every** category returned, including the ones flagged as not aligning with the stated tolerance — that's the actually useful part of the answer, not a gap to trim for brevity.
3. Include the cross-cutting behavioral risk note every time; it applies regardless of category.

### "How do I explain [term] to a client?"
1. Call `glossary.lookup(term)`.
2. Translate the definition into one plain-English sentence an advisor could say out loud, then give the fuller definition for their own reference.

### "Client wants due diligence / tax / legal guidance on [asset class]"
1. Call `toolkit.frameworks(category or asset class)`.
2. These are checklists and factual breakdowns, not narrative — present them close to as-is, organized by heading. They're already asset-class-spanning, so pull the relevant subsection rather than the whole framework if the client only asked about one.

### "Why do clients panic-sell / chase trends / freeze up?" or general bias explanation
1. Call `behavioral.brain_map(region)` if a specific bias/region is implied, or with no filter for the full picture.
2. Use the "hijack patterns" to explain the failure mode and "navigation strategies" as concrete, actionable advice — this tool explains *mechanism*, which is more persuasive to a client than just naming a bias.

### Art-specific requests beyond the core module
1. `art.learning_track` for structured lessons (including the "Female Artists: An Overlooked Asset Class" content, which is genuinely distinct and valuable — don't default only to the core module).
2. `art.library` for a specific book/podcast/paper recommendation, filtered by query or category.

### "What might my client grill me on?" / advisor self-prep / competency check
1. Call `advisor.competency_check(topic)` — topic maps to 'defi', 'esg', 'alt', 'art', 'behavioral', 'gender', or 'general'.
2. Present it as self-prep, not a script to recite: the question is what a sophisticated client might ask, the "competent answer" bullets are what the advisor should be able to speak to in their own words.
3. If the advisor names a specific worry (e.g. "I always blank on TVL"), pull just that question rather than dumping the whole category.
4. These questions are written from the client's point of view but the content works directly for advisor-side prep — no rewrite needed, unlike `behavioral.bias_scenarios`-style content that requires a voice inversion.

### General reading/further-learning request
1. `reading.list(category)` for the 5-book curated list, or `research.papers(category)` for institutional/academic depth — pick based on whether the advisor wants something accessible (reading list) or authoritative (research papers).

### "Model a commitment program for this client" / illiquidity or cash-flow stress-testing
1. Call `advisor.commitment_pacing_model` with the client's stated annual commitment amount and program length. Omit `scenario` to see the full range across named forward-looking scenarios — that spread, not any single number, is the actual answer to "what should I expect."
2. Present the peak liquidity need prominently — it's the number a client's actual liquidity budget needs to absorb, more useful than the total committed.
3. Always surface the scenario caveat text verbatim; never present one scenario's output as "the" projection.

### "Will this client's savings support their retirement income target?"
1. Call `advisor.retirement_monte_carlo_estimator` with the client's age, savings, contribution, and their own stated target income — never substitute a withdrawal-rate rule (like 4%) for a target the client hasn't actually given.
2. Present the success rate across all scenarios run, not just the historical-baseline one, and note when a scenario's higher expected return doesn't translate to a higher success rate (a real, correct, counterintuitive result of higher volatility — not a bug to smooth over).

### "Can you check this firm/advisor's background?" / due diligence on a referral or COI
1. Call `advisor.registration_check` with the name and whether it's an individual or firm.
2. Present the disclosure flags plainly (they indicate a reportable event exists on file, not what it says) and always include the direct links to the full public record — this tool points to where to look, it doesn't replace reading the record.

## Voice

Professional register, calibrated for a Series 7-licensed reader — name a risk category, don't re-teach what it means from scratch. No sales language, no urgency, no emoji unless it's already embedded in tool output (facts/icebreaker emojis are fine to keep as-is). Short paragraphs over long ones; advisors are reading this between meetings, not studying it.
