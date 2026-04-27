---
name: data-visualization-generator
description: "Hermes-Skill-Data-Visualization-Generator - 智能数据可视化生成器，自动将数据转换为专业图表（折线/柱状/饼图/散点/热力图等），支持交互式仪表盘和报告级输出。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [visualization, charts, dashboard, data-analysis, plotly, d3, matplotlib, reporting, analytics]
    related_skills: [data-pipeline-orchestrator, deep-research, finance-tracker]
    requires_toolsets: [web, terminal]
    config:
      - key: default_chart_type
        default: auto
      - key: output_format
        default: interactive_html
      - key: theme
        default: professional
---

# Hermes-Skill-Data-Visualization-Generator (数据可视化生成器)

## 概述

**Hermes-Skill-Data-Visualization-Generator** 是一个智能数据可视化系统，能够自动分析数据特征并选择最优的图表类型，生成交互式、出版级的可视化作品。无需手动编写复杂的绘图代码，只需描述你的数据和需求。

### 核心价值

- **智能图表推荐**: 根据数据维度和分布自动选择最佳图表类型
- **多格式输出**: HTML(交互式) / PNG / SVG / PDF / PPTX
- **设计美学**: 内置多套专业配色方案和布局模板
- **一键发布**: 直接嵌入网页或导出为报告素材
- **响应式**: 自适应桌面/平板/移动端显示

---

## 支持的图表类型

### 基础图表

| 图表 | 适用场景 | 数据要求 |
|------|---------|---------|
| **柱状图 (Bar)** | 类别比较、排名 | 1个类别 + 1个数值 |
| **折线图 (Line)** | 趋势变化、时间序列 | 时间轴 + 数值 |
| **饼图 (Pie)** | 占比构成、部分整体 | 1个类别 + 1个占比 |
| **散点图 (Scatter)** | 相关性分析、分布 | 2个数值变量 |
| **面积图 (Area)** | 累积趋势、体量变化 | 时间 + 数值 |

### 高级图表

| 图表 | 适用场景 |
|------|---------|
| **热力图 (Heatmap)** | 二维数据密度、相关性矩阵 |
| **箱线图 (Box Plot)** | 数据分布、异常值检测 |
| **雷达图 (Radar)** | 多维对比、能力评估 |
| **桑基图 (Sankey)** | 流量走向、转化漏斗 |
| **树状图 (Treemap)** | 层级占比、文件结构 |
| **地理地图 (Choropleth)** | 区域数据、地理位置 |
| **甘特图 (Gantt)** | 项目进度、时间线 |
| **漏斗图 (Funnel)** | 转化率、用户路径 |
| **旭日图 (Sunburst)** | 多层级占比 |
| **平行坐标图 (Parallel Coordinates)** | 多维数据比较 |

### 组合图表

```
组合模式:
├── 双Y轴: 左右两个不同量纲的指标
├── 子图网格: 多个相关图表并列展示
├── 主次图: 大图+小图的焦点+细节
└── 动画序列: 按时间顺序播放的多帧图表
```

---

## 快速开始

```bash
# 最简方式 - 自动识别最佳图表
/viz generate data.csv --auto

# 指定图表类型
/viz generate sales_data.xlsx --chart line --x month --y revenue

# 创建完整仪表盘
/viz dashboard create --name "Executive Overview" \
  --panels kpi_revenue,line_trend,bar_by_region,funnel_conversion

# 从自然语言描述生成
/viz from-text "展示过去12个月的月度收入趋势，用蓝色渐变折线图，标注最高点和最低点"

# 批量生成报告图表
/viz report create --data quarterly_report.json --template executive --output pptx
```

---

## 使用方式

### 方式一：CSV/Excel 文件输入

```bash
# CSV 文件
/viz generate monthly_sales.csv

# Excel 多 Sheet
/viz generate financial_report.xlsx --sheet "Q4 Revenue"
```

**数据格式建议:**

```csv
month,revenue,profit,customers,region
2026-01,150000,32000,1250,North
2026-02,178000,41000,1380,North
2026-03,195000,48000,1520,North
...
```

### 方式二：JSON 数据输入

```json
{
  "chart_type": "combo",
  "title": "季度业绩概览",
  "data": {
    "categories": ["Q1", "Q2", "Q3", "Q4"],
    "series": [
      {"name": "收入", "values": [450000, 520000, 610000, 720000], "type": "bar"},
      {"name": "利润", "values": [90000, 110000, 140000, 180000], "type": "line"}
    ]
  }
}
```

### 方式三：自然语言描述

