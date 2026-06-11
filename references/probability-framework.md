# Probability and Recommendation Framework

Use this framework to convert evidence into a clear but probabilistic investment view.

## Scenario Setup

For each selected horizon, define three mutually exclusive scenarios:

- Bullish: price or index meaningfully rises, or the target outperforms the benchmark.
- Neutral: range-bound, mixed, or roughly in line with benchmark.
- Bearish: price or index meaningfully falls, or the target underperforms.

Probabilities must sum to 100%. Use rounded increments, usually 5 percentage points.

## Evidence Weighting

Weigh evidence across these categories:

- Fundamentals: growth, margins, cash flow, balance sheet, guidance, quality of earnings.
- Valuation: absolute multiples, relative multiples, growth-adjusted valuation, rate sensitivity.
- Catalysts: earnings, product launches, regulatory decisions, macro events, management actions.
- Technicals: trend, momentum, support/resistance, volume, relative strength.
- Sentiment/positioning: analyst revisions, options, short interest, flows, crowding.
- Macro/regime: rates, inflation, Fed path, dollar, liquidity, risk appetite.

Do not let one category dominate unless it is clearly the main driver, such as an earnings shock or regulatory event.

For deep analysis, also weigh evidence from extended dimensions defined in `analytical-dimensions.md`:
- Insider trading, institutional ownership, options flow, competitive moat, management quality, supply chain risk, capital allocation, ESG factors, seasonality, factor exposure.

## Multi-Horizon Probability

For standard and deep analysis, produce a probability table for each horizon:

| Horizon | Bullish | Neutral | Bearish | Confidence |
|---------|---------|---------|---------|------------|
| Short (1-4w) | XX% | XX% | XX% | low/med/high |
| Medium (1-3m) | XX% | XX% | XX% | low/med/high |
| Long (6-12m) | XX% | XX% | XX% | low/med/high |

**Horizon guidance**:
- **Short term** is driven mainly by catalysts, technicals, and sentiment.
- **Medium term** adds fundamentals and valuation as primary drivers.
- **Long term** is driven mainly by competitive moat, secular trends, management quality, and capital allocation.

Probabilities across different horizons do not need to sum to 100%. They are independent assessments.

If a horizon's probability differs drastically from another (e.g., short-term bullish 70% but long-term bearish 55%), explain the transition thesis — what changes between now and then?

## Confidence Levels

- **High**: multiple independent sources align, data is fresh, and invalidation conditions are clear. Data completeness is "complete" per `data-quality.md`.
- **Medium**: evidence leans one way but has meaningful uncertainty or mixed signals. Data completeness is "partial."
- **Low**: data is incomplete, event risk is high, news is fast-moving, or signals conflict. Data completeness is "severe."

### Confidence Calibration Principles

- **Avoid overconfidence**: when evidence is thin or mixed, prefer medium confidence.
- **Probabilities near 50% should rarely have high confidence** — if the outcome is uncertain, confidence should reflect that.
- **Rapidly changing situations** (earnings day, FOMC day, major news) → cap at medium confidence.
- **High confidence requires**: ≥3 independent A/B-grade sources, data freshness within TTL, and no significant source conflicts.

## Near-50/50 Scenario Guidance

When bull and bear probabilities are both in the 40-50% range:

- The primary recommendation should be **hold/watch**, not buy or sell.
- Focus the report on what would tip the balance — specific catalysts or data points.
- Use the "wait for X before acting" framing.
- The quantitative score (`scoring-model.md`) should cluster near 45-55, confirming the neutral stance.

## Action Mapping

- Bullish probability 60% or above with medium/high confidence: buy/add for suitable risk profiles, or constructive view.
- Bullish 50%-60% with mixed evidence: hold/watch, selective buy on pullbacks, or small position only.
- Neutral scenario highest: hold/observe, wait for catalyst, trade range only if user asks for trading setup.
- Bearish probability 55% or above: reduce/avoid, hedge, or wait for better entry.
- Low confidence: avoid strong sizing language; emphasize conditions that would improve confidence.

## Consistency with Quantitative Score

The probability view and the quantitative score (`references/scoring-model.md`) must be directionally consistent. Run this check before finalizing:

| Score Range | Expected Probability Direction | If Mismatched |
|-------------|-------------------------------|---------------|
| 85-100 | Bullish should be highest | Re-evaluate evidence weighting |
| 70-84 | Bullish should be ≥50% | Re-check for overlooked risks |
| 55-69 | Bullish slightly favored | Sensitivity analysis recommended |
| 45-54 | Neutral should be highest or near-highest | Check if you're being too cautious |
| 30-44 | Bearish slightly favored | Re-check for overlooked strengths |
| 15-29 | Bearish should be ≥50% | Re-check for overlooked support |
| 0-14 | Bearish should be highest | Re-evaluate evidence weighting |

If a mismatch is found, follow the reconciliation process in `scoring-model.md` Section 4.2.

## Required Output

Every probability forecast must include:

- Horizon.
- Probability table.
- Main evidence for each scenario.
- What would change the probability.
- Key risks.
- Data timestamp and source limitations.
- Consistency note with quantitative score (standard/deep reports only).

## Language Rules

Use clear wording:

- "基于当前证据，我倾向..."
- "未来 1-3 个月上涨概率约..."
- "这个概率不是确定性预测，主要取决于..."

Avoid impossible certainty:

- Do not say "一定上涨", "必跌", "稳赚", or "准确预测".
- Do not imply suitability for the user's personal portfolio without personal context.
- Do not express probability with false precision (e.g., "63.7%" — round to nearest 5pp unless a quantitative model with clear methodology is cited).
