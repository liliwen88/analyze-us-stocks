---
name: analyze-us-stocks
description: Analyze US stocks, Nasdaq-listed companies, US market indices, sectors, themes, earnings, valuation, technicals, news catalysts, and investor sentiment. Use when Codex is asked in Chinese or English to produce a US equity market report, individual stock research report, market hot-trend scan, earnings/news interpretation, buy/sell/hold view, or probability-based short-, medium-, or long-term price direction forecast using latest market data and news.
---

# Analyze US Stocks

## Core Rule

Always ground the analysis in current evidence. Do not rely only on model memory for prices, news, earnings dates, analyst moves, macro data, or management guidance. If current data or browsing tools are unavailable, state that the analysis is limited and ask to enable retrieval rather than inventing facts.

Write the final report in Chinese by default. Give a clear action view when the user asks for investment advice, but frame it as research judgment, not guaranteed profit or personalized financial advice.

## Workflow

1. Define the target and horizon:
   - Target: market, index, sector/theme, single stock, or comparison.
   - Horizon: short term 1-4 weeks, medium term 1-3 months, long term 6-12 months unless the user specifies otherwise.
   - User objective: trading, investing, risk review, earnings preview, or news reaction.
2. Gather fresh evidence before forming the conclusion:
   - Quote/market data: latest price, daily move, volume, 52-week range, market cap, valuation multiples when available.
   - Market context: S&P 500, Nasdaq 100, Russell 2000, VIX, Treasury yields, US dollar, sector rotation, major macro events.
   - Company fundamentals: latest quarterly results, guidance, margin trend, cash flow, balance sheet, growth drivers, valuation.
   - News and catalysts: company press releases, SEC filings, earnings calls, regulatory events, product/customer news, analyst actions, macro headlines.
   - Technical and positioning signals: trend, support/resistance, moving averages, relative strength, volume, options or short-interest data when available.
3. Cross-check material claims:
   - Prefer primary sources for company facts: SEC filings, investor relations, earnings release, transcript.
   - Use reputable financial news for breaking catalysts.
   - Mark each important data point with source and timestamp/date.
   - Separate observed facts from inference.
4. Build the probability view:
   - Assign probabilities that sum to 100% across bullish, neutral/range-bound, and bearish scenarios for the selected horizon.
   - Tie each probability to concrete evidence and counter-evidence.
   - Include confidence level: low, medium, or high.
   - Never present probability as certainty.
5. Produce the report using `references/report-template.md`.
6. Include source notes and data freshness:
   - State the market close or intraday timestamp used.
   - Link or name the most important sources.
   - If source coverage is incomplete, put the limitation near the conclusion.

## Report Standards

Read these references as needed:

- `references/source-checklist.md` for required data and source hierarchy.
- `references/probability-framework.md` for probability, confidence, and scenario rules.
- `references/report-template.md` for the default Chinese report structure.

The final report must include:

- Clear conclusion: bullish, bearish, or neutral; buy, sell/reduce, hold/watch, or avoid when an action view is requested.
- Forecast horizon and probability table.
- Key evidence supporting the view.
- Counterarguments and invalidation conditions.
- Risks and what to monitor next.
- Source/data freshness notes.

## Safety and Quality Guardrails

- Do not fabricate prices, news, earnings numbers, analyst ratings, or SEC filing content.
- Do not give personalized financial advice based on the user's private financial situation unless they provide that context and explicitly request suitability analysis; even then, keep the output framed as research support.
- Avoid overprecision. Use rounded probabilities such as 55%, 30%, 15% unless a model or dataset justifies finer granularity.
- Prefer ranges and scenarios over single-point price targets when uncertainty is high.
- If the user asks for "accurate" or "guaranteed" predictions, explain that markets are probabilistic and provide the best evidence-weighted forecast with risks.
- If sources conflict, show the conflict and explain which source is more reliable.
