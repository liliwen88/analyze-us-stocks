# 搜索查询模板库

本文档提供预构建的搜索查询模板，Agent 可直接将 `{TICKER}`、`{SECTOR}`、`{DATE}` 替换后执行 WebSearch。中英文双模板确保在不同搜索环境下都能获得最佳结果。

---

## 一、查询优先级规则

| 优先级 | 标签 | 含义 | 何时执行 |
|--------|------|------|---------|
| **P0** | 必须 | 不执行则无法完成分析 | 所有任务都必须执行 |
| **P1** | 核心 | 标准报告的核心数据 | 标准报告和深度研究必须执行 |
| **P2** | 扩展 | 提升全面性 | 深度研究和风险审查时执行 |
| **P3** | 可选 | 锦上添花 | 仅在特定情境下执行 |

---

## 二、P0 — 必须执行的查询

### 行情与基本市场数据

```
# EN
"{TICKER} stock price today market cap volume"
"{TICKER} 52 week high low current price performance"

# 中文
"{TICKER} 股票 今日行情 市值 成交量"
"{TICKER} 股价 52周高低 近期表现"
```

### 最新基本面

```
# EN
"{TICKER} latest quarterly earnings results revenue EPS Q{1-4} {YEAR}"
"{TICKER} earnings guidance outlook forecast {YEAR}"

# 中文
"{TICKER} 最新季度财报 营收 每股收益"
"{TICKER} 业绩指引 展望 {YEAR}"
```

### 最新新闻

```
# EN
"{TICKER} news today latest developments"
"{TICKER} breaking news {CURRENT_MONTH} {YEAR}"

# 中文
"{TICKER} 最新消息 新闻 动态 {CURRENT_MONTH}"
"{TICKER} 重大事件 公告"
```

### 市场背景

```
# EN
"US stock market today S&P 500 Nasdaq Dow"
"VIX index Treasury yield 10 year US dollar index today"
"Federal Reserve interest rate outlook {CURRENT_MONTH} {YEAR}"

# 中文
"美股大盘 标普500 纳斯达克 {CURRENT_MONTH}"
"恐慌指数 VIX 美债收益率 美元指数 今日"
"美联储 利率前景 {YEAR}年"
```

---

## 三、P1 — 核心维度查询

### 估值与同行

```
# EN
"{TICKER} P/E ratio PEG EV/EBITDA valuation vs industry historical average"
"{TICKER} vs {PEER1} {PEER2} comparison valuation growth margins market cap"
"{SECTOR} industry average P/E ratio profit margins growth rate"

# 中文
"{TICKER} 市盈率 PEG 估值 对比行业历史均值"
"{TICKER} 对比 {PEER1} {PEER2} 估值 增长 利润率"
"{SECTOR} 行业 平均市盈率 利润率 增长率"
```

### 分析师与评级

```
# EN
"{TICKER} analyst rating consensus target price upgrade downgrade"
"{TICKER} analyst estimate revision earnings revenue trend"

# 中文
"{TICKER} 分析师评级 共识 目标价 上调 下调"
"{TICKER} 分析师盈利预测 调整趋势"
```

### 技术面

```
# EN
"{TICKER} technical analysis trend support resistance moving average"
"{TICKER} RSI MACD volume analysis chart pattern"

# 中文
"{TICKER} 技术分析 趋势 支撑位 阻力位 均线"
"{TICKER} RSI MACD 成交量 技术形态"
```

### SEC Filing

```
# EN
"{TICKER} SEC filing 10-K 10-Q annual report latest"
"{TICKER} 8-K filing material event recent"

# 中文
"{TICKER} SEC 年报 季报 10-K 10-Q 最新"
"{TICKER} 重大事项 8-K 披露"
```

---

## 四、P2 — 扩展维度查询

### 内幕交易 (维度 1)

```
# EN
"{TICKER} insider trading insider buys sells Form 4 recent"
"{TICKER} CEO CFO director stock purchase sale transaction"
"{TICKER} insider trading cluster buying activity last 6 months"

# 中文
"{TICKER} 内部人交易 高管买卖 最新"
"{TICKER} 内部人 集体买入 增持"
```

### 机构持仓 (维度 2)

