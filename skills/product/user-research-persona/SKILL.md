---
name: user-research-persona
description: "Hermes-Skill-User-Research-Persona - 用户研究与画像生成器，支持用户访谈设计、问卷创建、数据分析、用户画像构建和 empathy map 生成。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [user-research, persona, ux-research, user-interview, survey, empathy-map, customer-journey, ux-design]
    related_skills: [competitive-intelligence, content-calendar, frontend-design-assistant, deep-research]
    requires_toolsets: [web, terminal]
    config:
      - key: research_method
        default: mixed_methods
      - key: persona_detail_level
        default: detailed
---

# Hermes-Skill-User-Research-Persona (用户研究与画像生成器)

## 概述

**Hermes-Skill-User-Research-Persona** 是一个完整的用户体验研究工具链。从研究规划、数据收集、分析洞察到最终的用户画像（Persona）和旅程地图，提供端到端的用户研究能力。

### 核心能力

- **研究方法论**: 定性/定量/混合方法选择与执行
- **访谈设计**: 半结构化访谈指南 + 问题模板库
- **问卷工程**: 科学问卷设计 + 偏见消除
- **数据分析**: 编码/主题提取/统计推断/AI 辅助洞察
- **Persona 构建**: 数据驱动的用户画像生成
- **Empathy Map**: 同理心地图 + 痛点/需求可视化

---

## 研究流程

```
Phase 1: Define (定义)
    ↓ 明确研究问题 + 选择方法 + 规划样本
    
Phase 2: Discover (发现)
    ↓ 访谈 / 问卷 / 观察 / 数据分析
    
Phase 3: Analyze (分析)
    ↓ 编码 → 聚类 → 洞察提取 → 假设形成
    
Phase 4: Synthesize (综合)
    ↓ 用户画像 + 旅程地图 + 机会地图
    
Phase 5: Validate (验证)
    ↓ Stakeholder 对齐 + 快速测试假设
```

---

## 用户画像系统

### Persona 生成引擎

```bash
# 从原始研究数据生成 Persona
/persona generate --from interview_transcripts/ --output personas.md

# 创建特定场景的 Persona
/persona create --role "SaaS产品经理" --company_size "50-200人" --tech_savvy medium

# Persona 对比矩阵
/persona compare --all --matrix features,pains,gains,motivations
```

### 完整 Persona 示例

