# 分析维度扩展手册

本手册定义了 12 个扩展分析维度，补充原有 6 类基础证据（基本面、估值、催化剂、技术面、情绪/仓位、宏观/制度）。Agent 应根据任务类型和深度要求，从维度选择矩阵中选择适用的维度。

---

## 维度选择矩阵

| 任务类型 | 必查维度 | 建议维度 | 总维度数 |
|---------|---------|---------|---------|
| **快速扫描** (5-10 min) | 内幕交易(1), 空头持仓(3) | 同行对比概览(11) | 2-3 |
| **标准报告** (15-30 min) | 1,3,5,7,10,11 | 2,4,8 | 6-9 |
| **深度研究** (30+ min) | 全部 12 个维度 | — | 12 |
| **风险审查** | 3,8,9,12 | 1,7 | 4-6 |
| **行业/主题研究** | 7,9,11,12 | 5,6 | 4-6 |
| **财报后评估** | 1,2,5,8,10 | 3,4,11 | 5-8 |
| **技术面交易** | 3,4,6 | 1,5 | 3-5 |

---

## 维度 1：内幕交易分析

### 定义
通过分析公司内部人（高管、董事、大股东）的股票买卖行为，判断管理层对公司前景的实际信心。内幕交易数据来自 SEC Form 4 filings。

### 数据来源
- **主要**：SEC EDGAR Form 4 filings、OpenInsider、InsiderMonkey
- **备用**：Nasdaq.com insider activity、MarketBeat insider trades、Finviz insider
- **WebSearch 查询**：
  - `"{TICKER} insider trading Form 4 buying selling recent"`
  - `"{TICKER} insider buys cluster CEO CFO director"`
  - `"{TICKER} insider sales significant amount last 3 months"`

### 数据解读规则

| 信号 | 判断 | 说明 |
|------|------|------|
| 多位内部人同时买入 | **强看涨** | 群体买入比单笔买入更有说服力 |
| CEO/CFO 大额买入 | **强看涨** | 核心管理层最有信息优势 |
| 低位首次买入 | **看涨** | 历史低位 + 内部人买入形成共振 |
| 偶发性卖出（非规律性） | **中性偏空** | 可能出于个人财务需求，未必是看空 |
| 大规模集群卖出 | **强看空** | 多位内部人在短时间内密集卖出 |
| 内部人卖出占比异常高 | **看空** | 卖出/买入比 > 3:1 需警惕 |
| 长期无内部人交易 | **中性** | 非信号，可能受窗口期限制 |
| 卖出但同时有行权买入 | **中性** | 可能是期权行权后的税务卖出 |

### 权重建议
- 在总体评分中占 **5-10%**
- 与机构持仓变动（维度 2）交叉验证：内部人买入 + 机构增持 = 强确认
- 与基本面（SKILL.md 原有）交叉验证：内部人卖出 + 基本面恶化 = 强警示

### 局限性
- 内部人卖出原因多样（分散投资、税务规划、购房等），不一定看空
- Form 4 有 2 个工作日延迟
- 10b5-1 计划交易不反映即时判断
- 小公司内部人交易信号比大公司更强

---

## 维度 2：机构持仓分析

### 定义
追踪大型机构投资者（对冲基金、共同基金、养老金）的持仓变动，判断"聪明钱"的流向。主要数据来自 13F 季度报告和基金持仓披露。

### 数据来源
- **主要**：SEC 13F filings、WhaleWisdom、Dataroma
- **备用**：Nasdaq.com institutional holdings、Yahoo Finance holders、MarketBeat institutional
- **WebSearch 查询**：
  - `"{TICKER} institutional ownership 13F changes latest quarter"`
  - `"{TICKER} hedge fund buying selling recent"`
  - `"{TICKER} institutional holders percentage float"`

### 数据解读规则

| 信号 | 判断 | 说明 |
|------|------|------|
| 知名基金新建仓/大幅增持 | **看涨** | 关注 Baupost、Pershing Square 等 |
| 机构持股比例持续上升 | **看涨** | 连续 2-3 季度增持更可靠 |
| 多家机构同期减仓 | **看空** | 尤其是长期持有者退出 |
| 机构集中度高 (>70%) | **中性偏多** | 稳定性好但流动性风险大 |
| 机构集中度过低 (<30%) | **中性偏空** | 缺少机构背书 |
| 13F 数据延迟 | **需注意** | 13F 有 45 天延迟，需结合近期新闻判断 |

