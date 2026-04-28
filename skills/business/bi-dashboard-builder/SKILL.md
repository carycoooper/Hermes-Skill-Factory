---
name: bi-dashboard-builder
description: "Hermes-Skill-BI-Dashboard-Builder - 商业智能仪表盘构建器，支持KPI设计、数据建模、可视化图表选择、交互式报表生成和多端适配方案。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [bi, dashboard, kpi, data-visualization, business-intelligence, metrics, reporting, analytics, tableau, powerbi]
    related_skills: [data-visualization-generator, data-pipeline-orchestrator, finance-tracker, competitive-intelligence]
    requires_toolsets: [web, terminal]
    config:
      - key: dashboard_type
        default: executive
      - key: data_source
        default: sql_database
---

# Hermes-Skill-BI-Dashboard-Builder (商业智能仪表盘构建器)

## 概述

**Hermes-Skill-BI-Dashboard-Builder** 是一个专业的 BI 仪表盘设计和实现系统。它帮助企业从原始数据到可决策的视觉洞察，覆盖指标定义、数据建模、可视化设计和交互开发的完整链路。

### 核心能力

- **KPI 设计框架**: 从业务目标到可衡量指标的转化方法论
- **数据建模**: Star Schema / Snowflake Schema 设计
- **仪表盘布局**: Executive / Operational / Analytical 三种模式
- **可视化选型**: 30+ 图表类型的最佳实践匹配
- **交互设计**: Drill-down / Filter / Cross-highlight / Tooltip
- **性能优化**: 大数据量下的查询优化和渲染策略

---

## KPI 设计体系

### 业务目标 → KPI 转化框架

```
Level 1: Business Objective (业务目标)
└── 例: "提升客户满意度和留存率"

Level 2: Critical Success Factor (关键成功因素)
    ├── CSF 1: "缩短客户问题响应时间"
    ├── CSF 2: "提高首次解决率"
    └── CSF 3: "增加客户触达频率"

Level 3: Key Performance Indicator (关键绩效指标)
    ├── KPI 1.1: "平均响应时间 (ART)" — 单位: 分钟
    │   ├── Definition: 从客户提交工单到首次回复的时间差
    │   ├── Target: ≤ 30 分钟
    │   ├── Data Source: Zendesk API / 工单系统DB
    │   └── Calculation: AVG(first_reply_time - created_at)
    │
    ├── KPI 1.2: "SLA 达标率" — 单位: %
    │   ├── Definition: 在承诺时间内响应的工单占比
    │   ├── Target: ≥ 95%
    │   └── Formula: COUNT(on_time) / COUNT(total) × 100
    │
    ├── KPI 2.1: "首次解决率 (FCR)" — 单位: %
    │   ├── Definition: 无需二次联系即可解决的工单占比
    │   ├── Target: ≥ 70%
    │   └── ...
    │
    └── KPI 3.1: "NPS 净推荐值" — 单位: 分 (-100~+100)
        ├── Definition: 客户推荐意愿调查得分
        ├── Target: ≥ 50
        └── Survey Source: Post-interaction survey

Level 4: Metric (度量/衍生指标)
    └── 用于计算 KPI 的原子数据点
```

### KPI 卡片设计规范

```
┌─────────────────────────────────────────┐
│  📈 Monthly Active Users               │
│                                         │
│  ████████████████████░░░░░ 127,450     │
│                                         │
│  ↑ 12.5% vs last month                  │
│  🟢 Above target (100K)                │
│                                         │
│  Target: 150K by Q4                    │
│  Progress: ████████░░░░░ 65%           │
│                                         │
│  ── Trend (6 months) ──────────────    │
│     ╱╲  ╱╲  ╱╲                        │
│                                         │
└─────────────────────────────────────────┘

KPI Card 必须包含元素:
├── 指标名称 + 图标
├── 当前值 (大字体突出显示)
├── 趋势方向 + 变化百分比
├── 与目标的对比状态 (红/黄/绿)
├── 进度条 (如有目标值)
├── 趋势迷你图 (Sparkline)
└── 时间范围标注
```

---

## 仪表盘类型

### Type 1: Executive Dashboard (高管仪表盘)

```
受众: C-Level / VP / Board Members
刷新频率: Daily / Weekly
核心原则: "一页看完全局，不超过 10 秒"

布局建议 (1920×1080):
┌────────────────┬────────────────┬────────────────┐
│ Revenue MRR   │ Active Users   │ NPS Score     │
│ $1.2M (+15%)  │ 45K (+8%)      │ 52 (+3)      │
├────────────────┴────────────────┴────────────────┤
│              Revenue Trend (Area Chart)          │
│              Last 12 Months                          │
├──────────────┬───────────────┬───────────────────┤
│ By Region    │ By Product    │ Top Opportunities │
│ (Map/Pie)    │ (Bar)         │ (Table)           │
├──────────────┴───────────────┴───────────────────┤
│              Team Performance (Rank Table)          │
│              Top 5 / Bottom 5                       │
└──────────────────────────────────────────────────┘

设计原则:
✅ 最多 5-7 个 KPI (信息过载 = 不看)
✅ 绿/黄/红 交通灯颜色编码
✅ 趋势比绝对值更重要
✅ 可点击 drill down 到详情
❌ 不要放原始数据表
❌ 不要放超过 2 层的钻取
```

### Type 2: Operational Dashboard (运营仪表盘)

