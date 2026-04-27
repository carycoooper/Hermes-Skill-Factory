---
name: competitive-intelligence
description: "Hermes-Skill-Competitive-Intelligence - 竞争情报分析系统，提供竞品监控、市场定位分析、SWOT矩阵、定价策略对比和差异化机会发现。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [competitive-intelligence, market-analysis, swot, competitor-monitoring, pricing-strategy, positioning, product-management]
    related_skills: [deep-research, data-visualization-generator, content-calendar, prompt-engineering-assistant]
    requires_toolsets: [web, terminal]
    config:
      - key: industry_focus
        default: saas
      - key: analysis_depth
        default: comprehensive
---

# Hermes-Skill-Competitive-Intelligence (竞争情报分析系统)

## 概述

**Hermes-Skill-Competitive-Intelligence** 是一套专业的市场竞争情报收集与分析工具。它帮助产品经理、战略师和企业决策者系统地了解竞争对手动态，发现市场机会，制定差异化策略。

### 核心能力

- **竞品全景**: 多维度竞品数据库（功能/定价/技术栈/用户评价）
- **实时监控**: 网站变更、新功能发布、融资新闻、招聘动向
- **SWOT 分析**: 自动化优势/劣势/机会/威胁评估
- **定价情报**: 竞品价格体系拆解和策略解读
- **差异化引擎**: 基于数据的产品定位建议
- **报告生成**: 一键产出专业级竞品分析报告

---

## 分析框架

### 6C 竞争分析框架

```
Company (公司): 谁是竞争对手？
  ├── 直接竞品: 相同目标用户 + 相同解决方案
  ├── 间接竞品: 不同方案解决相同问题
  └── 潜在竞品: 可能进入市场的玩家

Customer (客户): 用户在选择什么？
  ├── 决策因素排序 (价格/功能/品牌/服务)
  ├── 用户画像重叠度
  └── NPS 和满意度对比

Capability (能力): 核心竞争力在哪？
  ├── 功能完整性矩阵
  ├── 技术架构差异
  └── 创新速度对比

Cost (成本): 价格战怎么打？
  ├── 定价模式分析 (Freemium/订阅/用量)
  ├── TCO (总拥有成本) 对比
  └── 隐性成本发现

Channel (渠道): 他们怎么触达用户？
  ├── 获客渠道分布
  ├── 内容营销策略
  └── 合作伙伴生态

Context (环境): 外部环境影响？
  ├── 行业趋势 (PESTEL 分析)
  ├── 监管政策变化
  └── 技术范式转移
```

---

## 核心模块

### 1. 竞品数据库

```bash
# 添加竞品
/ci competitor add "Notion" --url notion.so --category productivity --tier enterprise

# 批量导入
/ci competitors import competitors_list.csv

# 查看竞品卡片
/ci competitor show Notion --detail full
```

**竞品档案结构：**

```yaml
competitor:
  basic_info:
    name: "Notion"
    founded: 2013
    headquarters: "San Francisco, CA"
    employees: "~1000"
    funding: "$10B+ (Series C)"
    website: "notion.so"
    
  positioning:
    tagline: "All-in-one workspace"
    target_audience: ["Teams", "Individuals", "Enterprise"]
    primary_use_cases: ["Documentation", "Project Management", "Wiki", "Notes"]
    pricing_position: "Mid-market to Enterprise"
    
  product_analysis:
    core_features:
      - name: "Blocks & Database"
        maturity: mature
        differentiation: "Relational database approach to documents"
        our_status: "✅ We have equivalent"
        
      - name: "AI Writing Assistant"
        maturity: growing
        differentiation: "Native AI integration across all blocks"
        our_status: "⚠️ We have basic, they have advanced"
        
      - name: "API & Integrations"
        maturity: mature
        differentiation: "Rich API + 100+ integrations"
        our_status: "❌ Gap identified"
        
    tech_stack:
      frontend: "React, TypeScript, WASM"
      backend: "Node.js, PostgreSQL, ElasticSearch"
      infrastructure: "AWS, Cloudflare Workers"
      
    strengths: ["Brand recognition", "Ecosystem size", "Community"]
    weaknesses: ["Offline mode limited", "Performance with large databases", "Learning curve"]
    
  go_to_market:
    pricing:
      free_tier: "Generous (individual)"
      paid_start: "$8/user/month (Pro)"
      enterprise: "Custom (contact sales)"
      model: "Per-seat subscription"
      
    channels:
      - "Product-led growth (viral sharing)"
      - "YouTube tutorials ecosystem"
      - "Template marketplace"
      - "Ambassador/affiliate program"
      
    recent_moves:
      - "2026-Q1: Launched Notion AI (GPT-4 integration)"
      - "2026-Q2: Notion Forms (Typeform alternative)"
      - "Hiring: 50+ open positions (aggressive expansion)"
```

