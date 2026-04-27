---
name: system-design-prep
description: "Hermes-Skill-System-Design-Interview-Prep - 系统设计面试准备专家，覆盖分布式系统、大规模架构、数据库设计、缓存策略、消息队列等高频面试题库和解题框架。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [system-design, interview-prep, distributed-systems, architecture, scalability, database-design, cache, message-queue, tech-interview]
    related_skills: [data-pipeline-orchestrator, cicd-pipeline-manager, code-review-pro, api-testing-suite]
    requires_toolsets: [web, terminal]
    config:
      - key: target_company_tier
        default: faang  # faang/unicorn/mid-market/startup
      - key: focus_area
        default: balanced
---

# Hermes-Skill-System-Design-Interview-Prep (系统设计面试准备专家)

## 概述

**Hermes-Skill-System-Design-Interview-Prep** 是一个全面的系统设计面试训练系统。它汇集了 FAANG/字节跳动/阿里等顶级科技公司的高频面试题，提供结构化的解题框架、架构模式库和模拟面试功能。

### 核心能力

- **题库**: 100+ 高频系统设计题目（分类/难度/公司标签）
- **解题框架**: STEP-BY-STEP 标准化解题方法论
- **架构模式**: 20+ 经典设计模式（每种含优缺点和适用场景）
- **估算练习**: Fermi Problem 训练
- **模拟面试**: AI 扮演面试官进行实战演练
- **知识图谱**: 分布式系统概念关联图

---

## 解题框架: DESIGN 法则

```
D - Understand the Requirements and Constraints (明确需求与约束)
E - High-Level Architecture Estimate (高层架构估算)
S - Design Core Components (设计核心组件)
I - Scale the System (扩展系统)
G - Identify Bottlenecks and Optimize (识别瓶颈并优化)
N - Wrap Up with Summary (总结收尾)
```

### 详细步骤

#### D: Requirements (需求澄清)

```
必须澄清的问题清单:

Functional Requirements (功能需求):
├── 核心功能是什么？（CRUD？读写比例？）
├── 用户角色有哪些？（普通用户/VIP/管理员？）
├── 数据模型是什么？（实体关系？）
├── 需要实时性吗？（秒级/分钟级？）
└── 有什么特殊业务规则？

Non-Functional Requirements (非功能需求):
├── Scale: QPS? DAU? Data volume?
├── Latency: P99 要求?
├── Consistency: Strong / Eventual?
├── Availability: 几个 9?
├── Reliability: 数据丢失容忍度？
└── Security: 认证/授权/加密？

Back-of-Envelope Estimation (粗略估算):
├── Storage: 每条记录多大 × 总记录数
├── Bandwidth: 读QPS × 平均响应大小
├── Memory Cache: 热数据占比 × 总量
└── Server Count: 总QPS / 单机容量
```

#### E: Estimate (规模估算)

```
常用估算参考值:

DAU 估算:
├── 全球级服务: 1B+ DAU (WhatsApp, Facebook)
├── 国家级: 100M+ DAU (微信, TikTok China)
├── 区域级: 10M+ DAU (Uber US, DoorDash)
├── 垂直领域: 1M+ DAU (Notion, Figma)
└── 初创产品: 10K-100K DAU

QPS 估算:
├── 日活用户平均每日请求: 10-30 次
├── 峰值 QPS ≈ 日均 QPS × (3-5x)
├── 写操作通常是读操作的 10-30%
└── 热点数据: 20% 的内容占 80% 的访问 (80/20 Rule)

存储估算:
├── 用户数据: ~1KB/user (profile + settings)
├── 文本内容: ~5KB/post (social media)
├── 图片: ~500KB/photo (compressed)
├── 视频: ~50MB/video (1080p 5min)
└── 元数据: ~100 bytes/object
```

#### S: Design Components (组件设计)

```
分层架构:

┌─────────────────────────────────────┐
│         Client Layer                 │
│   Web / Mobile App / Third-party API │
├─────────────────────────────────────┤
│         Load Balancer                │
│   L7 (Nginx/ALB) + L4 (Network LB)   │
├─────────────────────────────────────┤
│         API Gateway                  │
│   Rate Limit / Auth / Routing         │
├─────────────────────────────────────┤
│       Service Layer (Microservices)  │
│   User Service / Content Service ... │
├─────────────────────────────────────┤
│         Data Layer                   │
│   Cache (Redis) / DB (PostgreSQL)     │
│   Search (Elasticsearch) / Object (S3)│
├─────────────────────────────────────┤
│       Infrastructure                 │
│   Message Queue / CDN / Monitoring    │
└─────────────────────────────────────┘
```