```
支持的描述语法:

基础:
  "画一个柱状图，比较各地区的销售额"
  "展示用户增长趋势的折线图"

进阶:
  "堆叠柱状图，按产品线和地区两层分组，显示Q1-Q4的收入"
  "散点图矩阵，展示各产品特性之间的相关性，带回归线"

样式:
  "使用商务蓝色主题"
  "添加数据标签和趋势线"
  "标题用中文，数值格式化为万元"
```

---

## 仪表盘构建

### 预设模板

```bash
# 业务运营仪表盘
/viz template business-dashboard
# 包含: KPI卡片 + 收入趋势 + 地区分布 + 产品排行 + 目标达成率

# 技术监控仪表盘
/viz template devops-dashboard  
# 包含: 请求量/延迟P99/错误率/CPU/内存/部署频率

# HR 人事仪表盘
/viz template hr-dashboard
# 包含: 入离职趋势/部门人数/薪资分布/绩效分布/考勤统计
```

### 自定义布局

```yaml
dashboard:
  name: "CEO Monthly Review"
  layout: grid  # grid / flex / absolute
  columns: 3
  rows: 2
  
  panels:
    - position: [0, 0, 1, 1]  # col_start, row_start, col_span, row_span
      type: kpi
      title: "总营收"
      value: "$2,300,000"
      change: "+15.2%"
      trend: up
      
    - position: [1, 0, 1, 1]
      type: chart
      chart_type: line
      title: "收入趋势"
      
    - position: [2, 0, 1, 1]
      type: chart
      chart_type: pie
      title: "收入来源"
      
    - position: [0, 1, 3, 1]
      type: chart
      chart_type: bar
      title: "各地区业绩对比"
      span: full_width
```

---

## 设计系统

### 配色方案

```
内置主题:

🎨 Professional (商务专业)
   Primary: #1f77b4 (蓝) / Accent: #ff7f0e2 (橙)
   
🎨 Dark Mode (暗色)
   Background: #1a1a2e / Text: #eaeaea
   
🎨 Minimalist (极简)
   Monochrome grayscale with single accent color
   
🎨 Vibrant (活力多彩)
   Bold saturated colors for presentations
   
🎨 Accessible (无障碍)
   WCAG 2.1 AA compliant colorblind-safe palette
   
🎨 Brand Custom (品牌自定义)
   输入品牌色值自动生成和谐配色
```

### 排版规范

```
字体层级:
├── H1 标题: 24px Bold
├── H2 副标题: 18px Semibold  
├── 正文: 14px Regular
├── 标签: 12px Medium
└── 注释: 11px Light

间距系统 (8px 基准):
├── xs: 4px
├── sm: 8px
├── md: 16px
├── lg: 24px
└── xl: 32px
```

---

## 导出与分享

### 输出格式

| 格式 | 特点 | 适用场景 |
|------|------|---------|
| **HTML (交互式)** | 可缩放、可筛选、动画 | 网页嵌入、在线分享 |
| **PNG** | 静态图片、透明背景 | 文档插入、社交媒体 |
| **SVG** | 矢量无损、可编辑 | 印刷品、设计软件 |
| **PDF** | 多页、高质量 | 正式报告、存档 |
| **PPTX** | 可编辑幻灯片 | 演示汇报 |
| **Mermaid** | 文本代码 | 技术文档、Markdown |

### 分享选项

```bash
# 生成交互式链接 (托管)
/viz share dashboard.html --host --expire 7d

# 嵌入代码
/viz embed chart.png --format html --width 600 --height 400

# 批量导出
/viz export-all --format png --dpi 300 --output ./report_assets/
```

---

## 最佳实践

### ✅ 图表选择指南

| 你想展示什么 | 推荐图表 |
|-------------|---------|
| 排名/Top N | 水平柱状图 |
| 占比/构成 | 饼图（≤5类）/ 环形图（>5类） |
| 趋势/变化 | 折线图 / 面积图 |
| 分布情况 | 直方图 / 箱线图 |
| 相关关系 | 散点图 / 气泡图 |
| 流程/转化 | 漏斗图 / 桑基图 |
| 地理分布 | 地图 / 点状地图 |
| 多指标对比 | 雷达图 / 平行坐标 |

### ❌ 常见错误

- **3D 效果**: 歪曲数据比例，避免使用
- **双饼图对比**: 人眼难以比较角度差异
- **颜色过多**: >7 种颜色难以区分
- **忽略零基线**: 柱状图 Y 轴必须从 0 开始
- **图表垃圾**: 删除不必要的网格线、背景色、装饰元素

---

*创新技能 - 基于 D3.js/Plotly/ECharts 最佳实践设计*
*版本: 2.0.0 | 最后更新: 2026-04-27*
