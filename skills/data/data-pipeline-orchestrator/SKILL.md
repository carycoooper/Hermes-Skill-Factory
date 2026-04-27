---
name: data-pipeline-orchestrator
description: "Hermes-Skill-Data-Pipeline-Orchestrator - 企业级ETL数据管道编排器，支持多源数据抽取、转换、加载(ETL)，可视化管道设计和实时监控告警。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [etl, data-pipeline, orchestration, analytics, transformation, monitoring, airflow, dbt]
    related_skills: [deep-research, data-visualization-generator, finance-tracker]
    requires_toolsets: [web, terminal]
    config:
      - key: default_schedule
        default: daily
      - key: max_parallel_tasks
        default: 5
      - key: retry_policy
        default: exponential_backoff
---

# Hermes-Skill-Data-Pipeline-Orchestrator (数据管道编排器)

## 概述

**Hermes-Skill-Data-Pipeline-Orchestrator** 是一个强大的 ETL（Extract-Transform-Load）数据管道管理和编排系统。它帮助 AI Agent 设计、执行、监控和优化复杂的数据流水线，支持从多种数据源提取数据，进行灵活转换，并加载到目标存储中。

### 核心能力

- **多源连接器**: 50+ 数据源适配器（数据库/API/文件/流式）
- **可视化编排**: DAG（有向无环图）管道设计
- **增量同步**: Change Data Capture (CDC) 支持
- **数据质量**: 内置数据校验和清洗规则
- **实时监控**: 管道健康度仪表盘和告警
- **Schema 演进**: 自动处理表结构变更

---

## 适用场景

| 场景 | 描述 |
|------|------|
| **数据仓库建设** | 从业务库同步数据到数仓 |
| **报表自动化** | 定时生成运营/财务报表 |
| **数据迁移** | 系统间批量数据搬迁 |
| **实时分析** | 流式数据处理和分析 |
| **数据清洗** | 标准化和去重脏数据 |
| **API 数据聚合** | 整合多个第三方 API 数据 |

---

## 快速开始

```bash
# 创建新管道
/pipeline create --name etl_sales_data --schedule "0 2 * * *"

# 定义数据源
/pipeline source add postgresql --host db.prod.com --database sales

# 定义转换步骤
/pipeline transform add deduplicate --key user_id
/pipeline transform add enrich --lookup geo_data --on country_code

# 定义目标
/pipeline target add bigquery --dataset analytics

# 运行管道
/pipeline run --name etl_sales_data

# 查看运行历史
/pipeline history --name etl_sales_data --last 10
```

---

## 架构设计

### 管道组成

```
┌──────────────────────────────────────────────────────┐
│                  Data Pipeline                        │
│                                                       │
│  ┌────────┐   ┌──────────┐   ┌────────┐   ┌────────┐ │
│  │ Source │ → │ Transform │ → │ Validate│ → │ Target │ │
│  │ 抽取   │   │ 转换      │   │ 校验   │   │ 加载   │ │
│  └────────┘   └──────────┘   └────────┘   └────────┘ │
│       ↑              ↑            ↑           ↑     │
│  多源连接       数据清洗      质量规则     目标存储    │
│                                                       │
│  ┌──────────────────────────────────────────────┐   │
│  │          Orchestration & Monitoring           │   │
│  │  调度 / 重试 / 告警 / 版本管理 / 血缘追踪     │   │
│  └──────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────┘
```

### 支持的数据源

| 类别 | 支持的源 |
|------|---------|
| **关系型数据库** | PostgreSQL, MySQL, SQL Server, Oracle, SQLite |
| **NoSQL** | MongoDB, Redis, Cassandra, Elasticsearch |
| **云数据仓库** | BigQuery, Snowflake, Redshift, Databricks |
| **文件系统** | CSV, JSON, Parquet, Avro, Excel, S3/GCS/Azure Blob |
| **API/流式** | REST APIs, Kafka, Kinesis, Pub/Sub, WebSocket |
| **SaaS** | Salesforce, HubSpot, Stripe, Google Analytics |

---

## 核心功能

### 1. 数据抽取 (Extract)

```yaml
# PostgreSQL 数据源配置
source:
  type: postgresql
  connection:
    host: ${DB_HOST}
    port: 5432
    database: production
    user: ${DB_USER}
    password: ${DB_PASSWORD}
    
  query: |
    SELECT 
      u.id,
      u.email,
      u.created_at,
      o.id as order_id,
      o.total_amount,
      o.status
    FROM users u
    LEFT JOIN orders o ON u.id = o.user_id
    WHERE u.updated_at > '${last_sync_time}'
    
  # 增量同步策略
  sync_mode: incremental
  watermark_column: updated_at
  batch_size: 10000
```

### 2. 数据转换 (Transform)

