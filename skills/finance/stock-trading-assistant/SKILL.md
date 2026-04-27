---
name: stock-trading-assistant
description: "Hermes-Skill-Stock-Trading-Assistant - 智能股票交易助手，基于同花顺/东方财富等数据源，提供技术分析、量化策略回测、风险管理和模拟盘交易功能。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [stock, trading, finance, quantitative, technical-analysis, risk-management, portfolio, a-shares]
    related_skills: [finance-tracker, data-visualization-generator, data-pipeline-orchestrator]
    requires_toolsets: [web, terminal]
    config:
      - key: data_source
        default: ths  # ths/eastmoney/tushare
      - key: risk_level
        default: moderate
      - key: initial_capital
        default: 100000
---

# Hermes-Skill-Stock-Trading-Assistant (智能股票交易助手)

## 概述

**Hermes-Skill-Stock-Trading-Assistant** 是一个专业的 A 股智能交易辅助系统，集成技术分析、量化策略、风险控制和模拟交易全流程。支持同花顺/东方财富数据接口，帮助投资者做出更理性的投资决策。

### 核心能力

- **实时行情**: A股/港股/美股实时行情与历史数据
- **技术指标**: 50+ 种技术指标（MA/MACD/KDJ/RSI/BOLL 等）
- **量化策略**: 策略编写、回测、参数优化
- **风险管理**: 仓位控制、止损止盈、VaR 计算
- **模拟盘**: 虚拟资金交易，零成本验证策略
- **智能预警**: 价格/成交量/技术形态条件触发通知

---

## 快速开始

```bash
# 初始化交易系统
/trader init --capital 100000 --risk-level moderate --market A-share

# 查看实时行情
/trader quote 000001.SZ 600519.SH
/trader watchlist add 300750.SZ 002594.SZ

# 技术分析
/trader analyze technical 300750.SZ --indicators MA,MACD,KDJ,RSI,BOLL,VOL
/trader analyze pattern 300750.SZ --detect head_shoulders,double_bottom

# 运行策略回测
/trader backtest strategy/dual_ma.py --start 2025-01-01 --end 2026-04-27 --benchmark SH000001

# 风险评估
/trader risk assess --portfolio my_portfolio.json --var-confidence 95%

# 模拟交易
/trader simulate buy 300750.SZ --amount 50000 --reason "突破均线系统"
/trader simulate sell 002594.SZ --position all --reason "触及止损线"
/trader pnl report
```

---

## 技术分析模块

### 核心指标体系

```
趋势指标:
├── 移动平均线 (MA/SMA/EMA/WMA) - 5/10/20/60/120/250日
├── MACD (指数平滑异同移动平均线)
├── BOLL (布林带)
├── DMI (趋向指标)
└── SAR (抛物线转向)

动量指标:
├── RSI (相对强弱指数) - 6/12/24日
├── KDJ (随机指标)
├── CCI (顺势指标)
├── ROC (变动率)
└── Williams %R

量价指标:
├── VOL (成交量) / AMO (成交额)
├── OBV (能量潮)
├── EMV (简易波动指标)
├── VRSI (量相对强弱)
└── VP (成交量分布)

波动性指标:
├── ATR (真实波幅)
├── 布林带宽 (Bollinger Bandwidth)
├── Keltner Channel
└── Historical Volatility
```

### 图表形态识别

```
反转形态:
├── 头肩顶/底 (Head & Shoulders)
├── 双顶/底 (Double Top/Bottom)
├── 三重顶/底 (Triple Top/Bottom)
├── V形反转 (V Pattern)
└── 圆弧顶/底 (Rounding)

持续形态:
├── 上升/下降三角形
├── 旗形/楔旗形 (Flag/Pennant)
├── 矩形整理 (Rectangle)
├── 上升/下降通道
└── 杯柄形态 (Cup & Handle)
```

### 智能信号系统

