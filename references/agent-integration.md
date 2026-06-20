# Agent 集成指南

本文档定义 `analyze-us-stocks` skill 在四个 Agent 平台上的集成方式、工具链配置、多 Agent 协作模式以及结构化输出规范。支持的平台：Claude Code（原生 Skill）、OpenAI Codex CLI（Plugin）、OpenAI GPTs/Assistants（YAML 配置）、通用 LLM Agent（Prompt 注入）。

---

## 一、支持的 Agent 平台

### 1.1 Claude Code Skill（原生支持）

**调用方式**：Agent 通过 SKILL.md 中定义的 `$analyze-us-stocks` 自动识别并加载。

**可用工具**：
- `WebSearch` — 获取实时行情、新闻、分析师数据
- `WebFetch` — 抓取 SEC EDGAR、IR 页面、数据平台页面
- `Bash` — 可选，用于数据处理脚本

**最佳实践**：
- Agent 应按照 `search-queries.md` 的并行查询策略进行多轮搜索
- 使用 `WebFetch` 对 A 级来源（SEC EDGAR、公司 IR）进行深度抓取
- 严格遵循 `data-quality.md` 的 TTL 和来源评级规则

### 1.2 OpenAI GPTs / Assistants API

**配置入口**：`agents/openai.yaml` — 只保留 UI 元数据、默认提示和可选工具依赖。完整工作流、schema 和分析规则保留在 `SKILL.md` 与 `references/`，避免元数据文件过重。

**Function Calling 工具定义**（建议在 GPT/Assistant 配置中添加）：

```yaml
# 工具 1: Web Search
search_stock_info:
  description: "Search for the latest stock information, news, and data"
  parameters:
    query:
      type: string
      description: "Search query (e.g., 'AAPL latest earnings Q2 2026')"

# 工具 2: Fetch SEC Filing
fetch_sec_filing:
  description: "Fetch SEC filing content from EDGAR"
  parameters:
    ticker:
      type: string
    filing_type:
      type: string
      enum: ["10-K", "10-Q", "8-K", "Form 4"]

# 工具 3: Fetch Market Data
fetch_market_data:
  description: "Fetch current market data for a ticker"
  parameters:
    ticker:
      type: string
    data_type:
      type: string
      enum: ["quote", "key-statistics", "historical", "holders", "analysis"]
```

**推荐 Instructions / System Prompt 注入**：
```
You are a US stock analyst. Use the analyze-us-stocks skill framework:
1. Read references/analytical-dimensions.md to select analysis dimensions
2. Use references/search-queries.md query templates for data collection
3. Apply references/data-quality.md rules for source reliability and freshness
4. Score the stock using references/scoring-model.md
5. Build probability scenarios using references/probability-framework.md
6. Output the report using references/report-template.md structure
Always cite sources with reliability grade (A/B/C/D) and timestamp.
```

### 1.3 Codex CLI Plugin（原生支持）

OpenAI Codex CLI 从 v0.117.0（2026年3月）起原生支持 Plugin 系统。本 Skill 已提供完整的 Codex Plugin 配置。

**调用方式**：Codex 自动发现 `.codex-plugin/plugin.json` 并加载 skill。

**插件文件结构**：

```
analyze-us-stocks/
├── .codex-plugin/
│   └── plugin.json              # 插件清单（name, version, interface, capabilities, skills路径, mcpServers路径）
├── .mcp.json                     # MCP Server 配置（Yahoo Finance, Finnhub, Alpha Vantage）
├── skills/
│   └── analyze-us-stocks/
│       ├── SKILL.md              # Skill 指令（仅 name + description frontmatter + 工作流）
│       └── agents/
│           └── openai.yaml       # 简短 UI 元数据与可选工具依赖
├── references/                   # 8 个参考文档（按需加载；插件子目录可用 ../../references）
├── SKILL.md                      # Claude Code 兼容入口（保留向后兼容）
└── agents/
    └── openai.yaml               # 独立 GPTs/Assistants 配置（保留向后兼容）
```

**plugin.json 关键字段**：