### 2. SWOT 分析引擎

```bash
/ci swot analyze --for my-product --against Notion,Coda,Obsidian,Confluence
```

**自动化 SWOT 输出：**

```
╔══════════════════════════════════════════════════════════╗
║           SWOT Analysis: MyProduct vs Market                    ║
╠══════════════════════════════════════════════════════════╣
║                                                            ║
║  STRENGTHS (内部优势) 🟢                                   ║
║  ├─ ✅ 更快的本地性能 (桌面应用)                          ║
║  ├─ ✅ 离线优先架构 (无网络依赖)                           ║
║  ├─ ✅ 一次性付费 vs 订阅制 (TCO更低)                       ║
║  ├─ ✅ 开源透明度更高                                     ║
║  └─ ✅ 更灵活的自定义能力                                 ║
║                                                            ║
║  WEAKNESSES (内部劣势) 🔴                                  ║
║  ├─ ❌ 协作功能较弱 (实时编辑)                              ║
║  ├─ ❌ 生态系统较小 (模板/集成)                             ║
║  ├─ ❌ 品牌知名度低                                       ║
║  ├─ ❌ 移动端体验一般                                      ║
║  └─ ❌ 企业级功能缺失 (SSO/审计/合规)                       ║
║                                                            ║
║  OPPORTUNITIES (外部机会) 🟡                                ║
║  ├─ 📈 AI-native generation 成为主流需求                     ║
║  ├─ 📈 远程办公推动离线工具需求增长                         ║
║  ├─ 📈 用户对隐私和数据主权的关注度提升                      ║
║  ├─ 📈 开发者/创作者经济崛起                                 ║
║  └─ 📈 竞品提价潮 (Notion Pro $8→$10) 创造窗口期             ║
║                                                            ║
║  THREATS (外部威胁) 🔵                                      ║
║  ├─ ⚠️ Notion AI 的先发优势                                 ║
║  ├─ ⚠️ Microsoft Loop (Copilot集成) 进入同一赛道             ║
║  ├─ ⚠️ 免费层越来越慷慨 (提高转换门槛)                     ║
║  ├─ ⚠️ 经济下行可能压缩企业IT预算                          ║
║  └─ ⚠️ 大平台入场 (Google Workspace AI?)                   ║
║                                                            ║
╠══════════════════════════════════════════════════════════╣
║  STRATEGIC PRIORITIES (基于SWOT):                            ║
║  P1: 强化AI能力以缩小与Notion AI的差距 (O1+W2抵消T1)     ║
║  P2: 打造杀手级协作功能作为差异化突破点 (W1→S)             ║
║  P3: 推进企业版功能获取更大客户群 (O3+S组合)               ║
║  P4: 利用隐私/本地化叙事对抗云端竞品 (S1+O3+O4)          ║
╚══════════════════════════════════════════════════════════╝
```

### 3. 定价情报

```bash
/ci pricing analyze --market productivity-tools --depth deep
```

**定价矩阵可视化：**

```
                    Free     Personal    Team       Enterprise
                    ─────    ────────    ────        ───────────
Notion              ✅       $8/mo      $15/mo      Custom
Coda               ✅       $12/mo     $30/mo      Custom
Obsidian           ✅       $5/mo(1x)  $8/mo(1x)   $15/mo(1x)
Confluence         Trial    $5.75/mo   $11/user   Custom
MyProduct          ✅       $49/life  $99/life   $299/life

关键洞察:
├── Notion 采用 per-seat 模式 → 团队规模越大越贵
├── Obsidian 一次性付费 → LTV 高但现金流慢
├── 我们的一次性定价在 Team 阶段极具竞争力 ($99 vs $180/年/人)
├── 机会: 强调 "买断 vs 订阅" 的长期节省计算
```

