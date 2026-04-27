---
name: api-doc-generator
description: "Hermes-Skill-API-Doc-Generator - API文档自动生成器，从代码/OpenAPI规范/Postman集合自动生成专业API文档，支持Swagger UI、Redoc、Markdown多格式输出。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [api, documentation, swagger, openapi, redoc, postman, developer-experience, technical-writing]
    related_skills: [api-testing-suite, code-review-pro, frontend-design-assistant]
    requires_toolsets: [web, terminal]
    config:
      - key: output_format
        default: interactive_html
      - key: doc_style
        default: modern
      - key: include_examples
        default: true
---

# Hermes-Skill-API-Doc-Generator (API 文档自动生成器)

## 概述

**Hermes-Skill-API-Doc-Generator** 是一个智能 API 文档工程系统。它能从源代码、OpenAPI 规范、Postman Collection 或 cURL 命令自动生成专业、美观、交互式的 API 文档，大幅提升开发者体验（DX）。

### 核心能力

- **多源输入**: 代码注释 / OpenAPI 3.x / Postman / cURL / GraphQL Schema
- **多格式输出**: Swagger UI / Redoc / Markdown / PDF / Docusaurus
- **智能推断**: 自动识别请求/响应模型、错误码、认证方式
- **示例生成**: 基于 schema 自动填充真实感的示例数据
- **变更追踪**: 版本间 API 变更对比 (diff)
- **SDK 生成**: 从文档直接生成客户端 SDK (TypeScript/Python/Go)

---

## 支持的文档风格

| 风格 | 特点 | 适用场景 |
|------|------|---------|
| **Swagger UI** | 交互式测试 + Try it out | 内部团队开发 |
| **Redoc** | 三栏布局，响应式，美观 | 公开 API 展示 |
| **Stoplight** | 设计优先，交互式教程 | API 产品页 |
| **Slate** | Markdown 风格，左右分栏 | 技术博客 |
| **Docusaurus** | 完整文档站，支持搜索 | 大型项目 |
| **ReadTheDocs** | Sphinx 集成，版本切换 | 开源项目 |

---

## 快速开始

```bash
# 从 OpenAPI 规范生成
/api-doc generate --input openapi.yaml --output docs/ --theme redoc

# 从代码注释提取 (Python FastAPI)
/api-doc extract app/main.py --framework fastapi

# 从 Postman Collection 导入
/api-doc import postman_collection.json --convert openapi3

# 批量处理多个 API 项目
/api-doc batch process ./apis/* --output ./docs/ --format html+markdown

# 对比两个版本的 API 差异
/api-doc diff v1/openapi.yaml v2/openapi.yaml --output changelog.md

# 生成交互式文档站点
/api-doc site build --source ./specs --output ./site --theme redoc
```

---

## 输入源适配

### OpenAPI 3.x 规范

```yaml
# 输入: openapi.yaml
openapi: "3.1.0"
info:
  title: Hermes Skills API
  version: 2.0.0
  description: |
    ## Overview
    The **Hermes Skills API** provides programmatic access to 
    the complete collection of AI Agent skills.
    
    ### Authentication
    All endpoints require Bearer token authentication.
    Get your token from [Developer Portal](https://dev.hermes.ai).
    
servers:
  - url: https://api.hermes.ai/v2
    description: Production
    
  - url: https://staging-api.hermes.ai/v2
    description: Staging

paths:
  /skills:
    get:
      summary: List all available skills
      operationId: listSkills
      tags:
        - Skills
      
      parameters:
        - name: category
          in: query
          required: false
          schema:
            type: string
            enum: [productivity, development, ai, data]
          description: Filter by skill category
            
        - name: limit
          in: query
          schema:
            type: integer
            minimum: 1
            maximum: 100
            default: 20
          
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SkillList'
              examples:
                all_skills:
                  summary: Return first 20 skills
                  value:
                    skills:
                      - id: deep-research
                        name: Deep Research
                        category: research
                        popularity: 180000
                        
        '401':
          $ref: '#/components/responses/Unauthorized'
          
      security:
        - bearerAuth: []
        
    post:
      summary: Create a new custom skill
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateSkillRequest'
            example:
              name: My Custom Skill
              description: A skill for processing customer feedback
              category: productivity
              instructions: |
                You are an expert at analyzing customer feedback...
              version: 1.0.0
              
      responses:
        '201':
          description: Skill created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Skill'
                
components:
  schemas:
    SkillList:
      type: object
      properties:
        skills:
          type: array
          items:
            $ref: '#/components/schemas/SkillSummary'
        total:
          type: integer
        has_more:
          type: boolean
          
    Skill:
      type: object
      required: [id, name, category, version]
      properties:
        id:
          type: string
          format: uuid
          example: "550e8400-e29b-41d4-a716-446655440000"
        name:
          type: string
          maxLength: 100
          example: "Deep Research"
        description:
          type: string
          nullable: true
        category:
          $ref: '#/components/schemas/SkillCategory'
        version:
          type: string
          pattern: '^\d+\.\d+\.\d+$'
        created_at:
          type: string
          format: date-time
        github_url:
          type: string
          format: uri
          
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: |
        JWT Token obtained from `/auth/login` endpoint.
        Format: `Bearer <your_token_here>`
        
        Example: `Bearer eyJhbGciOiJIUzI1NiIs...`
```

### 代码注释提取

