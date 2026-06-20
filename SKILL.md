---
name: analyze-us-stocks
description: Analyze US stocks, ETFs, Nasdaq/NYSE companies, US indices, sectors, themes, earnings, valuation, technicals, news catalysts, positioning, and probabilistic trade or investment views. Use when asked in Chinese or English for US equity research, market trend scans, individual stock reports, earnings or news reactions, buy/hold/sell views, watchlist comparisons, risk checks, or short-, medium-, or long-horizon price direction forecasts using current market data, filings, and news.
---

# Analyze US Stocks

## Operating Standard

Use this skill to produce evidence-grounded US equity research. Output in Chinese by default unless the user asks for another language.

- Ground prices, news, earnings dates, analyst actions, macro data, guidance, and filings in current retrieval. Never rely on memory for market-sensitive facts.
- Start with the conclusion, then show the evidence and risks. Keep quick answers compact.
- Treat outputs as research support, not personalized financial advice. If the user asks for position sizing or suitability, ask for risk tolerance, holding period, portfolio context, and current position before giving a sizing view.
- Label each material fact with a source grade (A/B/C/D), source name or URL, and timestamp/date.
- If retrieval is unavailable or core data is stale, state the limitation near the conclusion and cap confidence.

## Fast Triage

Before retrieval, identify:

- Target: single stock, ETF/index, sector/theme, comparison, watchlist, or event.
- User goal: intraday trade, swing trade, long-term investment, earnings/news reaction, risk review, or idea scan.
- Horizon: default to short term 1-4 weeks, medium term 1-3 months, and long term 6-12 months unless specified.
- Depth: choose quick, standard, or deep.

Ask at most one concise clarification only when the ticker/target is ambiguous or the user requests personal suitability/sizing. Otherwise proceed with reasonable defaults.

| Depth | Use When | Evidence Budget | Required Output |
| --- | --- | --- | --- |
| Quick | 盘中问股, "今天能不能买", "快速看一下", news reaction | P0 only; 4-6 high-signal sources | Conclusion, 3-scenario probability, key evidence, key levels/risks, sources |
| Standard | Default single-stock, ETF, index, sector, or theme report | P0 + targeted P1; stop when added sources are unlikely to change the view | Quick + 6-category score, fundamentals, valuation, peers, macro, invalidation |
| Deep | Major decision, high-risk setup, "深度", "全维度", large move, earnings/FOMC/regulatory event | P0 + P1 + selected P2; use all 12 dimensions only when explicitly requested | Standard + extended dimensions, full data-quality review, sensitivity |

For multi-stock comparisons, run the minimum comparable workflow for each ticker first, then rank. Do not deep-dive every ticker unless requested.

## Retrieval Workflow

Prefer structured data tools when available. Otherwise use official and primary sources first, then reputable financial media/data aggregators, then broad search.

### P0: Must Gather

Run these in parallel when tools allow:

1. Quote snapshot: latest price, percent change, volume, 52-week range, market cap, and timestamp.
2. Latest company or theme news: intraday plus the last 7 days, with event dates.
3. Latest earnings, guidance, or filing: earnings release/10-Q/10-K/8-K/IR page.
4. Market regime: SPY/QQQ or relevant index, VIX, 10-year Treasury yield, dollar, and sector ETF.

If a P0 item is unavailable, mark it missing. Do not invent substitutes.

### P1: Standard Add-Ons

Use only the P1 packs that can affect the user's decision:

- Valuation and peers: forward P/E, EV/EBITDA, PEG, FCF yield, growth, margins.
- Analyst revisions: consensus, target changes, estimate revisions, date and firm.
- Technicals: trend, moving averages, support/resistance, RSI or momentum, volume-price behavior.
- Filings and calls: latest 10-Q/10-K/8-K, transcript, management guidance, risk factors.
- Positioning: short interest, options, insider transactions, institutional ownership when material.

### P2: Deep or Event-Driven Add-Ons

Read `references/analytical-dimensions.md` only when deep analysis or a specific risk area requires it. Select dimensions by materiality:

- Trading setup: short interest, options flow, seasonality, factor exposure.
- Earnings reaction: guidance quality, analyst revisions, management tone, peer read-through.
- Long-term investment: moat, capital allocation, management quality, supply chain/geography, ESG materiality.
- Risk review: short interest, leverage/liquidity, regulatory/legal, supply chain/geography, governance.

## Evidence Ledger

Maintain an internal evidence ledger before scoring:

| Field | Requirement |
| --- | --- |
| Claim/data point | Concrete fact or metric |
| Value/unit | Price, percent, multiple, date, or text summary |
| Source | URL/name and source grade A/B/C/D |
| Timestamp | Publication date, filing date, quote timestamp, or market session |
| TTL status | Fresh, stale, unknown, or unavailable |
| Independence | Independent source or same original source |
| Thesis impact | Bullish, neutral, bearish, or only background |

Rules:

- No source, no material claim.
- No reliable source, no strong conclusion.
- Major facts require at least two independent sources unless they come directly from an A-grade primary source.
- D-grade sources can support sentiment only; never use them as the sole basis for a fact.
- If sources conflict, prefer A > B > C > D and explain the conflict if it affects the view.

## Data Quality Gate

Apply `references/data-quality.md` before forming the final view:

- Complete: all P0 and decision-critical P1 data are fresh; confidence may be high if sources align.
- Partial: 1-3 important items are missing/stale; cap confidence at medium.
- Severe: core quote, news, or latest financials are missing/stale; cap confidence at low and avoid a strong buy/sell action.

For quick answers, still state the quote/news timestamp and any missing P0 item.

## Analysis Method

Separate facts from interpretation. Use phrases such as "数据显示", "公司披露", "市场反映", and "我的推断是".

### Score

Use `references/scoring-model.md` after data collection:

- Quick: score the 6 categories only if enough evidence exists; otherwise omit the total score and explain why.
- Standard: score 6 categories on a 0-100 weighted basis.
- Deep: score the 21 sub-items and show confidence for uncertain sub-scores.
- Do not impute missing data as neutral. Mark the sub-score as estimated or unavailable and lower confidence.

### Probability

Use `references/probability-framework.md`:

- Probabilities must sum to 100% for each horizon.
- Use 5 percentage-point increments unless a documented model justifies finer granularity.
- Short-term weight: catalysts, technicals, sentiment, positioning.
- Medium-term weight: fundamentals, valuation, estimates, macro.
- Long-term weight: moat, capital allocation, management quality, secular trends.

### Consistency Check

Reconcile score and probability before finalizing:

- Score >70 and bullish probability <50%: re-check overlooked risks.
- Score <30 and bearish probability <45%: re-check overlooked support.
- Score 45-54 and any directional probability >60%: re-check overconfidence.

If the final view remains mixed, say so directly and use hold/watch or wait-for-trigger language.

## Report Output

For quick reports, use:

1. 结论先行: action view, confidence, data timestamp, data quality.
2. 概率: bullish/neutral/bearish for the selected horizon.
3. 关键证据: 3-6 bullets with source grade and date.
4. 关键价位/触发条件: support/resistance, catalyst, invalidation.
5. 风险 and next watch items.
6. Sources and limitations.

For standard and deep reports, use `references/report-template.md` and compress sections that do not affect the decision.

Always include:

- Data-as-of timestamp or market session.
- Source reliability distribution or at least source grades for key evidence.
- Missing/stale/conflicted data.
- Counterarguments and measurable invalidation signals.
- Clear uncertainty language; never say "稳赚", "必涨", "必跌", or "准确预测".

## Reference Loading

Load only what the task needs:

- `references/source-checklist.md`: minimum evidence by task.
- `references/search-queries.md`: query packs and parallel retrieval strategy.
- `references/data-quality.md`: TTL, source grades, cross-validation, completeness.
- `references/scoring-model.md`: scoring rules and score-probability reconciliation.
- `references/probability-framework.md`: scenario probabilities and action mapping.
- `references/report-template.md`: standard/deep Chinese report structure.
- `references/analytical-dimensions.md`: deep or extended dimension selection.
- `references/agent-integration.md`: only when configuring this skill for another agent or plugin surface.

If loaded from the Codex plugin subdirectory `skills/analyze-us-stocks`, resolve references from `../../references/...` when `references/...` is not present.