```markdown
# 👤 Persona: "效率追求者 Emma"

## 基本信息
| 属性 | 详情 |
|------|------|
| **姓名** | Emma Chen |
| **年龄** | 32 岁 |
| **职位** | 产品经理 |
| **公司** | 中型 SaaS 公司 (120人) |
| **行业** | 企业协作工具 |
| **工作年限** | 7 年 |
| **技术熟练度** | 中高 (能使用大部分工具，但不写代码) |
| **设备** | MacBook Pro + iPhone 15 Pro + iPad (阅读) |

## 一句话描述
> "Emma 是一个永远在追赶截止日期的产品经理，她需要工具帮她更快地完成跨部门协调和文档工作，而不是增加她的认知负担。"

## 目标与动机

### 主要目标 (Goals)
1. **每周节省 5+ 小时** 在会议纪要和状态同步上
2. **减少信息遗漏** — 不再错过重要的决策背景
3. **提升团队透明度** — 让每个人都知道项目进展
4. **快速产出高质量文档** — PRD / 发布说明 / 周报

### 核心动机 (Motivations)
- 🏆 **职业成长**: 想成为"高效PM"的标杆
- 💪 **掌控感**: 讨厌混乱和信息不对称
- 🎯 **成就感**: 喜欢看到想法快速落地
- 🤝 **团队认可**: 希望被同事视为可靠的人

## 行为模式

### 典型一天
```
08:30  ☕ 到公司，先看邮件和 Slack 未读消息
09:00  📋 整理今日 To-Do（用 Notion）
09:30  🔨 深度工作时间（写PRD）— 最多2小时不被打扰
11:30  🍱 午餐 + 刷 LinkedIn/Twitter 看行业动态
13:00  🤝 跨部门对齐会（通常1-2个）
15:00  📊 查看数据仪表盘（Mixpanel/Amplitude）
16:00  📝 回复异步消息 + 处理突发需求
17:30  📅 规划明天 + 更新项目进度
18:00  🚗 下班（理想情况，实际经常到19:30）
```

### 信息获取习惯
```
首选渠道:
├── 内部: Slack 频道 > Wiki/Notion > 口头询问
├── 外部: Twitter/X (关注 PM 大V) > Newsletter > Product Hunt
│
信任层级:
├── ⭐⭐⭐⭐⭐ 同行推荐 / 实际使用过
├── ⭐⭐⭐⭐ 知名博主深度评测
├── ⭐⭐⭐ G2/Capterra 评价 + 截图证据
└── ⭐⭐ 官方营销材料
```

## 痛点地图 (Pain Points)

### 🔴 严重痛点 (Daily Frustrations)

| # | 痛点 | 发生频率 | 情绪影响 | 当前应对方式 |
|---|------|---------|---------|-------------|
| P1 | **会议太多，没时间做深度工作** | 每天 3-5 个会 | 😤 焦虑/挫败 | 提早到公司或晚走 |
| P2 | **信息散落在 10+ 个工具中** | 每小时多次 | 😫 认知疲劳 | 手动复制粘贴 |
| P3 | **等设计师/开发的反馈** | 每天 | 😤 无力感 | 反复催促 + 私聊 |
| P4 | **周报/月报是纯消耗** | 每周 | 😒 厌烦 | AI 工具辅助但质量不稳定 |

### 🟡 中等痛点

| # | 痛点 | 说明 |
|---|------|------|
| M1 | 新工具学习成本高 | "又一个要学的平台？" |
| M2 | 跨时区协作困难 | 与海外团队 async 沟通低效 |
| M3 | 数据分散无法统一视角 | Mixpanel + GA + CRM + 内部DB |
| M4 | 文档版本混乱 | "这是最新版吗？Google Doc 还是 Notion？" |

### 🟢 微小不满

- 工具间切换打断心流 (Context switching cost)
- 找不到之前看过的好文章/资源
- 团队知识没有沉淀，新人问同样的问题

## 技术栈与偏好

### 当前使用的工具
```
核心工具:
├── 项目管理: Linear (爱它的速度和键盘快捷键)
├── 文档: Notion (团队标准) + Google Docs (协作用)
├── 设计评审: Figma (只看不编辑)
├── 数据分析: Mixpanel + SQL (基础查询)
├── 通信: Slack + Zoom
└── 笔记: Obsidian (个人知识库)

尝试过但放弃:
├── Asana (太重了) 
├── Monday.com (UI 太花哨)
├── Confluence (搜索太差)
└── Jira (对非技术人员不友好)
```

### 工具选择标准
```
决策权重:
├── 效率优先 (40%) — 能否真正省时间？
├── 学习成本低 (25%) — 10分钟能上手吗？
├── 团队采用度 (20%) — 别人也在用吗？
├── 价格合理 (10%) — 公司报销或个人可承受
└── 美观/体验 (5%) — 用起来心情好
```

## 引用语 (Quotes)

> "我的一天被切成碎片。如果能把碎片时间拼成完整的一块，我能做出更好的产品。"

> "我最怕的不是工作量多，而是不知道该先做什么。决策疲劳比工作本身更累。"

> "给我一个工具前，请先告诉我：它帮我省的时间 > 学它花的时间吗？"

> "我不需要另一个 All-in-One 平台。我需要一个能和我现有工具无缝配合的工具。"

## 可触达渠道

```
线上:
├── LinkedIn (活跃用户，每周发 2-3 条动态)
├── Twitter/X (浏览为主，偶尔互动)
├── Product Hunt (新工具发现)
├── Indie Hackers / Hacker News (技术兴趣)
├── YouTube (PM 相关频道订阅者)