```python
# 内置转换操作
transforms:
  # 数据清洗
  - type: clean
    operations:
      - trim_whitespace: [email, name]
      - lowercase: [email]
      - remove_nulls: true
      
  # 数据标准化
  - type: standardize
    operations:
      - format_date: [created_at, updated_at]  # ISO 8601
      - format_currency: [total_amount]        # 2位小数
      
  # 数据丰富
  - type: enrich
    operations:
      - lookup:
          source: geo_reference
          on: country_code
          fields: [country_name, region, timezone]
          
  # 数据聚合
  - type: aggregate
    group_by: [user_id]
    metrics:
      total_orders: count(order_id)
      total_revenue: sum(total_amount)
      avg_order_value: avg(total_amount)
```

### 3. 数据校验 (Validate)

```yaml
validation_rules:
  # 必填字段检查
  - type: not_null
    fields: [id, email, created_at]
    
  # 唯一性约束
  - type: unique
    field: email
    
  # 范围校验
  - type: range
    field: total_amount
    min: 0
    max: 1000000
    
  # 正则表达式
  - type: pattern
    field: email
    regex: '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    
  # 自定义规则
  - type: custom
    name: valid_order_status
    condition: "status IN ('pending', 'processing', 'shipped', 'delivered', 'cancelled')"
    
# 校验失败处理
on_failure:
  action: quarantine  # quarantine / drop / abort
  notify: ["data-team@company.com"]
```

### 4. 数据加载 (Load)

```yaml
target:
  type: bigquery
  project: my-analytics-project
  dataset: sales_analytics
  table: user_orders
  
  load_strategy: merge  # append / replace / merge_upsert
  merge_keys: [user_id, order_id]
  
  partitioning:
    type: time
    column: created_at
    granularity: day
    
  clustering:
    columns: [user_id, status]
```

---

## 管道调度与管理

### 调度策略

```yaml
scheduling:
  # Cron 表达式
  cron: "0 */4 * * *"  # 每4小时
  
  # 或事件触发
  trigger:
    type: event
    source: webhook
    endpoint: /pipeline/trigger/sales-etl
    
  # 依赖链
  depends_on:
    - pipeline: raw_data_ingestion
      wait_for: success
    - pipeline: dimension_refresh
      wait_for: success
      
  # SLA 保证
  sla:
    timeout: 2h
    alert_if_exceeded: true
```

### 监控与告警

```
实时指标:
├── 运行状态 (running/success/failed/skipped)
├── 数据量统计 (rows_processed/rows_loaded)
├── 性能指标 (duration/throughput/latency_p99)
├── 错误率 (error_count/error_rate)
└── 资源使用 (cpu/memory/network)

告警规则:
├── 管道失败 → 立即通知 PagerDuty
├── 运行超时 → 15分钟内警告
├── 数据量异常 (>3σ) → 每日摘要
└── 源连接失败 → 重试3次后升级
```

---

## 高级特性

### 1. Schema 演进

当上游表结构发生变化时：
- **自动检测**: 比较新旧 schema 差异
- **兼容处理**: 新列自动添加，删除列标记 deprecated
- **类型转换**: 自动映射兼容类型
- **回滚机制**: 保留历史 schema 版本

### 2. 数据血缘追踪

```
source_table.column_a
  → transform_step_1.new_column_b
    → target_table.final_metric_c

完整血缘图: 可视化展示数据从源头到终点的流转路径
影响分析: 上游变更会影响哪些下游管道和报表
```

### 3. 渐进式数据处理

```yaml
# 分批处理大数据集
batching:
  enabled: true
  batch_size: 50000
  parallel_batches: 4
  
# 断点续传
checkpoint:
  enabled: true
  storage: s3://checkpoints/my-pipeline/
  
# 幂等保证
idempotency:
  strategy: hash_based  # hash_based / timestamp / custom
  dedup_window: 7d
```

---

## 最佳实践

### ✅ 推荐做法

1. **模块化设计**: 单一职责原则，每个管道专注一个数据域
2. **版本化管理**: 管道配置纳入 Git 版本控制
3. **渐进式上线**: shadow mode → canary → full rollout
4. **数据质量左移**: 在源头而非末端做质量检查
5. **成本意识**: 监控云资源消耗，优化查询计划

### ❌ 避免陷阱

- **大事务**: 避免单次加载超过 GB 级数据
- **硬编码环境**: 所有配置通过环境变量注入
- **忽略错误处理**: 每个步骤都要考虑失败场景
- **缺少文档**: 管道逻辑必须有清晰注释

---

## 验证清单

- [ ] 数据源连接测试通过
- [ ] 增量同步水印正确设置
- [ ] 转换逻辑单元测试覆盖
- [ ] 数据质量规则已配置
- [ ] 目标表分区策略合理
- [ ] 调度和依赖关系正确
- [ ] 告警通知渠道可用
- [ ] 回滚方案已准备
- [ ] 生产环境灰度发布计划
- [ ] 运维手册已完成

---

*基于 OpenClaw Data Pipeline Orchestrator (15K+ 安装) 优化迁移*
*版本: 2.0.0 | 最后更新: 2026-04-27*