| 字段 | 说明 |
|------|------|
| `name` | `"analyze-us-stocks"` — 必须与目录名匹配，kebab-case |
| `skills` | `"./skills/"` — 指向 skill 目录 |
| `mcpServers` | `"./.mcp.json"` — 指向 MCP 配置 |
| `interface.displayName` | `"美股综合分析"` — Codex 界面显示名称 |
| `interface.category` | `"Data Analytics"` |
| `interface.capabilities` | 22 项分析能力声明 |
| `interface.defaultPrompt` | 3 条建议提示词，供用户快速开始 |

**.mcp.json 配置的 MCP Server**：

| Server | 用途 | 需要 API Key |
|--------|------|-------------|
| `yahoo-finance` | 实时行情、历史数据、基本面、持仓、期权链 | 否 |
| `finnhub` | 实时行情、SEC文件、内幕交易、ESG | 是（`FINNHUB_API_KEY`） |
| `alpha-vantage` | 行情、技术指标、经济指标 | 是（`ALPHA_VANTAGE_API_KEY`） |

**Codex 专属特性**：
- Codex 自动识别 `SKILL.md` 的 YAML frontmatter（仅 `name`, `description`）
- Skill 的 `agents/openai.yaml` 只提供简短界面元数据、默认提示和工具依赖
- `references/` 下的 8 个 .md 文件按需加载；从 `skills/analyze-us-stocks` 目录加载时使用 `../../references/...`
- 支持 Codex 的多 Agent 协作模式（Workflow/Agent 工具原生兼容 §3 的 4 种模式）
- MCP Server 通过 `.mcp.json` 声明，Codex 自动启动和管理

**与其他平台的区别**：

| 特性 | Claude Code | Codex CLI | OpenAI GPTs |
|------|------------|-----------|-------------|
| 入口文件 | 根目录 `SKILL.md` | `.codex-plugin/plugin.json` | `agents/openai.yaml` |
| Skill 格式 | Claude Code YAML frontmatter | Codex YAML frontmatter | Function Calling YAML |
| MCP 配置 | `mcpServers` in settings | `.mcp.json` at plugin root | 需手动配置 |
| 多 Agent | Workflow 工具 | Workflow 工具 + 原生 Plugin | 手动编排 |
| 安装方式 | `$analyze-us-stocks` 自动 | Plugin marketplace / local path | GPT/Assistant 配置导入 |

**安装到 Codex**：

```bash
# 方式 1: 本地开发安装
codex plugin install ./analyze-us-stocks

# 方式 2: 通过 marketplace.json 注册
# 在 ~/.agents/plugins/marketplace.json 中添加:
{
  "name": "personal",
  "plugins": [
    {
      "name": "analyze-us-stocks",
      "source": { "source": "local", "path": "./plugins/analyze-us-stocks" },
      "policy": { "installation": "AVAILABLE", "authentication": "ON_INSTALL" },
      "category": "Data Analytics"
    }
  ]
}
```

### 1.4 通用 LLM Agent（Prompt 注入模式）

对于没有原生 Skill 支持的 LLM Agent，将核心内容嵌入 System Prompt：

**最小注入（精简版）**：
```
You are a US stock analyst. Follow these rules:
1. NEVER fabricate prices, earnings, or news — use web search for all current data
2. Cross-verify key data points with ≥2 independent sources
3. Assign probabilities (bull/neutral/bear, sum=100%) with evidence for each scenario
4. Score stocks 0-100 across: fundamentals(25), valuation(15), catalysts(20), technicals(15), sentiment(15), macro(10)
5. Identify 3-5 key risks and counterarguments to your main view
6. Output in Chinese by default with structured sections
```

**完整注入**：将 `SKILL.md` + 所有 references 的核心内容作为 System Prompt。

---

## 二、推荐工具链

### 2.1 内置工具（无额外配置）

| 工具 | 用途 | 必要性 |
|------|------|--------|
| **WebSearch** | 实时新闻、行情查询、分析师评级、SEC/Form 4 概览 | **必须** |
| **WebFetch** | SEC EDGAR 文件深度阅读、公司 IR 页面、数据聚合页 | **推荐** |

### 2.2 可选：MCP Server 配置

以下 MCP Server 可以提供更结构化的金融数据，减少对 WebSearch 的依赖：

#### Yahoo Finance MCP (非官方)