---

## 高频面试题库

### Tier 1: 必备题目 (出现率 >80%)

| # | 题目 | 核心考点 | 难度 | 出现公司 |
|---|------|---------|------|---------|
| 1 | **Design a URL Shortener** (TinyURL) | Hashing / Base62 / Cache / DB Sharding | ⭐⭐ | All |
| 2 | **Design Instagram** | Feed Generation / Media Storage / Timeline | ⭐⭐⭐ | Meta, ByteDance |
| 3 | **Design a Chat System** (WhatsApp) | WebSocket / Message Queue / Online Status | ⭐⭐⭐ | Meta, WeChat |
| 4 | **Design a Notification System** | Multi-channel / Template Engine / Rate Limit | ⭐⭐ | Uber, Airbnb |
| 5 | **Design Google Search** | Crawler / Indexer / Ranking / MapReduce | ⭐⭐⭐⭐ | Google, Baidu |
| 6 | **Design YouTube** | Video Upload / Transcoding / CDN / Recommendation | ⭐⭐⭐⭐ | Google, TikTok |
| 7 | **Design a Distributed Key-Value Store** | Consistent Hashing / Replication / Gossip | ⭐⭐⭐⭐ | Amazon, Redis Labs |
| 8 | **Design Google Maps** | Quadtree / Geohash / Real-time Updates | ⭐⭐⭐⭐ | Google, Amap |

### Tier 2: 进阶题目 (出现率 50-80%)

| # | 题目 | 核心考点 | 难度 |
|---|------|---------|------|
| 9 | **Design a Rate Limiter** | Token Bucket / Sliding Window / Redis Lua | ⭐⭐ |
| 10 | **Design a News Feed System** | Fan-out / Push vs Pull / Rank / Cache | ⭐⭐⭐ |
| 11 | **Design an E-commerce Platform** | Order Management / Inventory / Payment | ⭐⭐⭐ |
| 12 | **Design a File Sharing Service** (Dropbox) | Delta Sync / Conflict Resolution | ⭐⭐⭐ |
| 13 | **Design a Streaming Platform** (Netflix) | Adaptive Bitrate / CDN / Recommendation | ⭐⭐⭐⭐ |
| 14 | **Design a Payment System** | ACID / Idempotency / Ledger | ⭐⭐⭐ |
| 15 | **Design a Collaborative Editing Tool** (Google Docs) | OT / CRDT / Conflict Resolution | ⭐⭐⭐⭐ |
| 16 | **Design a Ride-Sharing App** (Uber) | Geo-spatial / Matching Algorithm | ⭐⭐⭐ |

### 题目详解示例: URL Shortener

```markdown
## Design a URL Shortener (类似 bit.ly / TinyURL)

### Step 1: Requirements Clarification
**Functional:**
- 输入长 URL → 返回短 URL
- 访问短 URL → 重定向到长 URL
- 可选: 自定义短码 / 过期时间 / 分析统计

**Non-Functional:**
- 低延迟: 重定向 < 200ms (P99)
- 高可用: 99.99% uptime
- 不可变: 短链接一旦创建不可更改
- 可预测: 短链接长度固定 (6-7 字符)

**Scale Estimates:**
- 读写比: 100:1 (读远多于写)
- 新 URL 产生速率: ~1K/sec
- 重定向请求: ~100K/sec (峰值)
- 存储: 5 年累积 ~150 亿条记录
- 每条记录: ~500 bytes (长URL + 元数据)
- 总存储: ~7.5 TB (5年)

### Step 2: API Design
```
POST /api/v1/shorten
Request: { "long_url": "https://example.com/very/long/path", "custom_alias": null }
Response: { "short_url": "https://short.ly/abc123", "created_at": "2026-04-27T..." }

GET /api/v1/{short_code}
Response: 301 Redirect → Location: https://example.com/very/long/path
```

### Step 3: Database Schema
```sql
CREATE TABLE url_mappings (
    id              BIGSERIAL PRIMARY KEY,
    short_code       CHAR(7) NOT NULL UNIQUE,
    long_url         TEXT NOT NULL,
    created_by       UUID,
    created_at       TIMESTAMPTZ DEFAULT NOW(),
    expires_at       TIMESTAMPTZ,           -- NULL = never expire
    click_count      BIGINT DEFAULT 0,
    
    INDEX idx_short_code (short_code),
    INDEX idx_created_by (created_by),
    INDEX idx_expires (expires_at) WHERE expires_at IS NOT NULL
);
```

### Step 4: Short Code Generation
**Option A: Base62 Encoding (Recommended)**
```
Characters: a-z (26) + A-Z (26) + 0-9 (10) = 62 chars
Length: 7 characters → 62^7 ≈ 3.5 trillion unique codes