### 权重建议
- 在总体评分中占 **5-8%**
- 与内部人交易（维度 1）交叉验证
- 高知名度的基金经理操作权重更高

### 局限性
- 13F 不披露空头仓位
- 45 天延迟意味着可能已过时
- 不披露衍生品持仓
- 只反映美国上市股票的多头持仓

---

## 维度 3：空头持仓分析

### 定义
分析做空数据的规模和变化趋势，评估下行压力和潜在轧空风险。

### 数据来源
- **主要**：Exchange short interest reports (NYSE/Nasdaq bi-monthly)、MarketWatch short interest、Shortsqueeze.com
- **备用**：Finviz short float、Yahoo Finance short data、S3 Partners (付费)
- **WebSearch 查询**：
  - `"{TICKER} short interest float percentage days to cover"`
  - `"{TICKER} short squeeze risk high short interest"`
  - `"{TICKER} short interest change trend decreasing increasing"`

### 数据解读规则

| 信号 | 判断 | 说明 |
|------|------|------|
| 空头占比 >20% | **潜在轧空** | 高做空比例 + 任何利好可能触发轧空 |
| 空头占比 10-20% | **警惕** | 市场明显看空，需了解原因 |
| 空头占比 <5% | **中性** | 正常水平 |
| days-to-cover >7 天 | **轧空风险高** | 平仓需要时间，可能放大涨幅 |
| days-to-cover <3 天 | **正常** | 流动性充足 |
| 空头比例下降 | **看涨** | 空方撤退 |
| 空头比例上升 + 价格上涨 | **看涨** | 多方碾压空方（squeeze in progress） |
| 空头比例上升 + 价格下跌 | **看空** | 空方胜出，下行趋势 |

### 权重建议
- 在总体评分中占 **5-10%**
- 与期权流（维度 4）和技术面交叉验证
- 做空报告（如 Hindenburg、Muddy Waters）权重极高

### 局限性
- 交易所每月报告两次，有延迟
- 不代表所有做空活动（如场外衍生品做空）
- 被动做空（ETF 做空）也会计入

---

## 维度 4：期权流分析

### 定义
分析异常期权交易活动，捕捉聪明钱通过期权市场的布局信号。关注大单、异常成交量、Put/Call 比率等指标。

### 数据来源
- **主要**：CBOE data、Barchart unusual options、MarketBeat options、FlowAlgo
- **备用**：Yahoo Finance options chain、Nasdaq options flow
- **WebSearch 查询**：
  - `"{TICKER} unusual options activity call put volume today"`
  - `"{TICKER} options flow large block trades sweeps"`
  - `"{TICKER} put call ratio change trend"`

### 数据解读规则

| 信号 | 判断 | 说明 |
|------|------|------|
| 大量虚值 Call 买入 | **看涨** | 预期大幅上涨 |
| 大量虚值 Put 买入 | **看空** | 对冲或方向性做空 |
| 实值 Call 大单 | **看涨** | 机构建仓信号 |
| Put/Call 比率 < 0.7 | **乐观** | 市场偏多 |
| Put/Call 比率 > 1.2 | **悲观** | 市场偏空 |
| 异常成交量（>5x 均值） | **高度关注** | 可能是知情交易 |
| 价差策略（spread）活跃 | **方向不明但波动预期高** | 用跨式/宽跨式识别 |
| LEAPS 远月 Call 大单 | **中长线看涨** | 长线资金布局 |

### 权重建议
- 在总体评分中占 **5-8%**
- 与内幕交易（维度 1）和机构持仓（维度 2）交叉验证
- 期权数据噪音大，单日异常需结合多日趋势

### 局限性
- 期权交易可能是对冲而非方向性押注
- 无法区分买入和卖出（除非有 flow 数据）
- 短期期权到期日临近时波动加大
- 部分策略（如 covered call）信号方向不直观

---

## 维度 5：因子暴露分析

### 定义
分析股票在主要量化因子上的暴露度，判断在当前市场风格下的适应性。

### 数据来源
- **主要**：Portfolio Visualizer、Yahoo Finance risk metrics、Morningstar
- **备用**：Seeking Alpha factor grades、Koyfin、TradingView
- **WebSearch 查询**：
  - `"{TICKER} factor exposure beta momentum value quality size"`
  - `"{TICKER} stock beta vs S&P 500 correlation"`