```json
{
  "mcpServers": {
    "yahoo-finance": {
      "command": "npx",
      "args": ["-y", "@anthropic/yahoo-finance-mcp"],
      "env": {}
    }
  }
}
```

可获取：实时报价、历史数据、基本面统计、持仓数据、期权链。

#### Finnhub MCP

```json
{
  "mcpServers": {
    "finnhub": {
      "command": "npx",
      "args": ["-y", "finnhub-mcp-server"],
      "env": {
        "FINNHUB_API_KEY": "<your-api-key>"
      }
    }
  }
}
```

可获取：实时报价、公司基本面、SEC 文件、内幕交易、新闻、分析师评级、ESG 评分。

#### Alpha Vantage MCP

```json
{
  "mcpServers": {
    "alpha-vantage": {
      "command": "npx",
      "args": ["-y", "alpha-vantage-mcp-server"],
      "env": {
        "ALPHA_VANTAGE_API_KEY": "<your-api-key>"
      }
    }
  }
}
```

可获取：实时/历史行情、技术指标、外汇/加密货币数据、经济指标。

### 2.3 工具使用优先级

```
优先使用 MCP Server（结构化数据，可靠性高）
  ↓ 如不可用
使用 WebFetch 抓取 Yahoo Finance / Finviz / MarketBeat（结构化抓取）
  ↓ 如不可用
使用 WebSearch 搜索（语言模型理解非结构化结果）
  ↓
标注数据来源等级和获取方式
```

---

## 三、多 Agent 协作模式

### 3.1 并行分析模式（推荐用于标准/深度报告）

适用于 Claude Code 的 Agent / Workflow 功能：

```
主 Agent (Orchestrator)
  ├─ Agent A: 基本面分析
  │   ├─ 收集最新财报、指引
  │   ├─ 分析营收、利润、现金流趋势
  │   └─ 输出：基本面评分 (25/25) + 关键发现
  ├─ Agent B: 市场与情绪分析
  │   ├─ 收集大盘背景、利率、VIX
  │   ├─ 收集分析师评级、空头、期权、内幕交易
  │   └─ 输出：宏观评分 (10/10) + 情绪评分 (15/15) + 关键发现
  ├─ Agent C: 技术与估值分析
  │   ├─ 收集技术面数据、估值倍数
  │   ├─ 收集同行估值对比
  │   └─ 输出：估值评分 (15/15) + 技术评分 (15/15) + 关键发现
  └─ Agent D: 深度维度扫描
      ├─ 收集护城河、管理层、供应链、ESG
      └─ 输出：催化剂评分 (20/20) + 扩展维度发现

主 Agent 汇总 → 一致性校验 → 概率构建 → 生成报告
```

### 3.2 分工-验证模式（推荐用于高风险决策）

```
Phase 1 — 分析:
  Agent X: 完整分析（看多视角） → 输出报告草稿
  Agent Y: 完整分析（看空视角） → 输出反驳清单

Phase 2 — 验证:
  Agent Z (仲裁者):
    - 对比两份分析
    - 识别事实差异（数据分歧）
    - 识别推理差异（相同数据不同解读）
    - 综合裁决，生成最终报告
```

### 3.3 Adversarial Review（挑错模式）

适用于深度研究后的质量把控：

```
分析 Agent: 生成完整报告

挑错 Agent (Devil's Advocate):
  - 检查每个数据点：是否有来源？来源是否可靠？时间是否新鲜？
  - 检查每个推论：逻辑是否合理？是否有反例？
  - 检查概率：是否过度自信？是否有遗漏的风险因素？
  - 输出：错误清单 + 遗漏风险 + 报告质量评级

修正 Agent: 根据挑错结果修改报告
```

### 3.4 多股票对比模式

```
主 Agent:
  ├─ Agent 1: 分析 {TICKER_1}（完整分析）
  ├─ Agent 2: 分析 {TICKER_2}（完整分析）
  ├─ Agent 3: 分析 {TICKER_3}（完整分析）
  └─ Agent 4: 分析 {TICKER_4}（完整分析）

汇总 Agent:
  - 生成对比矩阵（评分、概率、关键指标）
  - 排名推荐
  - 综合分析结论
```

---

## 四、结构化输出 Schema

