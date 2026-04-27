---
name: knowledge-base-manager
description: "Hermes-Skill-Knowledge-Base-Manager - 个人/团队知识库构建与管理工具，支持知识采集、结构化组织、语义搜索、关联发现和持续进化。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [knowledge-base, notes, wiki, obsidian, notion, semantic-search, zettelkasten, second-brain]
    related_skills: [deep-research, summarize, self-improving-agent]
    requires_toolsets: [web, terminal]
    config:
      - key: storage_backend
        default: local  # local/notion/obsidian/git
      - key: index_engine
        default: semantic
      - key: auto_tagging
        default: true
---

# Hermes-Skill-Knowledge-Base-Manager (知识库管理器)

## 概述

**Hermes-Skill-Knowledge-Base-Manager** 是一套完整的个人/团队知识管理系统（PKM），帮助 AI Agent 和用户构建"第二大脑"。它支持从多种渠道采集知识，通过语义理解和自动分类进行结构化组织，并提供强大的关联发现和检索能力。

### 核心理念

> **"你的大脑用来思考，而不是用来存储。"**
> 
> — Building a Second Brain, Tiago Forte

### 核心能力

- **多源采集**: 网页/文档/PDF/邮件/聊天记录/代码注释
- **智能提取**: 自动识别关键概念、实体、行动项
- **结构化组织**: PARA 方法论 + Zettelkasten 卡片盒笔记法
- **语义搜索**: 不只是关键词匹配，而是理解意图
- **知识图谱**: 发现隐藏的知识关联和知识缺口
- **主动推送**: 基于上下文的相关知识推荐

---

## 知识框架

### PARA 方法论

```
你的知识库结构:

📁 Projects (项目)
   └── 有明确截止日期的短期任务
   └── 例: Q2 产品发布计划 / 客户提案 POC

📁 Areas (领域)
   └── 持续负责的责任范围
   └── 例: 团队管理 / 个人健康 / 财务规划

📁 Resources (资源)
   └── 未来可能用到的参考资料
   └── 例: Python 学习笔记 / 设计模式大全 / 行业报告

📁 Archives (归档)
   └── 已完成的项目和不活跃的领域
   └── 例: 2025 年度总结 / 已结束的项目
```

### 卡片盒笔记法 (Zettelkasten)

```
笔记类型:

💡 永久笔记 (Permanent Notes)
   └── 独立、原子化的想法单元
   └── 一张卡片 = 一个明确的想法
   └── 示例: "微服务架构中服务发现的3种模式"

📝 文献笔记 (Literature Notes)
   └── 阅读/学习时的摘录和思考
   └── 用自己的话重写，而非复制粘贴

🔖 闪念笔记 (Fleeting Notes)
   └── 快速捕捉临时想法
   └── 每日/每周整理到永久笔记

📋 项目笔记 (Project Notes)
   └── 特定项目的上下文信息
   └── 项目结束后归档
```

---

## 快速开始

```bash
# 初始化知识库
/kb init --name my-second-brain --method para --storage local

# 采集知识
/kb capture "今天学到了 React Server Components 可以减少客户端JS bundle大小" --tag react,performance
/kb capture-web https://example.com/article/about-ai --extract
/kb import-notes obsidian-vault/

# 组织和管理
/kb organize --auto-classify
/kb link-discover  # 发现笔记间的隐含关联
/kb review-daily  # 每日回顾待整理的闪念笔记

# 搜索和使用
/kb search "如何优化数据库查询性能" --semantic
/kb graph show --topic "system-design"  # 展示某主题的知识图谱
/kb suggest --context "正在写一篇关于分布式系统的文章"

# 输出和同步
/kb export --format markdown --output ./exported/
/kb sync --target notion  # 同步到 Notion
```

---

## 功能模块

### 1. 知识采集

#### 网页采集

```bash
/kb capture-web URL [--options]

# 提取内容类型:
--full-article     # 完整文章正文
--highlights       # 仅关键段落
--summary          # AI 生成摘要
--key-points        # 要点列表
--qa               # 问答对形式

# 示例
/kb capture-web "https://blog.example.com/postgres-indexing" \
  --tags database,postgres,optimization \
  --extract key-points \
  --connect-to existing-note-about-postgres
```

#### 文档导入

```
支持格式:
├── Markdown (.md)
├── PDF (.pdf) - OCR + 文本提取
├── Word (.docx)
├── PowerPoint (.pptx)
├── Excel (.xlsx)
├── 纯文本 (.txt)
├── 代码文件 (.py/.js/.java 等)
└── 图片 (.png/.jpg) - OCR 文字识别
```