线下:
├── SaaS 行业会议 (SaaStratal, MicroConf)
├── 本地 PM Meetup (每月参加)
├── 产品经理社群 (微信群/Discord)
└── 同行推荐 (最高信任度的获客渠道)
```

## 反-Persona (Anti-Persona) — 不是我们的用户
- ❌ 只用 Excel 管理一切的传统企业用户
- ❌ 追求极致自定义能力的开发者/极客
- ❌ 预算 < $0 的免费版-only 用户
- ❌ 不愿意尝试新工具的技术保守派
```

---

## 访谈设计系统

### 访谈指南生成器

```bash
/interview guide create --type discovery --duration 45min --persona emma_pm
```

**输出：**

```markdown
# User Discovery Interview Guide
**目标用户**: 效率追求者型 PM  
**时长**: 45 分钟  
**形式**: 1v1 视频通话 (Zoom/Meet)

## 访谈前准备
- [ ] 确认参与者基本信息和同意书
- [ ] 测试录音/录屏工具
- [ ] 准备好屏幕共享 Demo 环境
- [ ] 准备好跟进问题清单

## 结构化议程 (45 min)

### Warm-up (5 min)
1. 自我介绍 + 访谈目的说明
2. "能简单介绍一下你的日常工作吗？"
3. "最近让你最有成就感的一个项目是什么？"

### Current Behavior (15 min)
4. "描述一下你典型的工作日是怎么安排的？"
5. "你目前用什么工具来完成 X 任务？能演示一下吗？"
6. "这些工具之间是如何配合的？有没有断点？"
7. "上一次你觉得'这个流程太痛苦了'是什么时候？"

### Pain Deep-dive (12 min)
8. "在你所有的工作任务中，哪个最耗时且价值最低？"
9. "如果有一个魔法棒可以消除一个工作烦恼，你会选什么？"
10. "告诉我一次你因为工具/流程问题而导致的失误或延迟。"
11. (Probe) "当时你是怎么解决的？那个方案满意吗？"

### Solution Exploration (8 min)
12. "你试过哪些方法来解决这些问题？效果如何？"
13. "如果你来设计理想的解决方案，它应该是什么样的？"
14. "什么样的新工具你会愿意尝试？什么样的你会直接拒绝？"

### Wrap-up (5 min)
15. "还有什么我们没问到但你认为重要的事情吗？"
16. "我们可以后续再联系你验证一些想法吗？"
17. 感谢 + 解释下一步

## 关键技巧
- **沉默的力量**: 问完问题后等待 5-7 秒不要说话
- **追问 5 次 Why**: 深挖表面回答背后的真实原因
- **具体化**: 把"有时候"变成"每周大概3次"
- **情绪标记**: 注意语调变化和表情反应
```

---

## 问卷设计系统

### 科学问卷框架

```yaml
survey_design:
  principles:
    - avoid_leading_questions: true
    - avoid_double_barreled: true
    - use_balanced_scales: true
    - randomize_option_order: true
    - keep_under_10_minutes: true
    
  section_structure:
    - section: screening
      purpose: 过滤不合格参与者
      questions: 3-5
      
    - section: behavior
      purpose: 了解当前行为模式
      questions: 8-12
      
    - section: attitude
      purpose: 测量态度和偏好
      questions: 6-10
      
    - section: psychographics
      purpose: 心理特征和价值观
      questions: 5-8
      
    - section: demographics
      purpose: 人口统计学信息
      questions: 5-7
      
    - section: open_ended
      purpose: 收集定性反馈
      questions: 2-3
```

---

*创新技能 - 结合 Alan Cooper Persona 方法论 + Google Ventures User Research 实践*
*版本: 2.0.0 | 最后更新: 2026-04-27*
