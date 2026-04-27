---
name: cicd-pipeline-manager
description: "Hermes-Skill-CICD-Pipeline-Manager - CI/CD流水线管理与自动化专家，支持GitHub Actions/GitLab CI/Jenkins流水线创建、优化、故障排查和DevOps最佳实践。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [cicd, devops, github-actions, gitlab-ci, jenkins, docker, kubernetes, deployment, automation]
    related_skills: [api-testing-suite, code-review-pro, github-integration, security-auditor]
    requires_toolsets: [web, terminal]
    config:
      - key: ci_platform
        default: github-actions
      - key: deployment_target
        default: docker
      - key: quality_gates
        default: lint,test,build,security-scan
---

# Hermes-Skill-CICD-Pipeline-Manager (CI/CD 流水线管理器)

## 概述

**Hermes-Skill-CICD-Pipeline-Manager** 是一个全面的 CI/CD（持续集成/持续部署）流水线管理和优化工具。它能帮助 AI Agent 设计、创建、调试和维护高效的自动化交付流水线，覆盖从代码提交到生产部署的全流程。

### 核心价值

- **多平台支持**: GitHub Actions / GitLab CI / Jenkins / CircleCI
- **流水线即代码**: 可版本控制、可审查、可复用的 Pipeline 定义
- **质量门禁**: Lint → Test → Build → Security Scan → Deploy
- **快速反馈**: 将构建时间从小时压缩到分钟
- **安全左移**: 在 CI 阶段集成安全扫描
- **可观测性**: 构建日志、测试覆盖率、部署状态全链路追踪

---

## 支持的平台

| 平台 | 配置文件 | 特点 |
|------|---------|------|
| **GitHub Actions** | `.github/workflows/*.yml` | 免费额度大、Marketplace 生态丰富 |
| **GitLab CI** | `.gitlab-ci.yml` | 内置 Docker/Cache/Kubernetes 支持 |
| **Jenkins** | `Jenkinsfile` | 高度可定制、插件生态成熟 |
| **CircleCI** | `.circleci/config.yml` | 并行构建性能优秀 |
| **Azure DevOps** | `azure-pipelines.yml` | 微软生态深度集成 |

---

## 快速开始

```bash
# 初始化 CI/CD 配置
/cicd init --platform github-actions --language python --framework fastapi

# 生成完整流水线
/cicd generate --stages lint,test,build,security,deploy --target docker

# 本地调试流水线
/cicd debug --workflow ci.yml --step test

# 分析流水线性能
/cicd analyze --repo ./my-project --suggest optimizations

# 安全审计
/cicd security-audit --check secrets,dependencies,container

# 部署预览环境
/cicd deploy-preview --branch feature/new-ui --env staging
```

---

## 流水线架构

### 标准流水线阶段

```
Code Commit
    ↓
┌─────────────────────────────────────────────────────┐
│                   CI (持续集成)                      │
│                                                       │
│  Stage 1: Pre-check                                   │
│  ├─ Format Check (prettier/black/ruff)                │
│  ├─ Lint (eslint/pylint/flake8)                       │
│  └─ Type Check (mypy/pyright/typescript)              │
│                                                       │
│  Stage 2: Test                                        │
│  ├─ Unit Tests (pytest/jest/go test)                  │
│  ├─ Integration Tests                                 │
│  ├─ Coverage Report (>80% threshold)                  │
│  └─ Test Results Upload                               │
│                                                       │
│  Stage 3: Build                                       │
│  ├─ Compile                                           │
│  ├─ Bundle Assets                                     │
│  └─ Docker Image Build & Push                         │
│                                                       │
│  Stage 4: Quality Gates                                │
│  ├─ SAST (Static Analysis)                            │
│  ├─ Dependency Audit (Snyk/Dependabot)                 │
│  ├─ Secret Scanning (gitleaks/trufflehogger)          │
│  └─ Container Security (Trivy/Grype)                   │
│                                                       │
└─────────────────────────────────────────────────────┘
    ↓ All Quality Gates Pass ✅
┌─────────────────────────────────────────────────────┐
│                  CD (持续部署)                        │
│                                                       │
│  Stage 5: Deploy                                      │
│  ├─ Staging Environment (自动)                        │
│  │   ├─ Deploy to staging                             │
│  │   ├─ Run E2E Tests                                │
│  │   └─ Performance Baseline                          │
│  │                                                    │
│  ├─ Production (需审批)                               │
│  │   ├─ Blue-Green / Canary Deployment               │
│  │   ├─ Health Checks                                │
│  │   └─ Rollback if Failed                           │
│  │                                                    │
│  └─ Post-Deploy                                      │
│      ├─ Notify Team (Slack/Discord/Email)             │
│      ├─ Update Status Badge                           │
│      └─ Generate Release Notes                        │
│                                                       │
└─────────────────────────────────────────────────────┘
```

---

## GitHub Actions 完整示例

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  workflow_dispatch:  # 手动触发