```yaml
signal_engine:
  multi_timeframe_analysis:
    - frame: daily    # 日线判断趋势
    - frame: 60min    # 60分钟找入场点
    - frame: 15min    # 15分钟精确执行
      
  signal_combination:
    buy_signals_required: 2+   # 至少2个买入信号共振
    sell_signals_required: 1+  # 1个卖出信号即可
    
  signal_types:
    trend_following:
      - MA_Golden_Cross     # 金叉
      - MACD_Divergence     # 底背离
      - Price_Above_MA20    # 站稳20日线
      
    mean_reversion:
      - RSI_Oversold < 30    # 超卖区
      - BOLL_Lower_Band     # 触及下轨
      - KDJ_J < 0           # J值转正
      
    volume_confirm:
      - Volume_Ratio > 1.5   # 放量确认
      - OBV_New_High         # 能量新高
```

---

## 量化策略框架

### 策略模板

```python
# strategies/dual_ma_cross.py
class DualMAStrategy(StrategyBase):
    """双均线交叉策略"""
    
    params = {
        'fast_period': 10,
        'slow_period': 30,
        'stop_loss_pct': 0.05,
        'take_profit_pct': 0.15,
        'max_position_pct': 0.3
    }
    
    def on_bar(self, bar):
        fast_ma = self.sma(bar.close, self.params.fast_period)
        slow_ma = self.sma(bar.close, self.params.slow_period)
        
        # 金叉买入
        if self.cross_above(fast_ma, slow_ma):
            if not self.has_position(symbol):
                position_size = self.calc_position_size(
                    capital=self.portfolio.cash * self.params.max_position_pct,
                    price=bar.close,
                    stop_loss=bar.close * (1 - self.params.stop_loss_pct)
                )
                self.buy(symbol, position_size, reason="Golden Cross")
                
        # 死叉卖出
        elif self.cross_below(fast_ma, slow_ma):
            if self.has_position(symbol):
                self.sell(symbol, reason="Death Cross")
                
        # 止损检查
        self.check_stop_loss(symbol, bar.close, self.params.stop_loss_pct)
        # 止盈检查
        self.check_take_profit(symbol, bar.close, self.params.take_profit_pct)
```

### 内置策略库

| 策略名称 | 类型 | 适用市场 | 年化收益参考 |
|---------|------|---------|------------|
| **Dual MA Cross** | 趋势跟踪 | 趋势市 | 15-25% |
| **MACD Divergence** | 动量反转 | 震荡市 | 12-20% |
| **Bollinger Squeeze** | 波动率突破 | 所有市场 | 18-30% |
| **RSI Mean Reversion** | 均值回归 | 震荡市 | 10-18% |
| **Volume Breakout** | 量价配合 | 突破行情 | 20-35% |
| **Multi-Factor Score** | 多因子综合 | 所有市场 | 22-40% |

### 回测引擎

```bash
# 完整回测
/trader backtest \
  --strategy dual_ma \
  --symbols 300750.SZ,002594.SZ,600519.SH \
  --start 2023-01-01 \
  --end 2026-04-27 \
  --initial-capital 100000 \
  --commission 0.00025 \
  --slippage 0.001 \
  --benchmark SH000001 \
  --output detailed_report.html

# 参数优化
/trader optimize \
  --strategy dual_ma \
  --params fast_period:[5,20],slow_period:[20,60] \
  --metric sharpe_ratio \
  --method grid_search \
  --folds 5
```

**回测报告包含:**
- 收益曲线 vs 基准
- 最大回撤 (Max Drawdown)
- 夏普比率 (Sharpe Ratio)
- 胜率和盈亏比
- 月度收益热力图
- 仓位变化图
- 交易明细日志

---

## 风险管理

### 仓位控制

```yaml
risk_management:
  # 单笔最大仓位
  max_single_position: 0.20  # 单只股票不超过总资产20%
  
  # 同行业最大仓位
  max_sector_exposure: 0.35  # 单行业不超过35%
  
  # 总仓位上限
  max_total_exposure: 0.95  # 保留5%现金
  
  # 凯利公式动态调整
  kelly_fraction: 0.25  # 使用Kelly公式的1/4（半Kelly）
  
  # ATR仓位计算
  position_sizing:
    method: atr_risk
    risk_per_trade: 0.02  # 每笔交易最大亏损2%
```

### 止损止盈系统