### 核心因子与解读

| 因子 | 看涨信号 | 看空信号 |
|------|---------|---------|
| **动量** | 6-12 月正收益，相对强度高 | 持续弱势，创新低 |
| **价值** | 低 PE/PS/FCF 相对行业均值 | 估值溢价不可持续 |
| **质量** | 高 ROE/ROIC、低杠杆、稳定增长 | ROE 下降、负债率上升 |
| **低波动** | 低 beta + 高 Sharpe | 高 beta + 市场下行 |
| **规模** | 中大市值在防御行情中 | 小市值在利率上行周期 |
| **成长** | 高营收/盈利增速 + 大 TAM | 增速放缓、渗透率见顶 |

### 市场风格适配
- **利率上行期**：价值、低波因子上行，成长股承压
- **利率下行期**：成长、动量因子上行
- **经济衰退期**：质量、低波因子防御性强
- **风险偏好期**：动量、成长、小市值跑赢

### 权重建议
- 在总体评分中占 **5-8%**
- 主要作为宏观/风格背景，不单独决策
- 结合市场环境（SKILL.md 原有宏观框架）

---

## 维度 6：季节性分析

### 定义
分析股票和市场在特定时间段的统计表现模式，识别季节性规律。

### 数据来源
- **主要**：Equity Clock、Stock Trader's Almanac 数据
- **备用**：Yahoo Finance historical data、TradingView seasonality
- **WebSearch 查询**：
  - `"{TICKER} seasonal pattern monthly performance history"`
  - `"{TICKER} best worst months historical returns"`
  - `"stock market seasonal patterns {CURRENT_MONTH} {CURRENT_QUARTER}"`

### 数据解读规则

| 模式 | 说明 |
|------|------|
| **月度模式** | 某些月份历史表现持续强势/弱势 |
| **财报季模式** | 财报前后 2 周的超额收益模式 |
| **日历效应** | 月末/季末/年末的窗口效应 |
| **行业季节性** | 零售 Q4 旺季、能源夏季驱动等 |
| **宏观周期** | 选举年、Fed 周期中的阶段表现 |

### 权重建议
- 在总体评分中占 **3-5%**
- 绝不单独依赖——季节性只是统计规律，充满例外
- 用作概率微调因子，不在主报告中强调

### 局限性
- "过去不预示未来" 最适用于季节性
- 小样本问题：个股季节性数据可能只有 10-20 个观察点
- 结构性变化可能使历史模式失效

---

## 维度 7：竞争护城河分析

### 定义
系统评估公司的竞争优势来源和可持续性，判断长期超额收益能力。

### 评估框架（基于 Morningstar 护城河方法论）

| 护城河来源 | 评估指标 | 强护城河信号 |
|-----------|---------|------------|
| **品牌/消费者粘性** | 毛利率稳定、定价权、搜索量 | 提价不影响销量 |
| **转换成本** | 续约率、客户流失率、ARPU 趋势 | 续约率 > 90% |
| **网络效应** | 用户增长、参与度、双边匹配率 | 用户越多价值越大 |
| **成本优势** | 规模/工艺/渠道/位置优势 | 毛利率持续高于同行 |
| **无形资产** | 专利、牌照、监管壁垒、数据 | 专利到期风险远、牌照稀缺 |

### 数据来源
- **主要**：Morningstar moat rating、公司年报/10-K 业务描述部分
- **备用**：行业研究报告、Seeking Alpha 分析文章
- **WebSearch 查询**：
  - `"{TICKER} competitive advantage moat market share industry"`
  - `"{TICKER} vs {PEER} market share comparison competitive position"`
  - `"{TICKER} Morningstar moat rating wide narrow none"`

### 权重建议
- 在总体评分中占 **5-10%**
- 对中长线投资（6 月+）权重更高
- 对短线交易（<4 周）权重降低

---

## 维度 8：管理层质量评估

### 定义
评估管理层的资本配置能力、战略执行记录、利益一致性以及对外沟通质量。

### 评估维度

| 指标 | 看涨信号 | 看空信号 |
|------|---------|---------|
| **资本配置记录** | 高 ROI 并购、时机好的回购 | 破坏价值的并购、盲目扩张 |
| **预测准确性** | 持续达标或超预期 | 频繁下调指引 |
| **内部人持股** | 高管持股比例高 | 频繁套现 |
| **薪酬合理性** | 与 ROIC 挂钩 | 与短期股价过度挂钩 |
| **沟通质量** | 透明坦诚，承认错误 | 回避问题，过度乐观 |
| **任期稳定性** | CFO/CEO 长期稳定 | 频繁更换核心管理层 |