```
受众: Manager / Team Lead / Analyst
刷新频率: Real-time / Hourly
核心原则: "发现问题 → 快速定位 → 立即行动"

布局建议:
┌──────────────────────────────────────────────────┐
│  🔍 Filters: Date Range | Region | Product | Status │
├──────────┬──────────┬──────────┬───────────────────┤
│ Pipeline │ Win Rate │ Deal Size│ Sales Activity   │
│ (Funnel) │ (Gauge)  │ (Trend)  │ (Heatmap/Calendar)│
├──────────┼──────────┼──────────┼───────────────────┤
│          │          │          │                   │
│ Detail   │ Detail   │ Detail   │ Detail View       │
│ Table    │ Table    │ Table    │ (with actions)    │
│ (Sortable│(Sortable│(Sortable│                   │
│ Filter)  │ Filter)  │ Filter)  │                   │
└──────────┴──────────┴──────────┴───────────────────┘

设计原则:
✅ 强大的筛选和过滤功能
✅ 数据表格可排序/搜索/导出
✅ 异常值自动高亮
✅ 操作按钮直接嵌入 (如 "Call Customer")
✅ 实时数据刷新指示器
```

### Type 3: Analytical Dashboard (分析仪表盘)

```
受众: Data Analyst / Data Scientist / Product Manager
刷新频率: On-demand / Ad-hoc
核心原则: "探索性分析，支持自由组合维度"

核心功能:
├── 多维分析 (OLAP Cube-style)
│   ├── Drag-and-drop 维度到行/列
│   ├── 动态聚合 (SUM/AVG/COUNT/DISTINCT)
│   └── Slice & Dice
│
├── 相关性分析
│   ├── Scatter plot with regression line
│   ├── Correlation matrix heatmap
│   └── Causation hints (not just correlation)
│
├── Cohort Analysis
│   ├── Retention cohort (用户留存)
│   ├── Revenue cohort (收入留存)
│   └── Feature adoption cohort
│
├── Funnel Analysis
│   ├── Conversion funnel with drop-off rates
│   ├── A/B test comparison
│   └── Time-to-convert analysis
│
└── Anomaly Detection
    ├── Statistical outlier detection
    ├── Forecast vs Actual variance
    └── Automated insight generation
```

---

## 可视化选型指南

| 你想展示什么 | 推荐图表 | 备选 |
|-------------|---------|------|
| **部分占整体** | 饼图 (<5类) / 环形图 (>5类) | 百分比堆叠柱状图 |
| **趋势变化** | 折线图 (时间序列) | 面积图 (体量) |
| **类别比较** | 柱状图 (横向/纵向) | 径向柱状图 (多类别) |
| **分布情况** | 直方图 | 箱线图 / 小提琴图 |
| **相关性** | 散点图 | 气泡图 (加第三维度) |
| **地理分布** | 地图 (Choropleth) | 点状地图 |
| **流程/转化** | 漏斗图 | 桑基图 |
| **层级结构** | 树状图 | 旭日图 / 矩形树图 |
| **实时监控** | 单数字 KPI + Sparkline | 仪表盘 Gauge |
| **对比差异** | 蛛形图 | 发散图 (Diverging Bar) |
| **复杂关系** | 网络图 / Sankey | 平行坐标图 |

### 配色最佳实践

```
顺序色阶 (Sequential):
├── 单一色深浅: 蓝(浅→深) 表示数值低→高
├── 适用: 热力图 / 地图 / 密度图
└── 推荐: ColorBrewer Blues/Greens/Oranges

发散色阶 (Diverging):
├── 中间为中性色, 两端为对比色
├── 适用: 正负值 / 高于/低于基准线
└── 推荐: 红-白-蓝 / 紫-白-绿

分类色阶 (Categorical):
├── 各类别颜色区分度高且美观
├── 适用: 饼图 / 分组柱状图
└── 推荐: Tableau 10 / Set2 / Pastel1

无障碍考虑:
├── 色盲友好 (避免红绿搭配)
├── 对比度 > 4.5:1 (WCAG AA)
├── 不仅依赖颜色传达信息 (添加图案/标签)
└── 深色背景友好 (深色模式支持)
```

---

## 性能优化策略

```
数据层优化:
├── 预聚合 (Materialized Views)
│   └── 将常用聚合结果预先计算存储
├── 分区裁剪 (Partition Pruning)
│   └── 只扫描相关时间分区
├── 列式存储 (Columnar Format)
│   └── Parquet / ClickHouse / DuckDB
└── 缓存层
    ├── Redis (热点查询缓存)
    └── CDN (静态资源)

查询层优化:
├── 查询计划分析 (EXPLAIN ANALYZE)
├── 避免 SELECT *
├── 合理使用索引
└── LIMIT 结果集大小

渲染层优化:
├── Canvas 渲染 (大数据量用 Canvas 替代 SVG)
├── 虚拟滚动 (Virtual Scrolling for long tables)
├── Web Worker (后台数据处理)
├── 懒加载 (Lazy load off-screen charts)
└── 数据抽样 (预览用采样, 详情用全量)

网络层优化:
├── 压缩传输 (Gzip/Brotli)
├── 增量更新 (WebSocket push vs polling)
├── CDN 边缘加速
└── HTTP/2 多路复用
```

---

*基于 Stephen Few / Edward Tufte / Tableau 最佳实践*
*版本: 2.0.0 | 最后更新: 2026-04-27*