Algorithm:
1. Use auto-increment ID from DB (or Snowflake ID)
2. Convert to base62
3. Example: ID 125,000,000 → "abc123z"

Pros: No collision, simple, predictable length
Cons: Sequentially predictable (can be mitigated)
```

**Option B: MD5/SHA Hash**
```
1. hash = MD5(long_url) → 128-bit hex
2. Take first 6-7 characters
3. Check for collision, if exists take next 6-7

Pros: Uniform distribution
Cons: Collision handling complexity
```

### Step 5: Scaling Strategy
```
Read Path Optimization (99% of traffic):
┌─ Browser requests short.ly/abc123
├─ DNS → CDN Edge (Cache popular redirects)
├─ If miss → Application Server
├─ Redis Lookup (short_code → long_url)
├─ If miss → PostgreSQL Query
└─ Return 301 Redirect + Update Redis

Write Path:
├─ Generate short_code
├─ Insert into PostgreSQL
├─ Invalidate/Update Redis cache
└─ Return result

Database Scaling:
├─ Sharding by short_code prefix (first char → shard)
├─ Read replicas for read-heavy workload
├─ Connection pooling (PgBouncer)
└─ Archive old entries (>1 year) to cold storage
```

### Trade-offs Discussion
| Decision | Option Chosen | Alternative | Reason |
|----------|--------------|-------------|--------|
| Short code generation | Base62 (auto-increment) | Hash | Simpler, no collisions |
| Cache layer | Redis | Memcached | Richer data structures |
| Redirect type | 301 Permanent | 302 Temporary | Browser caching benefit |
| ID generation | DB Auto-increment | Snowflake | Simpler for this scale |
```

---

## 架构模式速查表

### 数据库相关

| 模式 | 适用场景 | 代表系统 |
|------|---------|---------|
| **Master-Slave** | 读多写少 | 传统 Web 应用 |
| **Master-Master** | 多地域写入 | 全球社交网络 |
| **Sharding** | 超大数据集 | Instagram 用户数据 |
| **Follower Read** | 最终一致性可接受 | News Feed |
| **CQRS** | 读写模型差异大 | E-commerce Inventory |
| **Event Sourcing** | 需完整审计轨迹 | Financial Systems |
| **Polyglot Persistence** | 不同数据不同存储 | Modern Microservices |

### 缓存策略

| 策略 | 说明 | 适用场景 |
|------|------|---------|
| **Cache Aside** | 先查缓存，miss 再查 DB | 通用 |
| **Write Through** | 写入同时更新缓存 | 强一致性要求 |
| **Write Behind** | 异步批量写入 | 写入频率高 |
| **Refresh Ahead** | 主动预加载热点 | 可预测的热数据 |
| **LRU/LFU** | 淘汰策略选择 | 内存有限 |

### 消息队列模式

| 模式 | Kafka | RabbitMQ | Redis Streams |
|------|-------|----------|--------------|
| **Pub/Sub** | ✅ | ✅ | ✅ |
| **Work Queue** | Consumer Group | ✅ | Consumer Group |
| **Topic Partitioning** | ✅ | Exchanges | ✅ |
| **Exactly-Once** | ✅ (Transactional) | Publisher Confirms | ✅ |
| **Message Replay** | ✅ (Retention) | TTL | ✅ |
| **Throughput** | Very High | Medium | High |
| **Latency** | ms | μs | sub-ms |

---

## 模拟面试

```bash
# 开始模拟面试
/sdinterview start --topic url-shortener --difficulty mid --time 30min

# 练习特定环节
/sdinterview practice estimation --topic "How many photos does Instagram store per day?"
/sdinterview practice tradeoffs --topic "SQL vs NoSQL choice"

# 评估表现
/sdinterview review --session last --criteria clarity,completeness,tradeoffs,communication
```

---

*基于 Grokking the System Design Interview + System Design Primer 最佳实践*
*版本: 2.0.0 | 最后更新: 2026-04-27*