### 数据来源
- **主要**：Earnings call transcripts、Proxy statements (DEF 14A)、Glassdoor
- **备用**：Seeking Alpha transcripts、公司官网投资者关系
- **WebSearch 查询**：
  - `"{TICKER} CEO management track record capital allocation"`
  - `"{TICKER} earnings call tone guidance accuracy history"`
  - `"{TICKER} executive turnover CFO departure"`

### 权重建议
- 在总体评分中占 **5-8%**
- 对困境反转和中小市值公司权重更高
- 与内部人交易（维度 1）和资本配置（维度 10）交叉验证

---

## 维度 9：供应链与地理风险

### 定义
分析公司收入的地域分布和供应链集中度，评估地缘政治、关税、供应链中断风险。

### 评估维度

| 风险点 | 数据需求 | 高风险信号 |
|--------|---------|-----------|
| **中国/亚洲暴露** | 收入占比、生产基地位置 | 制造依赖单一国家 |
| **关税敏感度** | 进口成本占比、可替代性 | 大部分商品依赖进口 |
| **客户集中度** | 前 5 大客户收入占比 | 任一客户 > 15% |
| **供应商集中度** | 关键原材料/零部件来源 | 单一供应商不可替代 |
| **地缘热点** | 俄乌、中东、台海相关暴露 | 直接业务在冲突地区 |
| **汇率敏感度** | 海外收入占比、对冲政策 | 海外收入 > 50% 且不进行对冲 |

### 数据来源
- **主要**：10-K "Risk Factors" 章节、收入分地区 footnote
- **备用**：公司 presentation、Bloomberg supply chain data、Panjiva
- **WebSearch 查询**：
  - `"{TICKER} revenue by geography country exposure tariff risk"`
  - `"{TICKER} supply chain manufacturing location China dependency"`
  - `"{TICKER} top customers concentration risk 10-K"`

### 权重建议
- 在总体评分中占 **5-8%**
- 关税/贸易战时期权重翻倍
- 对制造业/硬件公司权重大于软件/服务公司

---

## 维度 10：资本配置分析

### 定义
评估公司如何分配自由现金流，判断股东回报质量和资本纪律。

### 评估维度

| 配置方式 | 看涨信号 | 看空信号 |
|---------|---------|---------|
| **股票回购** | 低价回购、持续减少股数 | 高价回购、总股数未减（抵消 SBC） |
| **分红** | 持续增长、派息率合理（<60%） | 借债分红、派息率不可持续 |
| **并购** | 小而精的补强收购、高 ROIC | 大额转型收购、商誉激增 |
| **资本支出** | 高回报率项目、有机增长 | 盲目扩张、回报率下降 |
| **研发投入** | 研发效率高（专利/收入比） | 撒胡椒面、缺乏聚焦 |
| **债务管理** | 净债务/EBITDA < 2x | 杠杆过高、再融资风险 |
| **股权融资稀释** | 极少增发、SBC < 3% 营收 | SBC 持续 > 5% 营收 |

### 数据来源
- **主要**：10-K/10-Q 现金流量表、Earnings presentation
- **备用**：Seeking Alpha、公司 Investor Day 材料
- **WebSearch 查询**：
  - `"{TICKER} stock buyback authorization share count reduction"`
  - `"{TICKER} free cash flow capital allocation dividend buyback"`
  - `"{TICKER} share dilution stock based compensation SBC"`

### 权重建议
- 在总体评分中占 **5-10%**
- 与内幕交易（维度 1）和管理层质量（维度 8）交叉验证
- 对成熟公司权重大于成长期公司

---

## 维度 11：同行对比分析

### 定义
系统对比目标公司与核心竞争对手在估值、增长、盈利能力和市场地位上的差异。

### 对比矩阵结构

| 指标 | 目标公司 | 同行1 | 同行2 | 同行3 | 行业均值 |
|------|---------|-------|-------|-------|---------|
| 市值 | | | | | — |
| 营收增速 (YoY%) | | | | | |
| 毛利率 (%) | | | | | |
| 营业利润率 (%) | | | | | |
| ROIC (%) | | | | | |
| FWD P/E | | | | | |
| EV/EBITDA | | | | | |
| PEG Ratio | | | | | |
| FCF Yield (%) | | | | | |
| 净债务/EBITDA | | | | | |
| RSI (14日) | | | | | |
| YTD 涨跌幅 (%) | | | | | |