```
# EN
"{TICKER} institutional ownership percentage 13F latest quarter"
"{TICKER} hedge fund buying selling position change"
"{TICKER} major shareholders institutional holders top"

# 中文
"{TICKER} 机构持仓 13F 最新季度变动"
"{TICKER} 对冲基金 持仓变化 买入卖出"
```

### 空头持仓 (维度 3)

```
# EN
"{TICKER} short interest float percentage days to cover latest"
"{TICKER} short squeeze risk high short interest trend"
"{TICKER} short seller report thesis bear case"

# 中文
"{TICKER} 空头持仓 做空比例 days-to-cover"
"{TICKER} 轧空风险 空头趋势"
```

### 期权流 (维度 4)

```
# EN
"{TICKER} unusual options activity call put volume flow today"
"{TICKER} options open interest change put call ratio"
"{TICKER} options block trade sweeps large orders"

# 中文
"{TICKER} 异常期权交易 大单 call put 活跃"
"{TICKER} 期权持仓变化 put call比率"
```

### 竞争护城河 (维度 7)

```
# EN
"{TICKER} competitive advantage moat market share industry position"
"{TICKER} Morningstar moat rating wide narrow"
"{TICKER} vs competitors competitive analysis Porter five forces"

# 中文
"{TICKER} 竞争优势 护城河 市场份额"
"{TICKER} 行业地位 竞争格局 对比分析"
```

### 管理层质量 (维度 8)

```
# EN
"{TICKER} CEO management team track record performance review"
"{TICKER} management capital allocation history shareholder value"
"{TICKER} executive turnover CFO departure CEO change"

# 中文
"{TICKER} 管理层 团队 业绩记录 评价"
"{TICKER} 高管变动 离职 CEO CFO"
```

### 供应链/地理风险 (维度 9)

```
# EN
"{TICKER} revenue by geography country exposure China tariff risk"
"{TICKER} supply chain manufacturing location dependency concentration"
"{TICKER} top customers concentration risk revenue dependency"

# 中文
"{TICKER} 收入地区分布 中国依赖 关税风险"
"{TICKER} 供应链 制造基地 集中度"
"{TICKER} 大客户集中度 收入依赖"
```

### 资本配置 (维度 10)

```
# EN
"{TICKER} stock buyback share repurchase authorization program"
"{TICKER} dividend history growth payout ratio sustainability"
"{TICKER} share dilution stock based compensation SBC impact"
"{TICKER} free cash flow capital allocation debt reduction M&A"

# 中文
"{TICKER} 股票回购 回购计划 股份减少"
"{TICKER} 分红历史 派息率 可持续性"
"{TICKER} 股权稀释 股票薪酬 SBC"
```

### 同行对比 (维度 11)

```
# EN
"{TICKER} peer comparison table valuation growth profitability {PEER1} {PEER2}"
"{TICKER} market share vs {PEER1} {PEER2} {PEER3} competitive position"

# 中文
"{TICKER} 同行对比表 估值 增长 盈利能力 {PEER1} {PEER2}"
```

### ESG (维度 12)

```
# EN
"{TICKER} ESG rating MSCI Sustainalytics controversy score"
"{TICKER} environmental regulatory investigation lawsuit penalty"
"{TICKER} governance shareholder activism proxy fight board independence"

# 中文
"{TICKER} ESG评级 争议事件 评分"
"{TICKER} 环境诉讼 监管调查 罚款"
```

### 季节性 (维度 6)

```
# EN
"{TICKER} historical performance by month seasonality pattern"
"{TICKER} best worst months quarterly seasonal trend"
"stock market seasonality {CURRENT_MONTH} historical pattern"

# 中文
"{TICKER} 历史月度表现 季节性规律"
```

---

## 五、P3 — 情境查询（按需执行）

### 财报季专用

```
# EN
"{TICKER} earnings preview estimates expectations whisper number"
"{TICKER} earnings call transcript key takeaways highlights"
"{TICKER} earnings surprise history beat miss pattern"

# 中文
"{TICKER} 财报预览 预期 市场共识"
"{TICKER} 财报电话会议 要点 关键信息"
```

### 并购/重组情境

