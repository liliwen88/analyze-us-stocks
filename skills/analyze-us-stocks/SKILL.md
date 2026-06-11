---
name: analyze-us-stocks
description: >
  Analyze US stocks, Nasdaq-listed companies, US market indices, sectors, themes,
  earnings, valuation, technicals, news catalysts, and investor sentiment. Use when
  asked in Chinese or English to produce a US equity market report, individual stock
  research report, market hot-trend scan, earnings/news interpretation, buy/sell/hold
  view, or probability-based short-, medium-, or long-term price direction forecast
  using the latest market data and news.
license: MIT
compatibility: Intended for coding agents with web search and web fetch capabilities.
metadata:
  owner: liliwen
  version: "1.0.0"
  language: zh-CN
  category: equity-research
---

# Analyze US Stocks

## Core Rule

Always ground the analysis in current evidence. Do not rely only on model memory for prices, news, earnings dates, analyst moves, macro data, or management guidance. If current data or browsing tools are unavailable, state that the analysis is limited and ask to enable retrieval rather than inventing facts.

Write the final report in Chinese by default. Give a clear action view when the user asks for investment advice, but frame it as research judgment, not guaranteed profit or personalized financial advice.

## Analysis Depth

Before starting, determine the analysis depth based on the user's request:

| Depth | Scope | Time | When to Use |
|-------|-------|------|-------------|
| **Quick** | Price, news, technicals, probability | 5–10 min | 快速扫描、盘中问股 |
| **Standard** | Quick + fundamentals, valuation, peers | 15–30 min | 标准个股/市场报告 |
| **Deep** | Standard + all 12 extended dimensions | 30+ min | 深度研究、重大投资决策 |

For deep analysis, consult `references/analytical-dimensions.md` and apply the dimension selection matrix.

---

## Workflow

### Step 1: Define the target and horizon

- **Target**: market, index, sector/theme, single stock, or comparison.
- **Horizon**: short term 1–4 weeks, medium term 1–3 months, long term 6–12 months unless the user specifies otherwise.
- **User objective**: trading, investing, risk review, earnings preview, or news reaction.
- **Depth**: quick, standard, or deep (see Analysis Depth above).

### Step 2: Gather fresh evidence (parallel where possible)

**2a. Identify required data**

Based on analysis depth and task type, determine which queries to execute. Use `references/search-queries.md` for pre-built query templates (available in both English and Chinese). Select dimensions from `references/analytical-dimensions.md` for deep analysis.

**2b. Execute data collection in parallel rounds**

Execute queries in parallel rounds as defined in `references/search-queries.md`:

- **Round 1 (P0 — must execute)**: Price/market data, latest quarterly earnings, latest news, market backdrop (SPX, VIX, Treasury yields, Dollar). Run these 4+ queries in parallel.
- **Round 2 (P1 — standard reports)**: Analyst ratings/targets, valuation vs peers, SEC filings, technicals. Run these 4+ queries in parallel.
- **Round 3 (P2 — deep analysis)**: Extended dimensions per the dimension selection matrix. Run dimension queries in parallel batches of up to 8.
- **Round 4 (fill gaps)**: Based on data completeness, execute supplementary queries.

Use WebSearch for discovery and WebFetch for deep content extraction (SEC EDGAR files, IR pages, data aggregation pages). Prefer MCP Server tools (Yahoo Finance, Finnhub) if available for structured data.

**2c. Record data quality metadata**

For each key data point, record:
- Data value and unit
- Source URL or name
- Source reliability grade (A/B/C/D per `references/data-quality.md`)
- Data timestamp or publication date

### Step 3: Cross-validate material claims

Before forming conclusions, apply the cross-validation rules from `references/data-quality.md`:

- **Must cross-validate (≥2 independent sources)**: quarterly revenue/EPS, management guidance, major news events, analyst rating changes, insider trading data, short interest, merger/acquisition announcements.
- **Should cross-validate**: price/quotes, valuation multiples, institutional ownership changes, peer comparison data.
- **Source conflict**: when sources disagree, prefer higher reliability grade (A > B > C > D), document the conflict, and explain your choice.
- **Source independence**: republished content from the same wire service counts as ONE source, not multiple.
- **D-grade sources** (social media, forums, anonymous): never use as sole evidence for any material claim.

### Step 4: Build the analysis (probability + quantitative score)

**4a. Qualitative probability view** (per `references/probability-framework.md`)

Assign probabilities that sum to 100% across bullish, neutral/range-bound, and bearish scenarios for each relevant horizon. Tie each probability to concrete evidence and counter-evidence. Include confidence level: low, medium, or high.

