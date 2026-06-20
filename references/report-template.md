# 中文美股综合研报模板

Use this structure by default. Compress sections for quick requests, but keep the conclusion, probability view, evidence, risks, and sources.

---

## 使用规则

- Quick 报告只保留第 1、2、6、7、8、9 节的压缩版。
- Standard 报告展示 6 大类评分；Deep 报告才展开 21 子项和扩展维度。
- 每条关键证据都要带来源等级和日期，例如 `[A-10-Q, 2026-05-10]`。
- 如果数据质量为 Partial/Severe，把局限写进第 1 节，不要只放在末尾。
- 若某节对结论没有影响，写一句"本次未发现会改变结论的材料信息"即可，不要填充泛泛内容。

---

## 1. 结论先行

- 标的/主题：
- 时间周期：短期 (1-4周) / 中期 (1-3月) / 长期 (6-12月)
- 当前判断：看涨 / 中性 / 看跌
- 操作倾向：买入 / 加仓 / 持有 / 观望 / 减仓 / 回避
- 量化评分：XX/100（[评分区间标签]）
- 核心理由：用 3-5 条说明最重要依据。
- 置信度：低 / 中 / 高
- 数据质量：[完整 / 部分缺失 / 严重缺失]
- 数据时间：注明行情、新闻和财报数据的时间点。

---

## 2. 涨跌概率预测

| 情景 | 概率 | 可能表现 | 主要依据 | 触发/失效条件 |
| --- | ---: | --- | --- | --- |
| 上涨 | XX% | | | |
| 震荡 | XX% | | | |
| 下跌 | XX% | | | |

Keep probabilities rounded (5pp increments) and make them sum to 100%.

**多时间框架概率**（标准/深度报告）：

| 时间框架 | 上涨 | 震荡 | 下跌 | 置信度 |
| --- | ---: | ---: | ---: | --- |
| 短期 (1-4周) | XX% | XX% | XX% | |
| 中期 (1-3月) | XX% | XX% | XX% | |
| 长期 (6-12月) | XX% | XX% | XX% | |

---

## 2.5 量化评分详情（标准/深度报告）

**综合评分**：XX/100 — [强烈看涨 / 看涨 / 偏涨 / 中性 / 偏跌 / 看跌 / 强烈看跌]

| 评分大类 | 得分 | 满分 | 权重 | 关键子项说明 |
| --- | ---: | ---: | ---: | --- |
| 基本面 | | 25 | 25% | 营收增速、利润率、财务健康、护城河 |
| 估值 | | 15 | 15% | 绝对估值、成长调整、利率敏感度 |
| 催化剂 | | 20 | 20% | 近期事件、质量、长期趋势、意外潜力 |
| 技术面 | | 15 | 15% | 趋势、支撑阻力、量价关系 |
| 情绪/仓位 | | 15 | 15% | 分析师情绪、拥挤度、内部人信号 |
| 宏观/制度 | | 10 | 10% | 利率货币、周期位置、行业适配 |
| **总计** | **XX** | **100** | **100%** | |

> 深度报告应展示 21 子项得分细节。标准报告仅展示 6 大类。

**一致性校验**：
- 量化评分 vs 概率判断：[一致 / 不一致（已复核）]
- 如不一致，说明原因：

---

## 3. 市场和行业背景

Cover index trend, rates, dollar, volatility, sector rotation, macro events, and theme heat. Explain whether the backdrop helps or hurts the target.

---

## 4. 个股/主题基本面

For a company, cover revenue growth, margins, EPS, free cash flow, balance sheet, guidance, competitive position, valuation, and key business drivers. For a theme or sector, cover demand, supply, policy, cycle position, and representative leaders/laggards.

---

## 4.5 同行对比矩阵（标准/深度报告）

| 指标 | [目标公司] | 同行1 | 同行2 | 同行3 | 行业均值 |
| --- | ---: | ---: | ---: | ---: | ---: |
| 市值 ($B) | | | | | — |
| 营收增速 (YoY%) | | | | | |
| 毛利率 (%) | | | | | |
| 营业利润率 (%) | | | | | |
| ROIC (%) | | | | | |
| 预测 P/E | | | | | |
| EV/EBITDA | | | | | |
| PEG Ratio | | | | | |
| FCF Yield (%) | | | | | |
| 净债务/EBITDA | | | | | |
| RSI (14日) | | | | | |
| YTD 涨跌幅 (%) | | | | | |

**同行对比结论**：
- 相对估值位置：
- 相对增长位置：
- 综合竞争力评估：

---

## 5. 新闻催化和情绪

Summarize the latest relevant news, earnings call messages, filings, analyst revisions, management actions, regulatory developments, options/short-interest/sentiment signals when available.

---

## 6. 技术面和关键价位

Mention trend, moving averages, support/resistance, volume, relative strength, and whether price action confirms or conflicts with fundamentals.

---

## 6.5 敏感性分析（标准/深度报告）

关键变量变动对投资判断的影响：

| 情景 | 变量变动 | 预计影响 | 对评分的改变 | 发生概率 |
| --- | --- | --- | ---: | ---: |
| 上行 | 营收 +10% | | +X 分 | |
| 上行 | 利率 −50bp | | +X 分 | |
| 下行 | 营收 −10% | | −X 分 | |
| 下行 | 市盈率压缩 20% | | −X 分 | |
| 下行 | 催化事件未兑现 | | −X 分 | |

**综合敏感性结论**：
- 主要上行驱动：
- 主要下行风险：
- 盈亏比判断（上行空间 vs 下行风险）：

---

## 7. 反证和风险

List the strongest counterarguments. Include specific invalidation signals, such as failed guidance, valuation compression, macro shocks, regulatory action, technical breakdown, or liquidity stress.

**失效条件**（具体可量化触发信号）：
1. [具体条件1] → 触发时意味着判断可能错误
2. [具体条件2]
3. [具体条件3]

---

## 7.5 数据质量评估（标准/深度报告）

| 评估项 | 结果 |
| --- | --- |
| 数据完整性 | 完整 / 部分缺失 / 严重缺失 |
| 来源可靠性分布 | A级: X, B级: X, C级: X, D级: X |
| 强制交叉验证 | [通过/未通过] — 未通过项：[列出] |
| 超时/过时数据 | [列出超时数据项及超时程度] |
| 缺失数据 | [列出无法获取的关键数据] |
| 来源冲突 | [列出冲突及解决方式] |

---

## 8. 需要继续跟踪

List the next 3-6 events/data points that could change the view: earnings date, CPI/Fed events, product launch, filing, conference, analyst day, option expiry, or key technical level.

| 事件 | 预计时间 | 重要性 | 关注点 |
| --- | --- | --- | --- |
| | | 关键 / 重要 / 一般 | |

---

## 9. 来源和局限

Name or link primary sources and key news sources. State any missing data, delayed quotes, or uncertainty that affects confidence.

**来源清单**（按可靠性排序）：
- A 级来源 (XX个)：[列出]
- B 级来源 (XX个)：[列出]
- C 级来源 (XX个)：[列出]
- D 级来源 (XX个)：[如有，列出并说明用途]

**主要局限**：
- [局限1]
- [局限2]

**免责声明**：本文仅为研究分析，不构成个人投资建议。市场存在不确定性，所有概率预测均基于当前证据，可能随新信息变化。