env:
  NODE_VERSION: '20'
  PYTHON_VERSION: '3.11'
  DOCKER_REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  # ===== Stage 1: Code Quality =====
  lint:
    name: 🔍 Code Quality Checks
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: ESLint
        run: npm run lint
        
      - name: Format Check
        run: npx prettier --check .
        
      - name: Type Check
        if: matrix.language == 'typescript'
        run: npm run typecheck

  # ===== Stage 2: Testing =====
  test:
    name: 🧪 Test Suite
    runs-on: ubuntu-latest
    needs: lint
    strategy:
      fail-fast: false
      matrix:
        node-version: [18, 20, 22]
        
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          
      - name: Cache dependencies
        uses: actions/cache@v4
        with:
          path: ~/.npm
          key: npm-${{ hashFiles('package-lock.json') }}
          
      - name: Install & Test
        run: |
          npm ci
          npm run test:coverage
          
      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          
      - name: Upload test results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-results-${{ matrix.node-version }}
          path: coverage/

  # ===== Stage 3: Security =====
  security:
    name: 🔒 Security Scanning
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
      
      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          scan-ref: '.'
          exit-code: '0'  # 不阻断，仅报告
          severity: 'CRITICAL,HIGH'
          
      - name: Check for secrets
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./
          base: main
          head: HEAD

  # ===== Stage 4: Build =====
  build:
    name: 🏗️ Build & Push Image
    runs-on: ubuntu-latest
    needs: [test, security]
    if: github.ref == 'refs/heads/main'
    
    outputs:
      image_tag: ${{ steps.meta.outputs.tags }}
      image_digest: ${{ steps.build.outputs.digest }}
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
        
      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ${{ env.DOCKER_REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
          
      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.DOCKER_REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=sha,prefix=
            type=ref,event=branch
            type=semver,pattern={{version}}
            
      - name: Build and push
        id: build
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
          platforms: linux/amd64,linux/arm64

  # ===== Stage 5: Deploy =====
  deploy-staging:
    name: 🚀 Deploy to Staging
    runs-on: ubuntu-latest
    needs: build
    environment: staging
    
    steps:
      - name: Deploy to Kubernetes
        run: |
          kubectl set image deployment/app \
            app=${{ needs.build.outputs.image_tag }} \
            -n staging
            
      - name: Health check
        run: |
          kubectl rollout status deployment/app -n staging --timeout=120s
          kubectl get pods -n staging -l app=app

  deploy-production:
    name: 🏭 Deploy to Production
    runs-on: ubuntu-latest
    needs: build
    environment: production
    if: github.event_name == 'workflow_dispatch'  # 需手动触发
    
    steps:
      - name: Canary Deployment (20% traffic)
        run: ./scripts/deploy-canary.sh ${{ needs.build.outputs.image_tag }}
        
      - name: Wait for validation
        run: sleep 300  # 5分钟观察期
        
      - name: Promote to 100%
        if: success()
        run: ./scripts/promote-full.sh
        
      - name: Rollback on failure
        if: failure()
        run: kubectl rollout undo deployment/app -n production
```

---

## 性能优化策略

### 构建加速

```
优化手段:
├── 依赖缓存 (Caching)
│   ├── npm/pip/maven gradle cache
│   ├── Docker layer caching
│   └── GitHub Actions Cache (gha)
│
├── 并行执行 (Parallelism)
│   ├── Matrix Strategy (多版本并行测试)
│   ├── Job 级别并行 (独立 stage 同时跑)
│   └── Step 内并行 (多个脚本同时执行)
│
├── 增量构建 (Incremental)
│   ├── TurboRepo / Nx affected
│   ├── 只跑变更文件的测试
│   └── 条件跳过 (paths-ignore/paths)
│
└── 远程构建 (Remote)
    ├── GitHub Actions Larger Runners (更多 CPU/RAM)
    └── Self-hosted runners (私有网络/预装依赖)
```

### 典型优化效果

| 优化前 | 优化后 | 提升 |
|--------|--------|------|
| 全量测试 ~15min | 增量测试 ~3min | **5x** |
| 无缓存构建 ~8min | 缓存构建 ~2min | **4x** |  
| 串行 Job ~25min | 并行 Job ~7min | **3.5x** |
| 单平台构建 | 多平台并行 | **2x** |

---

## 故障排除

| 问题 | 诊断命令 | 常见原因 |
|------|---------|---------|
| 测试在 CI 中失败但本地通过 | `/cicd debug --step test` | 环境差异 / 时区 / 依赖版本 |
| Docker 构建超时 | 检查 `.dockerignore` | 上下文过大 / 网络慢 |
| 缓存未命中 | 查看 cache hit/miss logs | lockfile 变更 / cache key 不匹配 |
| Secrets 泄露告警 | `/cicd security-audit` | 误提交了凭证文件 |
| 部署后 502 | `kubectl logs -f` + `kubectl describe pod` | 健康检查配置错误 |

---

## 最佳实践

### ✅ CI/CD 黄金法则

1. **快速反馈**: PR 的 CI 结果 < 10 分钟
2. **确定性**: 相同代码 → 相同结果（固定依赖版本）
3. **安全第一**: 绝不在日志中打印 secrets
4. **渐进式部署**: Staging → Canary → Production
5. **一切可回滚**: 每次部署都要有回滚方案

### ❌ 反模式

- **巨型 Workflow**: 单个 yml > 500 行 → 拆分为可复用 Composite Actions
- **硬编码环境**: 所有配置走 secrets/variables
- **忽略失败**: `continue-on-error: true` 滥用
- **缺少超时**: 每个 step 设置 timeout-minutes
- **手动触发过多**: 尽量自动化，减少人工干预

---

*创新技能 - 现代 DevOps 工程师必备的 CI/CD 管理助手*
*版本: 2.0.0 | 最后更新: 2026-04-27*