```
止损类型:
├── 固定百分比止损: -5% / -8% / -10%
├── ATR trailing stop: 价格 - 2*ATR（自适应）
├── 前高/前低止损: 跌破近期支撑位
├── 时间止损: 持有N天未达目标则退出
└── 波动率止损: 波动率异常放大时减仓

止盈类型:
├── 固定百分比止盈: +15% / +20% / +30%
├── 分批止盈: +10%卖30%, +20%再卖30%, 余下跟踪止损
├── 移动止盈: 从最高点回落X%时卖出
└── 目价位止盈: 到达预判目标位
```

### VaR 风险价值

```bash
# 计算组合 VaR (95%置信度，1日持有期)
/trader var calculate --portfolio current.json --confidence 95 --horizon 1d

# 输出示例:
# VaR (95%, 1D): ¥12,350 (即95%概率日内亏损不超过¥12,350)
# Expected Shortfall (CVaR): ¥18,200
# Stress Test Loss: ¥45,000 (极端情景)
```

---

## 数据源配置

### 同花顺 (iFinD)

```python
# config/data_sources.py
THS_CONFIG = {
    "api_base": "https://basic.10jqka.com.cn",
    "fields": {
        "realtime": ["code", "name", "price", "change_pct", "volume", "turnover", "high", "low", "open"],
        "kline": ["date", "open", "high", "low", "close", "volume", "amount"],
        "financial": ["revenue", "net_profit", "pe_ratio", "pb_ratio", "roe"]
    },
    "rate_limit": 100  # requests per minute
}
```

### 东方财富

```python
EASTMONEY_CONFIG = {
    "api_base": "push2.eastmoney.com/api/qt",
    "support_markets": ["SH", "SZ", "HK", "US"],
    "features": [
        "level2_quote",       # 逐档行情
        "fund_flow",          # 资金流向
        "institutional_trade"  # 龙虎榜
    ]
}
```

---

## 模拟盘交易

### 交易记录

```yaml
模拟账户信息:
  初始资金: ¥100,000.00
  当前净值: ¥127,350.00 (+27.35%)
  可用现金: ¥42,180.00
  持仓市值: ¥85,170.00
  
持仓明细:
  ┌────────┬──────────┬───────┬────────┬─────────┬────────┐
  │ 代码    │ 名称      │ 数量  │ 成本价  │ 现价    │ 盈亏%   │
  ├────────┼──────────┼───────┼────────┼─────────┼────────┤
  │ 300750 │ 宁德时代  │ 200   │ 218.50 │ 245.80  │ +12.5% │
  │ 002594 │ 比亚迪    │ 150   │ 256.00 │ 278.30  │ +8.7%  │
  │ 600519 │ 贵州茅台  │ 10    │ 1680   │ 1725.00 │ +2.7%  │
  └────────┴──────────┴───────┴────────┴─────────┴────────┘
  
今日交易:
  [09:32] 买 入 300750 × 200股 @ 218.50 → 突破20日线放量
  [13:45] 卖 出 000858 × 500股 @ 12.30 → 触发8%止盈
```

### 绩效仪表盘

```
📊 模拟盘绩效报告 (2026-01-01 ~ 2026-04-27)

总收益率:    ████████████████░░░░ +27.35%
年化收益率:  +89.2%
最大回撤:    -8.3% (发生在 2026-03-08)
夏普比率:    1.85
胜率:        62.5% (40胜/24负)
盈亏比:      2.34
月均交易次数: 12.3

对比基准:
├── 上证指数:  +5.8%
├── 沪深300:   +8.2%
└── 创业板指:  +3.1%
★ 超额收益:   +21.55% 🎉
```

---

## 最佳实践与警告

### ✅ 使用原则

1. **不构成投资建议**: 本工具为辅助决策，不替代独立判断
2. **先模拟后实盘**: 任何策略先在模拟盘运行 ≥3个月
3. **分散投资**: 单只仓位 ≤ 20%，行业 ≤ 35%
4. **纪律执行**: 严格执行止损，不抱侥幸心理
5. **持续学习**: 定期复盘交易记录，优化策略

### ⚠️ 风险提示

> **股市有风险，投资需谨慎。**  
> 过往业绩不代表未来表现。  
> 量化策略存在过拟合风险。  
> 模拟盘收益不等于实盘收益（考虑滑点/冲击成本/心理因素）。

---

*创新技能 - 基于 A 股实战需求设计，集成技术分析与量化交易*
*版本: 2.0.0 | 最后更新: 2026-04-27*
