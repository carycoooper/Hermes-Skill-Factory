---
name: api-testing-suite
description: "Hermes-Skill-API-Testing-Suite - 全面的API测试框架，支持REST/GraphQL/WebSocket协议的自动化测试、性能基准测试和安全扫描。基于OpenClaw热门API测试技能优化增强。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [api, testing, rest, graphql, websocket, automation, ci-cd, performance, security]
    related_skills: [code-review-pro, security-auditor, cicd-pipeline-manager]
    requires_toolsets: [web, terminal]
    config:
      - key: default_timeout
        default: 30
      - key: max_retries
        default: 3
      - key: report_format
        default: markdown
---

# Hermes-Skill-API-Testing-Suite (API 测试套件)

## 概述

**Hermes-Skill-API-Testing-Suite** 是一个企业级的 API 自动化测试框架，专为 AI Agent 设计。它支持 REST、GraphQL 和 WebSocket 协议的全面测试覆盖，包括功能测试、性能基准测试和安全漏洞扫描。

### 核心价值

- **多协议支持**: REST / GraphQL / WebSocket 统一测试接口
- **智能测试生成**: 根据API文档自动生成测试用例
- **性能基线检测**: 自动建立性能基线并检测回归
- **安全扫描集成**: OWASP API Security Top 10 检测
- **CI/CD 原生**: 与 GitHub Actions / GitLab CI 无缝集成

---

## 适用场景

| 场景 | 用途 |
|------|------|
| **API 开发验证** | 开发完成后自动验证所有端点 |
| **回归测试** | 每次代码变更后运行完整测试套件 |
| **性能监控** | 定期执行性能基准测试 |
| **安全审计** | 扫描 API 安全漏洞和配置问题 |
| **合同测试** | 验证 API 契约不被破坏 |
| **Mock 服务** | 在前端开发时模拟后端 API |

---

## 快速开始

```bash
# 初始化测试项目
/api-testing init --name my-api-tests --protocol rest

# 从 OpenAPI/Swagger 规范导入
/api-testing import openapi.json

# 运行全部测试
/api-testing run --all

# 运行特定测试组
/api-testing run --group auth

# 生成报告
/api-testing report --format html
```

---

## 功能模块

### 1. REST API 测试

```
支持方法: GET / POST / PUT / DELETE / PATCH / HEAD / OPTIONS
特性:
  - URL 参数和查询字符串处理
  - 请求头自定义（Content-Type, Authorization 等）
  - JSON / XML / Form-Data / Multipart 请求体
  - Cookie 和 Session 管理
  - 重定向跟随控制
```

#### 示例：创建用户 API 测试

```yaml
# tests/rest/users/create_user.yaml
name: Create User API Test
endpoint: POST /api/v1/users
headers:
  Content-Type: application/json
  Authorization: Bearer {{auth_token}}
body:
  name: "Test User"
  email: "test@example.com"
  role: "user"
expectations:
  status_code: 201
  response_time_ms: < 500
  body_contains:
    - id
    - name
    - email
  schema_validate: user_schema.json
```

### 2. GraphQL 测试

```
特性:
  - Query / Mutation / Subscription 支持
  - 变量注入和参数化
  - 深度限制和复杂度分析
  - 字段级权限验证
  - N+1 查询检测
```

#### 示例：GraphQL 查询测试

```graphql
query GetUser($id: ID!) {
  user(id: $id) {
    id
    name
    email
    posts {
      id
      title
      comments {
        id
        content
      }
    }
  }
}

# Variables: { "id": "123" }
# Expectations: response within 200ms, no N+1 queries
```

### 3. WebSocket 测试

```
特性:
  - 连接生命周期管理
  - 消息发送/接收验证
  - 心跳检测
  - 断线重连测试
  - 消息顺序保证
```

### 4. 性能测试

```bash
# 负载测试
/api-testing performance --concurrent-users 100 --ramp-up 30s --duration 5m

# 压力测试
/api-testing stress --max-users 1000 --step 100 --step-duration 60s

# 基准对比
/api-testing baseline compare --baseline v1.0 --current v1.1
```

**性能指标收集:**
- 响应时间（P50/P90/P95/P99）
- 吞吐量（RPS）
- 错误率
- 资源使用率
- 并发能力

### 5. 安全扫描

```
OWASP API Security Top 10 检测项:

API1: Broken Object Level Authorization (BOLA)
API2: Broken Authentication
API3: Broken Object Property Level Authorization
API4: Unrestricted Resource Consumption
API5: Broken Function Level Authorization (BFLA)
API6: Mass Assignment
API7: Security Misconfiguration
API8: Injection
API9: Improper Assets Management
API10: Unsafe Consumption of APIs
```

