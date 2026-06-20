# Source Checklist

Use this checklist before producing a US stock or market report.

## Required Freshness

- Use current-date retrieval for news, quotes, macro events, and analyst actions.
- Record the timestamp or market session for all price-sensitive data.
- If only delayed or stale data is available, say so in the conclusion.

## Evidence Budget by Depth

Do not run every possible query by default. Match evidence collection to the user's decision.

| Depth | Must Have | Stop When | Do Not Do |
| --- | --- | --- | --- |
| Quick | P0 quote, latest news/catalyst, market backdrop, one risk/counterpoint | 4-6 high-signal sources establish the view and no core fact conflicts | Full peer matrix, 21-item scoring, all 12 extended dimensions |
| Standard | All P0 + decision-relevant P1 fundamentals, valuation, technicals, peers, analysts or filings | More generic sources repeat known facts and would not change the action view | Deep dimensions with low materiality |
| Deep | P0 + P1 + selected P2 dimensions tied to the decision | Each material thesis driver has source support and counter-evidence has been checked | All 12 dimensions unless explicitly requested or truly material |

Use one high-quality primary source over several recycled summaries. Additional sources are useful only when they validate, challenge, or materially sharpen the conclusion.

## Decision Gates

- If ticker/target is ambiguous, ask one clarification before analysis.
- If latest quote or market timestamp is missing, give only a limited view and avoid strong action language.
- If latest earnings/guidance is missing for a single-stock standard/deep report, cap confidence at medium.
- If a major news claim has only one non-primary source, label it "single-source" and treat it as provisional.
- If the user asks "should I buy/sell now" without personal context, provide a research action view and explicit invalidation levels, not personal suitability.

## Source Hierarchy

1. Primary company sources:
   - SEC 10-K, 10-Q, 8-K, S-1, proxy filings
   - Investor relations earnings releases and presentations
   - Earnings call transcripts
   - Company press releases
2. Market and macro data:
   - Exchange or reputable market-data providers
   - Federal Reserve, BLS, BEA, Treasury, FRED when relevant
   - Index/ETF data for sector and factor context
3. News and analysis:
   - Reuters, Bloomberg, CNBC, WSJ, Financial Times, AP, MarketWatch, Barron's, The Information, or other reputable outlets
   - Analyst actions only when source, firm, and date are clear
4. Supplemental signals:
   - Options activity, short interest, institutional ownership, insider transactions, ETF flows, social/search trend data when available

## Minimum Evidence by Task

### Single-stock report

- Latest price and recent performance.
- Latest earnings results and guidance.
- Latest company-specific news.
- Valuation versus history and peers when available.
- Technical trend and key levels.
- At least one counterargument.
- One measurable invalidation signal.

### Market or index report

- Major index performance.
- Treasury yield and Fed expectation context.
- VIX or volatility proxy.
- Sector leadership and laggards.
- Major macro/news catalysts.
- Breadth or liquidity signals when available.

### Hot-trend scan

- Theme definition.
- Evidence that the theme is currently hot: price action, news volume, earnings mentions, policy, fund flows, or leadership stocks.
- Representative tickers and why they matter.
- Sustainability assessment and risk of crowding.

## Citation Discipline

- Cite the source beside the claim when the claim is material to the recommendation.
- Do not overquote copyrighted news. Paraphrase and link.
- Distinguish facts from interpretation with language such as "数据显示", "公司披露", "市场反映", and "我的推断是".
- Keep an evidence ledger internally: claim, value, source grade, timestamp, TTL status, independence, and thesis impact.