### 数据来源
- **主要**：Yahoo Finance、Finviz、Koyfin、TIKR
- **备用**：Seeking Alpha peer comparison、Simply Wall St
- **WebSearch 查询**：
  - `"{TICKER} vs {PEER1} {PEER2} comparison valuation P/E PEG growth margins"`
  - `"{TICKER} peers competitors in {SECTOR} industry comparison"`
  - `"{SECTOR} industry average P/E EV/EBITDA profit margins"`

### 解读规则
- **估值低于同行 + 增速高于同行**：最理想的看涨组合
- **估值高于同行 + 增速低于同行**：最危险的看空组合
- **估值溢价但护城河明显更强**：需判断溢价是否合理

### 权重建议
- 在总体评分中占 **8-12%**
- 相对估值是机构投资决策的核心依据
- 同行选择不当会导致错误结论——至少选 2-3 个真正可比的公司

---

## 维度 12：ESG 与重大性因素

### 定义
评估关键 ESG 因素对公司估值和机构资金流向的影响。

### 关键指标

| 领域 | 关键指标 | 数据来源 |
|------|---------|---------|
| **环境** | 碳排放强度、能源转型计划、气候风险暴露 | CDP、公司可持续发展报告 |
| **社会** | 员工满意度、多样性、数据隐私、产品安全 | Glassdoor、公司报告 |
| **治理** | 董事会独立性、股东权利、审计质量 | Proxy Statement、ISS/Glass Lewis |
| **争议** | 正在进行的诉讼、监管行动、丑闻 | 新闻搜索、SEC 8-K |

### 机构流影响
- 越来越多 ESG 基金有排除名单
- MSCI/FTSE ESG 评级下调可能导致被动基金流出
- 重大争议事件可能在短期内驱动主动基金减持

### 数据来源
- **主要**：MSCI ESG、Morningstar Sustainalytics、公司 CSR/ESG 报告
- **备用**：Yahoo Finance ESG score、新闻搜索
- **WebSearch 查询**：
  - `"{TICKER} ESG rating MSCI controversy score"`
  - `"{TICKER} environmental regulatory lawsuit penalty"`
  - `"{TICKER} governance shareholder activist board independence"`

### 权重建议
- 在总体评分中占 **3-5%**（正常时期）、**5-10%**（争议/事件驱动时）
- 主要作为风险调整项，不单独作为买卖理由
- 对欧洲上市的美国股票（ADR）权重更高

---

## 维度交叉验证矩阵

以下表格展示维度之间的交叉验证逻辑，帮助 Agent 识别信号的一致性：

| 维度A | 维度B | 一致看涨 | 一致看空 | 信号冲突时的处理 |
|-------|-------|---------|---------|----------------|
| 1.内幕交易 | 2.机构持仓 | 内部人买入 + 机构增持 = 强看涨 | 内部人卖出 + 机构减持 = 强看空 | 信任内部人信号（更有信息优势） |
| 3.空头持仓 | 4.期权流 | 空头低 + Call 活跃 = 看涨 | 空头高 + Put 活跃 = 看空 | 降低权重，等待更多信息 |
| 5.因子暴露 | 7.护城河 | 质量因子高 + 宽护城河 = 长期看好 | 质量下降 + 护城河收窄 = 结构性恶化 | 护城河变化比因子更根本 |
| 8.管理层 | 10.资本配置 | 管理优秀 + 资本纪律好 = 理想组合 | 管理差 + 随意配置 = 回避 | 重点看资本配置的实际结果 |
| 9.供应链 | 12.ESG | 供应链分散 + ESG 评分高 = 低风险 | 集中度高 + 争议 = 高风险 | 供应链风险更具体，权重更高 |

---

## 使用说明

1. **任务开始时**：根据任务类型，从维度选择矩阵中确定必查维度
2. **数据采集时**：使用每个维度的 WebSearch 查询模板进行并行搜索
3. **分析时**：将维度分析结果填入对应的证据类别中
4. **交叉验证时**：使用交叉验证矩阵检查信号一致性
5. **最终评分**：将维度结论作为 `scoring-model.md` 评分模型的输入
