---
name: seo-content-optimizer
description: "Hermes-Skill-SEO-Content-Optimizer - SEO内容优化专家，提供关键词研究、内容评分、竞品分析、元数据优化和搜索引擎可见性提升方案。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [seo, content-marketing, keyword-research, google-search, ranking, on-page, off-page, technical-seo]
    related_skills: [content-calendar, deep-research, data-visualization-generator, prompt-engineering-assistant]
    requires_toolsets: [web, terminal]
    config:
      - key: target_search_engine
        default: google  # google/bing/baidu
      - key: target_language
        default: zh-CN
      - key: content_type
        default: article
---

# Hermes-Skill-SEO-Content-Optimizer (SEO 内容优化专家)

## 概述

**Hermes-Skill-SEO-Content-Optimizer** 是一个全面的 SEO（搜索引擎优化）内容工程系统。它从关键词研究到内容创作、从页面优化到排名追踪，提供一站式的搜索引擎可见性提升方案。

### 核心能力

- **关键词引擎**: 搜索量/难度/意图分析，长尾词挖掘
- **内容评分**: SEO 完整度打分 + AI 质量评估
- **竞品分析**: TOP10 结果拆解，找到超越机会
- **On-Page 优化**: 标题/描述/H1/内链/图片ALT/Schema
- **技术 SEO**: 页面速度/Core Web Vitals/移动适配/结构化数据
- **排名追踪**: 关键词位置监控 + SERP 特征快照

---

## 工作流

```
Phase 1: 关键词研究
    ↓ 选定目标关键词
Phase 2: 竞品内容分析
    ↓ 了解搜索意图和内容缺口
Phase 3: 内容大纲规划
    ↓ 设计优于竞品的结构
Phase 4: 内容创作与优化
    ↓ 写作 + On-Page SEO
Phase 5: 发布后追踪
    ↓ 排名监控 + 持续优化
```

---

## Phase 1: 关键词研究

### 关键词发现

```bash
# 种子词扩展
/seo keywords expand "AI Agent 开发" --depth 3 --limit 50

# 长尾词挖掘
/seo keywords long-tail "Python 自动化" --search-volume > 100 --difficulty < 30

# 关键词聚类
/seo keywords cluster "机器学习 教程" --algorithm hierarchical

# 意图分类
/seo keywords intent "best CRM software" 
# 输出: informational(40%) / commercial(35%) / transactional(20%) / navigational(5%)
```

### 关键词评估矩阵

```
每个关键词输出:

┌─────────────────────────────────────────────────────┐
│ 关键词: "Hermes AI agent skills"                   │
│                                                       │
│ 搜索数据:                                             │
│ ├── 月搜索量 (MSV): 2,400                            │
│ ├── CPC: $1.20                                       │
│ └── 竞争难度: 25/100 (Low 🔵)                       │
│                                                       │
│ 趋势: 📈 上升中 (+15% MoM)                           │
│ 季节性: 无明显季节波动                                 │
│                                                       │
│ SERP 特征:                                            │
│ ├── Featured Snippet机会: 高 ⭐                        │
│ ├── PAA (People Also Ask): 4个问题                    │
│ ├── 视频结果: 无                                      │
│ └── 本地包: 不适用                                    │
│                                                       │
│ 建议: ★★★★★ 优先 targeting                          │
│ 理由: 低竞争 + 高商业意图 + FS机会                    │
└─────────────────────────────────────────────────────┘
```

---

## Phase 2: 竞品内容分析

### TOP10 拆解

```bash
/seo competitor analyze "how to build AI agent" --top 10
```

**输出报告：**