---

## 操作流程

### Phase 1: 项目初始化

```
输入: API 文档 (OpenAPI/Swagger/GraphQL Schema)
  ↓
自动解析: 端点列表 + 参数定义 + 数据模型
  ↓
输出: 测试项目骨架
```

### Phase 2: 测试生成

```
选择策略:
  ├─ 正向测试 (Happy Path) → 验证正常流程
  ├─ 边界值测试 → 空值/极值/特殊字符
  ├─ 异常测试 → 错误码/超时/网络异常
  └─ 安全测试 → 注入/越权/BOLA
  ↓
生成: YAML 测试文件集合
```

### Phase 3: 执行与验证

```
并行执行测试用例
  ↓
收集结果 + 截图 + 日志
  ↓
生成: 测试报告 (HTML/Markdown/JUnit XML)
```

### Phase 4: 持续监控

```
CI/CD Pipeline 集成
  ↓
每次提交自动运行
  ↓
性能基线对比
  ↓
告警: 回归检测 + 性能退化通知
```

---

## 配置选项

### 全局配置 (config.yaml)

```yaml
api_testing:
  defaults:
    protocol: rest
    base_url: https://api.example.com
    timeout: 30s
    retries: 3
    
  auth:
    type: bearer_token
    token: "${API_TOKEN}"
    refresh_endpoint: /auth/refresh
    
  performance:
    thresholds:
      p50_response_ms: 200
      p95_response_ms: 1000
      error_rate_percent: 1
      
  reporting:
    format: html
    include_logs: true
    screenshots_on_failure: true
    
  integrations:
    slack_webhook: "${SLACK_WEBHOOK}"
    jira_project: TEST
    github_issues: true
```

---

## 高级特性

### 1. 智能 Mock 服务

```bash
# 启动 Mock 服务器
/api-testing mock start --port 3000

# 录制真实响应作为 Mock 数据
/api-testing mock record --endpoint /api/users

# 自定义 Mock 规则
/api-testing mock rule add --path /api/users --method GET --response '{"users": []}' --status 200
```

### 2. 数据驱动测试

```yaml
# 使用 CSV/JSON 数据源
data_source: test_data.csv
variables:
  - user_id
  - expected_status

# 每行数据生成一个独立测试用例
```

### 3. 链式请求测试

```yaml
# 步骤 1: 登录获取 token
- request: POST /auth/login
  body: { username: "{{user}}", password: "{{pass}}" }
  extract: { token: $.data.token }

# 步骤 2: 使用 token 创建资源
- request: POST /api/resources
  headers: { Authorization: Bearer {{token}} }
  
# 步骤 3: 验证资源创建成功
- request: GET /api/resources/{{resource_id}}
  expect: { status: 200 }
```

### 4. 合同测试 (Contract Testing)

```
Provider (服务端): 记录真实响应作为契约
Consumer (客户端): 验证响应符合契约
工具: Pact / Spring Cloud Contract 兼容格式
```

---

## 最佳实践

### ✅ 推荐做法

1. **测试金字塔**: 单元 > 集合 > E2E，API 测试属于中间层
2. **环境隔离**: dev/staging/prod 使用不同配置
3. **数据独立性**: 每个测试用例使用独立数据，不依赖执行顺序
4. **幂等性设计**: 重复运行结果一致
5. **版本化契约**: API 变更时更新测试契约

### ❌ 常见陷阱

- **硬编码数据**: 使用变量和环境配置
- **过度依赖 UI**: API 测试不应依赖前端状态
- **忽略清理**: 测试后清理创建的资源
- **单一测试文件**: 按模块/领域拆分测试

---

## 验证清单

部署前检查:

- [ ] 所有核心端点有对应测试用例
- [ ] 认证/授权流程完整覆盖
- [ ] 错误场景（400/401/403/404/500）已测试
- [ ] 性能基线已建立
- [ ] 安全扫描无高危漏洞
- [ ] CI/CD 流水线已配置
- [ ] 测试报告可自动生成
- [ ] Mock 服务可用于本地开发

---

## 故障排除

| 问题 | 可能原因 | 解决方案 |
|------|---------|---------|
| 连接超时 | 网络问题或服务未启动 | 检查 base_url 和网络连通性 |
| 认证失败 | Token 过期或权限不足 | 刷新 Token 或检查角色权限 |
| Schema 验证错误 | API 响应格式变化 | 更新 schema 或联系后端团队 |
| 性能退化 | 新代码引入延迟 | 对比 git blame 定位问题提交 |

---

*基于 OpenClaw API Testing Suite (22K+ 安装) 优化迁移至 Hermes 平台*
*版本: 2.0.0 | 最后更新: 2026-04-27*
