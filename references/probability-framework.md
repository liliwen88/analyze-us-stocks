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

## Confidence Levels

- High: multiple independent sources align, data is fresh, and invalidation conditions are clear.
- Medium: evidence leans one way but has meaningful uncertainty or mixed signals.
- Low: data is incomplete, event risk is high, news is fast-moving, or signals conflict.

## Action Mapping

- Bullish probability 60% or above with medium/high confidence: buy/add for suitable risk profiles, or constructive view.
- Bullish 50%-60% with mixed evidence: hold/watch, selective buy on pullbacks, or small position only.
- Neutral scenario highest: hold/observe, wait for catalyst, trade range only if user asks for trading setup.
- Bearish probability 55% or above: reduce/avoid, hedge, or wait for better entry.
- Low confidence: avoid strong sizing language; emphasize conditions that would improve confidence.

## Required Output

Every probability forecast must include:

- Horizon.
- Probability table.
- Main evidence for each scenario.
- What would change the probability.
- Key risks.
- Data timestamp and source limitations.

## Language Rules

Use clear wording:

- "基于当前证据，我倾向..."
- "未来 1-3 个月上涨概率约..."
- "这个概率不是确定性预测，主要取决于..."

Avoid impossible certainty:

- Do not say "一定上涨", "必跌", "稳赚", or "准确预测".
- Do not imply suitability for the user's personal portfolio without personal context.