```markdown
## SERP 竞品分析: "how to build AI agent"

### 排名概览
| 排名 | 标题 | URL | 字数 | DA | FS? | 更新时间 |
|------|------|-----|------|----|-----|----------|
| 1 | Complete Guide to Building... | openai.com/blog | 4500 | 95 | ✅ | 2026-03 |
| 2 | How to Build Your First AI Agent | Towards Data Science | 3200 | 88 | ❌ | 2026-02 |
| ... | ... | ... | ... | ... | ... | ... |

### 内容缺口分析
**已有覆盖:**
- ✅ 基础概念解释
- ✅ Python 代码示例
- ✅ 工具推荐列表

**缺失/薄弱:**
- ❌ 实际项目案例（仅理论）
- ❌ 性能优化技巧
- ❌ 错误处理最佳实践
- ❌ 成本对比分析
- ❌ 视频教程嵌入

### 超越策略
**我们的角度**: "从0到生产级：构建企业级AI Agent的完整指南"
- 增加：真实项目代码仓库链接
- 增加：性能基准测试数据
- 增加：常见错误Top10 + 解决方案
- 增加：成本估算表格（不同模型API费用）
- 格式：交互式Demo + 代码沙盒
```

---

## Phase 3: 内容大纲规划

### AI 大纲生成

```bash
/seo outline generate --keyword "Hermes AI agent tutorial" --target-length 4000 --style comprehensive
```

**输出大纲：**

```markdown
# Hermes AI Agent 完整开发指南 (目标: 4000字)

## H1: 如何从零开发一个生产级的 Hermes AI Agent (主关键词)

### H2: 什么是 Hermes AI Agent？(LSI: definition, architecture)
- H3: Agent vs 传统程序的区别
- H3: Hermes 平台核心组件解析
  - H4: Skill 系统
  - H4: Tool Use 机制
  - H4: Memory 管理

### H2: 环境准备 (LSI: setup, installation, prerequisites)
- H3: 第一步：安装 Hermes CLI
- H3: 第二步：获取 API 密钥
- H3: 第三步：项目结构初始化
  - 代码块: 项目目录树
  - 截图: VS Code 项目视图

### H2: 你的第一个 Skill (LSI: hello world, beginner tutorial)
- H3: 创建 Skill 文件
  - 代码块: 最小可运行示例
- H3: 本地测试
- H3: 发布到 GitHub

### H2: 进阶：Tool Use 与 API 集成 (LSI: function calling, tools)
- H3: 定义自定义 Tool
- H3: 错误处理模式
- H3: 实战：天气查询 Agent

### H2: 生产级最佳实践 (LSI: best practices, optimization)
- H3: 性能优化 Top 5
  - 表格: 优化前后对比
- H3: 安全注意事项
- H3: 成本控制策略
  - 表格: 各模型 Token 用量和费用

### H2: 常见问题 FAQ (PAA 覆盖)
- Q1: Hermes 免费吗？
- Q2: 和 GPTs 有什么区别？
- ...

### H2: 总结与下一步 (CTA)
- 相关文章内部链接 ×3
- CTA: 下载完整项目代码
```

---

## Phase 4: On-Page SEO 优化

### 元数据优化

```yaml
title_optimization:
  rules:
    - length: 50-60 characters (中文: 25-30字)
    - primary_keyword: 放在最前面
    - power_words: ["完整","终极","免费","指南","2026"]
    - numbers: 使用阿拉伯数字 ("5个技巧" not "五个技巧")
    - brackets:适度使用括号吸引注意
    - separator: 使用 "| " or " - "
    
  example:
    before: "关于Hermes的一些介绍"
    after:  "Hermes AI Agent 开发完全指南 | 2026最新版 (含代码)"

description_optimization:
  rules:
    - length: 120-160 characters (中文: 60-80字)
    - include_primary_kw: 在前 50 字符内出现
    - include_secondary_kw: 自然融入 1-2 个相关词
    - cta: 包含行动号召
    - no_truncation: 避免被搜索引擎截断
    
  template: "{primary_kw} {benefit}。{secondary_phrase}。立即{cta}！"
```

### 内容 SEO 要素清单