#### 对话/会议记录

```bash
# 从转录文本提取知识点
/kb extract meeting_transcript.txt \
  --type meeting \
  --extract action-items,decisions,key-discussions \
  --participants "Alice,Bob,Charlie"
```

### 2. 智能处理

#### 自动标签和分类

```
AI 分析每个笔记并自动:
├── 提取关键词 (3-5 个核心词)
├── 分类到 PARA 四大区域
├── 识别实体 (人名/公司/技术/概念)
├── 评估重要程度 (⭐1-5)
└── 设定复习间隔 (基于遗忘曲线)
```

#### 关联发现

```
知识图谱构建:

笔记 A: "PostgreSQL 的 B-Tree 索引适合范围查询"
         ↓ 共享概念: "索引"
笔记 B: "MongoDB 使用 B-Tree 作为默认索引"
         ↓ 共享技术: "数据库"
笔记 C: "数据库索引设计的通用原则..."

发现路径:
A ←→ B (直接关联: 都讨论 B-Tree 索引)
A → C (间接关联: 通过索引→数据库原则)
B → C (间接关联: 通过 MongoDB→数据库)
```

### 3. 语义搜索

```bash
# 传统关键词搜索
/kb search "PostgreSQL optimization"

# 语义搜索（理解意图）
/kb search "我的数据库查询很慢怎么办"
# 匹配结果包括:
# - "PostgreSQL EXPLAIN ANALYZE 使用指南"
# - "数据库索引设计最佳实践"  
# - "慢查询排查 checklist"
# - "MySQL vs PostgreSQL 性能对比"

# 过滤条件
/kb search "microservices architecture" \
  --filter tag:architecture \
  --filter importance:>=4 \
  --filter created:>2026-01-01 \
  --limit 10
```

### 4. 知识进化

```
笔记生命周期:

新创建 → 待整理 → 已链接 → 成熟 → 归档
  ↑                                    │
  └──── 定期复习 / 更新 / 关联扩展 ────┘

复习调度 (Spaced Repetition):
├── 第 1 天后: 快速浏览
├── 第 3 天后: 回顾要点
├── 第 7 天后: 思考应用场景
├── 第 30 天后: 检查是否过时
└── 第 90 天后: 决定保留/合并/归档
```

---

## 高级功能

### 1. 知识图谱可视化

```
图形视图:
├── 力导向图 (Force Graph): 全局知识网络
├── 树状图 (Tree Map): 按层级展开
├── 时间线 (Timeline): 知识积累历程
└── 集群图 (Cluster): 自动发现知识社区

交互操作:
├── 点击节点查看笔记详情
├── 悬停显示关联强度
├── 缩放/拖拽/聚焦子图
└── 导出为 PNG/SVG
```

### 2. 协作知识库

```yaml
# 团队配置
team:
  members:
    - role: owner
      permissions: [read, write, admin]
    - role: editor
      permissions: [read, write]
    - role: viewer
      permissions: [read]
      
  workflows:
    - name: review
      trigger: new_note
      actions: [assign_reviewer, notify_team]
      
    - name: publish
      trigger: note_approved
      actions: [add_to_public_space, announce]
```

### 3. 与外部工具集成

```
已支持集成:
├── Obsidian (双向链接 + Dataview)
├── Notion API (数据库同步)
├── GitHub (Markdown 仓库作为知识库)
├── Readwise (阅读高亮同步)
├── Pocket/Instapaper (稍后阅读)
├── Twitter/X (收藏推文)
└── RSS Feeds (博客订阅自动采集)
```

---

## 最佳实践

### ✅ CODE of Knowledge Management

**C**apture Less - 少采集，多消化。不是所有内容都值得保存。
**O**rganize Actively - 主动组织，不要让收件箱堆积。
**D**iscover Connections - 发现关联比收集更重要。
**E**volve Continuously - 知识是活的，定期更新和淘汰。

### ❌ 常见陷阱

- **收藏夹坟墓**: 只收藏不阅读/不整理
- **完美主义**: 等到"准备好了"再记——永远不会有那一天
- **过度分层**: 太深的目录结构反而增加查找成本
- **重复造轮子**: 先搜索是否已有类似笔记

---

*创新技能 - 结合 PARA + Zettelkasten + 语义搜索的现代 PKM 系统*
*版本: 2.0.0 | 最后更新: 2026-04-27*