**4b. Quantitative score** (per `references/scoring-model.md`)

Score the target 0–100 across 6 categories:
- Fundamentals (0–25): revenue growth, profitability, financial health, moat/growth drivers
- Valuation (0–15): absolute valuation, growth-adjusted valuation, rate sensitivity
- Catalysts (0–20): near-term events, catalyst quality, secular trends, surprise potential
- Technicals (0–15): trend, support/resistance, volume-price relationship
- Sentiment/Positioning (0–15): analyst sentiment, crowding, insider/buyback signals, retail sentiment
- Macro/Regime (0–10): rates/monetary policy, cycle position, sector/style fit

Use the quick scoring shortcut (6 category-level scores) for quick/standard analysis; use the full 21 sub-item scoring for deep analysis.

**4c. Consistency check**

Verify the quantitative score and probability view are directionally consistent:
- Score >70 + bullish probability <50% → **conflict, must re-evaluate**
- Score <30 + bearish probability <45% → **conflict, must re-evaluate**
- Score 45–54 + bullish or bearish >60% → **conflict, must re-evaluate**

If a conflict is found, identify the source of divergence, re-check data quality, and adjust until the two frameworks agree.

### Step 5: Sensitivity analysis

For standard and deep analysis, assess how key variables affect the thesis:

- **Revenue shock**: what if revenue −10% / +10% vs guidance?
- **Rate shock**: what if Fed funds rate +100bp / −50bp?
- **Valuation compression**: what if P/E multiple contracts 20%?
- **Key catalyst failure**: what if the expected catalyst does not materialize?

Label the main upside and downside risks with estimated magnitude of impact.

### Step 6: Produce the report

Use `references/report-template.md` for the structure. Include:

- Clear conclusion with action view
- Probability table for each horizon
- Quantitative score breakdown (by 6 categories, with sub-scores for deep analysis)
- Sensitivity analysis scenarios (standard/deep)
- Peer comparison matrix (standard/deep)
- Key evidence with sources and timestamps
- Counterarguments and invalidation conditions
- Risks and watchlist
- Data quality assessment (completeness, source reliability distribution, missing data)

For quick analysis, compress to: conclusion + probability + key evidence + risks + sources.

### Step 7: Include source notes and data freshness

- State the market close or intraday timestamp used.
- Include source reliability grade distribution (A/B/C/D count).
- List any missing, stale, or conflicted data.
- State data completeness: complete / partial / severe.
- If source coverage is incomplete, put the limitation near the conclusion.

---

## Report Standards

Read these references as needed:

- `references/source-checklist.md` — required data and source hierarchy.
- `references/probability-framework.md` — probability, confidence, and scenario rules.
- `references/scoring-model.md` — 0–100 quantitative scoring across 6 categories (21 sub-items).
- `references/data-quality.md` — data freshness TTL, source reliability grading, cross-validation rules, data completeness ratings.
- `references/analytical-dimensions.md` — 12 extended analysis dimensions with data sources, interpretation rubrics, and cross-validation matrix.
- `references/search-queries.md` — pre-built English/Chinese search query templates with priority levels and parallel execution strategy.
- `references/report-template.md` — default Chinese report structure.
- `references/agent-integration.md` — multi-agent orchestration patterns, tool chain setup, output JSON schema, and error handling.

The final report must include:

- Clear conclusion: bullish, bearish, or neutral; buy, sell/reduce, hold/watch, or avoid when an action view is requested.
- Forecast horizon and probability table (at minimum; add quantitative score for standard/deep).
- Key evidence supporting the view, each with source grade and timestamp.
- Counterarguments and invalidation conditions.
- Risks and what to monitor next.
- Source/data freshness notes with completeness rating.

---

## Safety and Quality Guardrails

- Do not fabricate prices, news, earnings numbers, analyst ratings, or SEC filing content.
- Do not give personalized financial advice based on the user's private financial situation unless they provide that context and explicitly request suitability analysis; even then, keep the output framed as research support.
- Avoid overprecision. Use rounded probabilities such as 55%, 30%, 15% unless a model or dataset justifies finer granularity.
- Prefer ranges and scenarios over single-point price targets when uncertainty is high.
- If the user asks for "accurate" or "guaranteed" predictions, explain that markets are probabilistic and provide the best evidence-weighted forecast with risks.
- If sources conflict, show the conflict and explain which source is more reliable.
- Mark every material data point with source reliability grade (A/B/C/D) and timestamp.
- If data completeness is "severe," state the limitation prominently near the conclusion and consider deferring a clear buy/sell recommendation.
- When quantitative score and probability view conflict, re-evaluate before finalizing — do not present contradictory signals without explanation.