### 4. 差异化定位生成器

```bash
/ci differentiate --from "Another productivity tool" --angle privacy-first
```

**输出：定位声明 + 价值主张 + 营销文案**

```
=== DIFFERENTIATED POSITIONING ===

Primary Positioning Statement:
"MyProduct — The privacy-first, own-your-data workspace 
that works offline forever. No subscriptions, no vendor lock-in."

Value Proposition Canvas:
┌─────────────────────────────────────────────────────┐
│ PRODUCTS                                            │
│ • Offline-first document workspace                  │
│ • Local-first data storage (SQLite/Files)           │
│ • One-time purchase, lifetime updates               │
│ • No account required to start using               │
├─────────────────────────────────────────────────────┤
│ PAIN RELIEVERS                                      │
│ 🔒 "I'm worried about storing sensitive company    │
│     data in cloud services that could be breached" │
│ 💰 "Subscription fatigue — paying monthly for tools │
│     I don't use every month"                         │
│ 📴 "What if the service shuts down or changes       │
│     their pricing?"                                  │
│ 🐢 "I need to work on confidential client projects │
│     without risking data exposure"                    │
├─────────────────────────────────────────────────────┤
│ GAINS                                               │
│ ✓ Complete data ownership and portability           │
│ ✓ Predictable costs (no surprise price hikes)       │
│ ✓ Works anywhere (plane, cave, no-signal zone)     │
│ ✓ No vendor dependency for critical workflows       │
└─────────────────────────────────────────────────────┘

Marketing Copy Variants:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

For Privacy-Conscious Users:
"Notion reads your data. MyProduct keeps it on YOUR device.
Your notes. Your rules. Forever."

For Budget-Minded Teams:
"$99 once. Not $120/year. Every. Single. Year.
Calculate your 5-year savings: $99 vs $600+"

For Remote Workers:
"WiFi died? No problem. MyProduct works without internet.
Sync when you're back online. Your workflow never stops."
```

---

## 监控与告警

```bash
# 设置监控
/ci monitor add Notion --track features,pricing,hiring,blog

# 每周竞品简报
/ci report weekly --format markdown --email team@company.com

# 实时告警
/ci alert set --trigger "competitor launches AI feature" --notify slack
```

**监控维度：**

```
产品层面:
├── 新功能发布 (Feature Launch)
├── 定价变更 (Price Change)
├── 平台扩展 (Platform Expansion)
├── 集成新增 (New Integration)
└── 下架/废弃功能 (Deprecation)

公司层面:
├── 融资动态 (Funding News)
├── 关键人员变动 (Exec Changes)
├── 招聘趋势 (Hiring Patterns - 暗示战略方向)
├── 合作伙伴 (Partnerships/Acquisitions)
└── 负面舆情 (Negative Press/Reviews)

市场层面:
├── SEO 排名变化 (Keyword Rankings)
├── 社交媒体热度 (Social Mentions)
├── 用户评价情感 (Review Sentiment)
├── 社区活跃度 (Community Activity)
└── 广告投放 (Ad Placements)
```

---

## 报告模板

```bash
# 月度竞品情报报告
/ci report monthly --template comprehensive --output Q2_2026_CI_Report.pdf

# 季度决策快速报告
/ci report quick --focus pricing,features --format presentation
```

**月报结构：**
1. Executive Summary (1页精华)
2. Key Movements 本月大事记
3. Feature Comparison Matrix (功能对比表)
4. Pricing Landscape Update
5. SWOT Update (vs 上月变化)
6. Threat Assessment (新增威胁评级)
7. Opportunity Radar (高价值机会推荐)
8. Recommended Actions (下月行动项)

---

*创新技能 - 结合 Michael Porter 竞争战略 + 现代 SaaS 竞争分析方法论*
*版本: 2.0.0 | 最后更新: 2026-04-27*