### 4.1 标准报告 JSON Schema

Agent 在生成报告时，应同时输出以下结构化 JSON（便于下游消费）：

```json
{
  "$schema": "stock-analysis-report-v1",
  "metadata": {
    "ticker": "AAPL",
    "company_name": "Apple Inc.",
    "analysis_date": "2026-06-11",
    "data_as_of": "2026-06-11 14:30 EST",
    "analysis_depth": "deep",
    "horizons": ["short_term_1_4w", "medium_term_1_3m", "long_term_6_12m"],
    "language": "zh-CN"
  },
  "conclusion": {
    "short_term": "bullish",
    "medium_term": "bullish",
    "long_term": "neutral",
    "action": "buy_on_pullback",
    "confidence": "medium",
    "core_reasons": [
      "营收增速超预期，AI驱动新增长曲线",
      "股价回调至技术支撑位",
      "回购持续减少流通股"
    ],
    "data_quality": "partial",
    "data_quality_note": "最新期权流数据未获取"
  },
  "quantitative_score": {
    "total": 72,
    "breakdown": {
      "fundamentals": { "score": 20, "max": 25, "sub_scores": { "revenue_growth": 6, "profitability": 5, "financial_health": 5, "moat_growth_drivers": 4 } },
      "valuation": { "score": 9, "max": 15, "sub_scores": { "absolute": 3, "growth_adjusted": 3, "rate_sensitivity": 3 } },
      "catalysts": { "score": 15, "max": 20, "sub_scores": { "near_term": 6, "quality": 4, "secular_trend": 3, "surprise_potential": 2 } },
      "technicals": { "score": 11, "max": 15, "sub_scores": { "trend": 4, "support_resistance": 4, "volume_price": 3 } },
      "sentiment_positioning": { "score": 10, "max": 15, "sub_scores": { "analyst": 3, "crowding": 3, "insider_buyback": 2, "retail_social": 2 } },
      "macro_regime": { "score": 7, "max": 10, "sub_scores": { "rates_fed": 3, "cycle_position": 2, "sector_style_fit": 2 } }
    }
  },
  "probability": {
    "short_term": { "bullish": 55, "neutral": 30, "bearish": 15 },
    "medium_term": { "bullish": 60, "neutral": 25, "bearish": 15 },
    "long_term": { "bullish": 45, "neutral": 35, "bearish": 20 }
  },
  "key_evidence": {
    "bullish": [
      { "point": "AI产品线2026Q2营收同比增长45%", "source": "A-EDGAR 10-Q 2026Q2", "date": "2026-05-15" },
      { "point": "公司宣布200亿美元新回购计划", "source": "B-Reuters", "date": "2026-06-10" }
    ],
    "bearish": [
      { "point": "中国市场营收同比下降12%，关税风险持续", "source": "A-EDGAR 10-Q 2026Q2", "date": "2026-05-15" },
      { "point": "估值处于历史90分位数", "source": "B-Yahoo Finance", "date": "2026-06-11" }
    ]
  },
  "risks_and_counterarguments": [
    { "risk": "关税升级可能导致供应链成本上升10-15%", "severity": "high", "probability": "medium" },
    { "risk": "AI业务增速可能在下半年放缓", "severity": "medium", "probability": "low" },
    { "risk": "分析师预期过于乐观，存在不及预期的风险", "severity": "medium", "probability": "medium" }
  ],
  "invalidation_signals": [
    "下次财报AI业务增速<30%（当前45%）",
    "中国营收连续2个季度下降>15%",
    "跌破200日移动均线（当前$XX.XX）且3日内未收复",
    "CEO/CFO出现大规模减持"
  ],
  "watchlist": [
    { "event": "Q3财报", "date": "2026-07-28", "importance": "critical" },
    { "event": "FOMC利率决议", "date": "2026-07-29", "importance": "high" },
    { "event": "新产品发布", "date": "2026-09预计", "importance": "high" }
  ],
  "peer_comparison": [
    { "ticker": "MSFT", "score": 74, "valuation_rank": 3, "growth_rank": 1 },
    { "ticker": "GOOGL", "score": 68, "valuation_rank": 1, "growth_rank": 3 },
    { "ticker": "AMZN", "score": 65, "valuation_rank": 2, "growth_rank": 2 }
  ],
  "sources_summary": {
    "total_sources": 14,
    "grade_distribution": { "A": 4, "B": 6, "C": 4, "D": 0 },
    "data_completeness": "partial",
    "missing_data": ["期权流数据", "13F最新持仓"],
    "stale_data": ["空头持仓数据为14天前"],
    "source_conflicts": []
  }
}
```