```
On-Page SEO Checklist:

Title Tag:
├── [ ] 主关键词在前 30 字符
├── [ ] 长度适中 (25-30 中文字符)
├── [ ] 包含数字或年份
└── [ ] 有吸引力但不标题党

Meta Description:
├── [ ] 60-80 中文字符
├── [ ] 包含主关键词
├── [ ] 有明确的价值主张
└── [ ] 包含 CTA

URL Slug:
├── [ ] 简短 (< 5 个单词)
├── [ ] 包含主关键词
├── [ ] 全小写 + 连字符
└── [ ] 无停用词 (a/an/the/of/in)

Heading Structure:
├── [ ] 只有一个 H1（= Title）
├── [ ] H2 包含 LSI 关键词
├── [ ] 层级清晰 (H1→H2→H3，不跳级)
└── [ ] 每个 H2 下至少 2-3 个段落

内容质量:
├── [ ] 字数 ≥ 竞品平均 (通常 >2000 字)
├── [ ] 关键词密度 1-2%（自然分布）
├── [ ] 前 100 词出现主关键词
├── [ ] LSI 词自然散布全文
└── [ ] 可读性分数 > 60 (Flesch Reading Ease)

多媒体:
├── [ ] 至少 1 张图（文件名含关键词）
├── [ ] 图片 ALT 属性已填写
├── [ ] 视频嵌入（增加停留时间）
└── [ ] 信息图表/表格（增加价值）

内部链接:
├── [ ] 链接到 3-5 篇相关文章
├── [ ] 锚文本包含关键词（非 "点击这里"）
├── [ ] 无断链
└── [ ] 新窗口打开外部链接 rel="nofollow"

结构化数据 (Schema):
├── [ ] Article schema
├── [ ] FAQ schema (如有FAQ部分)
├── [ ] HowTo schema (如有教程步骤)
├── [ ] BreadcrumbList
└── [ ] Organization/WebSite

技术要素:
├── [ ] 加载速度 < 3秒
├── [ ] Mobile-friendly (响应式)
├── [ ] HTTPS 已启用
├── [ ] Canonical URL 正确设置
└── [ ] Open Graph + Twitter Card 标签
```

---

## Phase 5: 排名追踪与优化

### 监控面板

```bash
# 添加追踪关键词
/seo tracking add "Hermes AI agent development" --country CN --language zh --device desktop

# 查看排名报告
/seo tracking report --range last-30-days --format dashboard

# SERP 变动提醒
/seo alert set --keyword "Hermes AI agent" --condition "rank_change > 5" --notify slack
```

### Core Web Vitals 优化

```
LCP (Largest Contentful Paint) < 2.5s:
├── 优化首屏图片 (WebP/AVIF + lazy load)
├── 使用 CDN 加速静态资源
├── 预加载关键资源 (<link rel="preload">)
└── 减少 Render Blocking JS/CSS

FID/INP (Interaction to Next Paint) < 100ms:
├── 分割长任务 (Long Tasks)
├── 使用 Web Workers 处理复杂计算
├── 延迟加载非关键第三方脚本
└── 事件委托减少事件监听器

CLS (Cumulative Layout Shift) < 0.1:
├── 为图片/视频设置固定尺寸 (width/height)
├── 避免动态插入内容 above the fold
├── 使用 font-display: swap
└── Reserve space for ads (if any)
```

---

## 内容质量评分

### SEO Score 卡 (满分 100)

```
Technical SEO (25分):
├── Page Speed:     /10
├── Mobile:         /5
├── HTTPS/Security: /5
├── Structured Data:/5
└── Crawlability:  /5

On-Page SEO (35分):
├── Title/Meta:     /10
├── Content Quality:/10
├── Headings:       /5
├── Internal Links: /5
├── Images/Media:   /5

Off-Page Signals (20分):
├── Backlink Profile:/10
├── Social Signals: /5
├── Brand Mentions: /5

Content Value (20分):
├── E-E-A-T signals: /10
├── User Engagement: /5
├── Freshness:      /5

总分: ___ /100
评级: A(90+) / B(75-89) / C(60-74) / D(<60)
```

---

*创新技能 - 结合 Google 最新算法指南 + 中文SEO实战经验*
*版本: 2.0.0 | 最后更新: 2026-04-27*