```python
# Python FastAPI 示例
"""
@module: User Management
@version: 2.0.0
"""

from fastapi import APIRouter, HTTPException
from pydantic import BaseModel, EmailStr
from typing import Optional

router = APIRouter(prefix="/users", tags=["Users"])

class UserCreate(BaseModel):
    """Request body for creating a new user.
    
    Attributes:
        email (EmailStr): User's email address (must be unique)
        name (str): Display name, 2-50 characters
        role (str): One of admin, editor, viewer (default: viewer)
        avatar_url (Optional[str]): Profile picture URL
    """
    email: EmailStr
    name: str = Field(..., min_length=2, max_length=50)
    role: str = Field("viewer", regex="^(admin|editor|viewer)$")
    avatar_url: Optional[str] = None

class UserResponse(BaseModel):
    """Response model for user objects.
    
    Automatically includes `id`, `created_at`, and `updated_at`.
    """
    id: str
    email: EmailStr
    name: str
    role: str
    avatar_url: Optional[str] = None
    created_at: datetime
    updated_at: datetime


@router.post(
    "",
    response_model=UserResponse,
    status_code=201,
    summary="Create a new user",
    description="""
    Creates a new user account. The email address must be unique.
    
    ## Permissions
    Requires `users:write` scope.
    
    ## Error Responses
    - **400**: Invalid input data (validation error details in body)
    - **409**: Email already exists
    - **422**: Unprocessable entity (Pydantic validation failure)
    
    ## Example Request
    ```json
    {
      "email": "alice@example.com",
      "name": "Alice Johnson",
      "role": "editor"
    }
    ```
    """,
    responses={
        201: {"description": "User created successfully", "model": UserResponse},
        400: {"description": "Validation error"},
        409: {"description": "Email already registered"},
    },
    openapi_extra={
        "x-codeSamples": [
            {
                "lang": "Python (requests)",
                "source": """
import requests
resp = requests.post("https://api.example.com/users", json={
    "email": "alice@example.com",
    "name": "Alice Johnson",
    "role": "editor"
}, headers={"Authorization": "Bearer <token>"})
print(resp.json())
                """
            },
            {
                "lang": "cURL",
                "source": """
curl -X POST https://api.example.com/users \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer <token>" \\
  -d '{"email":"alice@example.com","name":"Alice Johnson","role":"editor"}'
                """
            }
        ]
    }
)
async def create_user(body: UserCreate):
    """Create user endpoint implementation."""
    # ... business logic ...
    pass
```

---

## 文档增强功能

### 智能示例生成

```yaml
# 自动基于 schema 生成逼真的示例数据
example_generation:
  strategy: realistic  # minimal / realistic / edge-case
  
  rules:
    emails: use_realistic_domains  # alice@example.com not test@test.com
    ids: use_uuid_v4_format
    dates: use_recent_dates  # within last year
    names: use_diverse_names  # multicultural
    text: use_lorem_ipsum_for_long_text
    
  locale: zh-CN  # 中文友好示例
```

### 交互式 Try-It-Out

```
Swagger UI 功能:
├── 直接在浏览器中发送请求
├── 填写参数 → 点击 "Try it out" → Execute
├── 查看 Response + Status Code + Headers
├── 支持 OAuth2 流程交互式授权
├── cURL 命令一键复制
└── 多语言 SDK 代码片段生成

Redoc 增强:
├── 三栏布局: 左侧导航 + 中间内容 + 右侧代码
├── 搜索全局 API 端点
├── 按标签分组显示
├── 响应示例可折叠/展开
├── 移动端完美适配
└── 无需构建步骤，纯静态 HTML
```

### Changelog 生成

```bash
/api-doc changelog generate \
  --old v1.0/openapi.yaml \
  --new v2.0/openapi.yaml \
  --output CHANGELOG.md \
  --sections added,changed,deprecated,removed,security
```

**输出格式：**

```markdown
# API Changelog: v1.0 → v2.0

## 🆕 Added Endpoints (5)

### POST /skills/generate
**New!** AI-powered skill generation endpoint.

**Request Body:**
```json
{
  "description": "A skill for...",
  "category": "ai",
  "complexity": "advanced"
}
```

**Breaking Changes:** None

---

## 🔧 Changed Endpoints (8)

### GET /skills
**Changes:**
- ✅ New parameter: `include_deprecated` (boolean)
- ✅ Response field `popularity` changed from `integer` to `object`
  ```json
  // Before
  "popularity": 180000
  
  // After
  "popularity": {
    "total": 180000,
    "monthly": 15000,
    "trend": "up"
  }
  ```
- ⚠️ Default `limit` changed from 50 to 20

---

## ❌ Removed Endpoints (2)

### DELETE /skills/{id}/force
**Reason:** Security risk. Use soft delete via PATCH instead.

**Migration:** Use `PATCH /skills/{id}` with `{"status": "archived"}`

---

## 🔒 Security Changes

- **New**: Required scope `skills:generate` for POST /skills/generate
- **Changed**: Rate limit increased from 100→500 req/min for authenticated users
- **New**: CORS policy now allows `https://partner.example.com`
```

---

## 最佳实践

### ✅ 文档质量标准

```
每个端点必须包含:
├── 清晰的 Summary (< 80 字符)
├── 详细 Description (用途/场景/限制)
├── 所有参数的类型和约束
├── 至少 2 个 Request/Response 示例
├── 错误码完整列表 (4xx/5xx)
├── 认证要求说明
└── 至少一种语言的 SDK 示例代码

文档维护规则:
├── API 变更必须同步更新文档
├── 废弃端点标记 deprecated + 迁移指南
├── 版本号遵循 SemVer
└── 定期审查过期/不准确的示例
```

*创新技能 - 解决 API 文档维护痛点，让文档与代码同步*
*版本: 2.0.0 | 最后更新: 2026-04-27*