### 4.2 Schema 字段说明

| 字段组 | 字段 | 类型 | 必填 | 说明 |
|--------|------|------|------|------|
| metadata | ticker | string | 是 | 股票代码 |
| metadata | analysis_date | string | 是 | 分析日期 ISO 格式 |
| metadata | data_as_of | string | 是 | 数据时间戳 |
| metadata | analysis_depth | enum | 是 | quick/standard/deep |
| conclusion | short_term | enum | 是 | bullish/neutral/bearish |
| conclusion | action | enum | 是 | buy/add/hold/watch/reduce/avoid |
| conclusion | confidence | enum | 是 | low/medium/high |
| quantitative_score | total | int | 推荐 | 0-100 |
| quantitative_score | breakdown | object | 推荐 | 6大类得分 |
| probability | {horizon} | object | 是 | bull/neutral/bear 概率，和为100 |
| key_evidence | bullish/bearish | array | 是 | 每项含 point, source, date |
| risks | — | array | 是 | 每项含 risk, severity, probability |
| invalidation_signals | — | array | 是 | 具体数字触发条件 |
| watchlist | — | array | 是 | 每项含 event, date, importance |
| peer_comparison | — | array | 推荐 | 同行评分和排名 |
| sources_summary | — | object | 是 | 数据质量总结 |

---

## 五、错误处理和降级策略

### 5.1 WebSearch 不可用

- **降级**：使用 WebFetch 直接抓取已知 URL（Yahoo Finance、MarketWatch）
- **标注**：在 sources_summary 中说明"WebSearch 不可用，数据采集受限"
- **置信度调整**：自动降低一级

### 5.2 SEC EDGAR 不可用

- **降级**：使用 Yahoo Finance、MarketBeat 等聚合平台作为备用
- **标注**：来源等级从 A 降为 C
- **注意**：聚合平台可能缺少某些数据项（如内幕交易细节）

### 5.3 部分数据缺失

- 按 `data-quality.md` 的数据完整性评级处理
- 缺失维度在报告中明确列出
- 相关评分子项使用最低分或标注为"估计"

### 5.4 跨平台兼容性

- 不同 Agent 平台可能只有 WebSearch 而没有 WebFetch
- 不同平台可能使用不同的工具命名（如 `search` vs `web_search`）
- Agent 应将 `search-queries.md` 中的查询模板适配到具体平台的搜索工具

---

## 六、性能优化建议

### 6.1 缓存策略

- **同一标的/同一天**：缓存行情和基本面查询结果（TTL < 1 天）
- **市场大盘数据**：跨分析任务共享（TTL < 1 小时盘中、< 1 天收盘后）
- **SEC Filing 内容**：缓存到下一季度财报（TTL < 100 天）
- **不缓存**：突发新闻、盘中行情（每次重新获取）

### 6.2 并行化

- P0 查询（行情、财报、新闻、大盘）：**4 个并行 WebSearch**
- P1 查询（分析师、估值、技术面、SEC）：只执行会影响结论的查询，通常 **3-5 个并行 WebSearch**
- P2 查询（扩展维度）：根据材料性选择，最多 **6 个并行 WebSearch**；全部 12 维只在用户明确要求或重大决策时执行
- WebFetch（深度抓取）：在 WebSearch 找到一手来源后按需并行，优先 SEC、公司 IR 和政府/交易所页面
- 停止条件：当关键事实已由 A 级或两个独立来源验证，且新增搜索只重复已有事实时停止

### 6.3 超时处理

- 单个 WebSearch 超时（>30s）：跳过该查询，标注"数据获取超时"
- WebFetch 超时：尝试备选 URL，如仍失败则使用 WebSearch 的摘要
- 超过 3 个 P0 查询超时：降级为"部分缺失"，并在结论中说明