```
# EN
"{TICKER} merger acquisition rumor target buyer deal"
"{TICKER} spin off split restructuring activist investor"
"{TICKER} takeover premium valuation comparable transactions"

# 中文
"{TICKER} 并购 收购 传闻 交易"
"{TICKER} 分拆 重组 激进投资者"
```

### 监管/政策情境

```
# EN
"{TICKER} FDA approval clinical trial results PDUFA date"
"{TICKER} FTC antitrust investigation regulatory risk"
"{TICKER} government contract policy change impact"

# 中文
"{TICKER} 监管审批 FDA 临床试验"
"{TICKER} 反垄断 监管调查 政策风险"
```

### 宏观事件驱动

```
# EN
"CPI inflation report {DATE} expectation impact stock market"
"FOMC meeting Fed decision rate cut hike {DATE} impact {SECTOR}"
"jobs report unemployment claims economic data {DATE}"

# 中文
"CPI 通胀数据 {DATE} 预期 美股影响"
"美联储 利率决议 FOMC {DATE} 影响 {SECTOR}"
"非农就业 失业 经济数据 {DATE}"
```

---

## 六、并行查询策略

为了最大化效率，Agent 应将查询按以下方式分组并行执行：

### 第 1 轮（并行，P0 查询）

```
WebSearch: "{TICKER} stock price today market cap"
WebSearch: "{TICKER} latest quarterly earnings results Q{1-4}"
WebSearch: "{TICKER} news today latest developments"
WebSearch: "US stock market S&P 500 VIX Treasury yields today"
```

### 第 2 轮（并行，P1 查询，在第 1 轮完成后）

```
WebSearch: "{TICKER} analyst rating consensus target price"
WebSearch: "{TICKER} P/E PEG valuation vs industry"
WebSearch: "{TICKER} SEC filing 10-K 10-Q latest"
WebSearch: "{TICKER} technical analysis trend support resistance"
WebSearch: "{TICKER} vs {PEER1} {PEER2} comparison valuation"
```

### 第 3 轮（并行，P2 查询，按需执行）

```
WebSearch: "{TICKER} insider trading Form 4 recent"
WebSearch: "{TICKER} short interest float percentage"
WebSearch: "{TICKER} institutional ownership 13F"
WebSearch: "{TICKER} unusual options activity"
WebSearch: "{TICKER} competitive advantage moat market share"
WebSearch: "{TICKER} stock buyback share repurchase dilution"
WebSearch: "{TICKER} supply chain China exposure tariff risk"
WebSearch: "{TICKER} ESG rating controversy"
```

### 第 4 轮（补充，基于前几轮发现的数据缺口）

在第 3 轮完成后，根据已获取数据的完整性判断是否需要补充查询。

---

## 七、WebFetch 深度抓取

以下 URL 模式适合使用 WebFetch 进行深度内容提取（而非 WebSearch）：

### SEC EDGAR 直达链接
```
https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={CIK}&type=10-K
https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={CIK}&type=10-Q
https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={CIK}&type=8-K
https://www.sec.gov/cgi-bin/own-disp?action=getowner&CIK={CIK}&type=4
```

### 公司 IR 页面
```
https://investor.{COMPANY}.com/
https://investor.{COMPANY}.com/financial-information/quarterly-results
https://investor.{COMPANY}.com/news-events/press-releases
```

### 数据聚合页面
```
https://finance.yahoo.com/quote/{TICKER}/
https://finance.yahoo.com/quote/{TICKER}/key-statistics
https://finance.yahoo.com/quote/{TICKER}/analysis
https://finviz.com/quote.ashx?t={TICKER}
https://www.marketbeat.com/stocks/NASDAQ/{TICKER}/
```

---

## 八、查询执行检查清单

Agent 在数据采集完成后，确认以下查询已覆盖：

```
□ P0: 行情数据 (价格、市值、52周高低)
□ P0: 最新季度财报 (营收、EPS)
□ P0: 最新新闻/催化剂
□ P0: 大盘背景 (SPX、VIX、债券、美元)
□ P1: 分析师评级/目标价
□ P1: 估值数据 (PE、PEG、EV/EBITDA)
□ P1: 技术面分析
□ P2: (按维度选择矩阵执行)
□ P2: 同行对比数据
□ 数据时间戳已记录
```
