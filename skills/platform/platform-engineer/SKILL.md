# Hermes-Skill-Platform-Engineer

> 🏗️ **平台工程与内部开发者平台专家** | IDP 构建 | 开发者体验 | 自助服务 | 平台治理

---

## 📋 技能概述

Hermes-Skill-Platform-Engineer 是一个专业的平台工程（Platform Engineering）AI 助手，专注于帮助企业构建高效的内部开发者平台（IDP - Internal Developer Platform）。涵盖平台架构设计、开发者体验（DX）优化、自助服务门户、Golden Path（黄金路径）、平台可观测性以及平台团队治理等核心领域。

### 平台工程技术栈

```
┌─────────────────────────────────────────────────────────────┐
│              Platform Engineering Stack                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🎯 Developer Portal │  ⚙️ Platform Core    │  🔧 Tooling  │
│  ├── Backstage (CNCF)│  ├── K8s Operators   │  ├── Terraform│
│  ├── Port            │  ├── Service Mesh     │  ├── Pulumi   │
│  └── Custom Portal   │  ├── GitOps (ArgoCD)  │  └── Crossplane│
│                                                             │
|  🛣️ Golden Paths      │  📊 Observability     │  🔄 Feedback  │
│  ├── App Onboarding │  ├── Platform Metrics │  ├── Surveys  │
│  ├── DB Provision.  │  ├── DevEx Scorecard  │  │   & NPS    │
│  └── Feature Flags  │  └── Cost Allocation │  └── Analytics│
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 快速开始

### 平台评估

```bash
# 开发者体验评估
/platform dx-assessment --team frontend

# 平台成熟度分析
/platform maturity-model "current-idp"

# Golden Path 设计
/platform golden-path "microservice-from-scratch"
```

### 平台构建

```bash
# 初始化 IDP 项目
/platform init-idp "hermes-developer-platform" --framework backstage

# 添加服务目录
/platform add-catalog "databases" --include postgres,redis,mongodb
```

---

## 🏗️ 内部开发者平台 (IDP) 架构

### 参考架构设计

```
┌─────────────────────────────────────────────────────────────┐
│           Internal Developer Platform (IDP)                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              Developer Portal (Frontend)               │   │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐  │   │
│  │  │Catalog  │ │Software │ │Docs &   │ │Cost     │  │   │
│  │  │(Services│ │Template │ │Knowledge│ │Explorer │  │   │
│  │  │ Search) │ │ Library)│ │ Base)   │ │         │  │   │
│  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘  │   │
│  └────────────────────────┬────────────────────────────┘   │
│                           │ API / GraphQL                   │
│  ┌────────────────────────▼────────────────────────────┐   │
│  │              Platform Backend Services                │   │
│  │                                                      │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │   │
│  │  │ Catalog Svc  │  │ Template Svc │  │ Auth/IDP  │ │   │
│  │  │ (Service Reg)│  │ (Scaffolding)│  │ (SSO/OAuth)│ │   │
│  │  └──────┬───────┘  └──────┬───────┘  └─────┬─────┘ │   │
│  │  ┌──────▼───────┐  ┌──────▼───────┐  ┌─────▼─────┐ │   │
│  │  │ Provisioning │  │ Permission   │  │ Cost Mgmt │ │   │
│  │  │ Service      │  │ Service      │  │ Service   │ │   │
│  │  └──────┬───────┘  └──────┬───────┘  └─────┬─────┘ │   │
│  └─────────┼────────────────┼────────────────┼─────────┘   │
│            │                │                │             │
│  ┌────────▼────────────────▼────────────────▼─────────┐   │
│  │              Infrastructure Abstraction Layer        │   │
│  │                                                      │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────┐  │   │
│  │  │Kubernetes│  │Cloud APIs│  │Terraform/Pulumi   │  │   │
│  │  │Operators│  │(AWS/Azure│  │Infrastructure as   │  │   │
│  │  │(CRDs)    │  │ /GCP)    │  │Code Wrappers      │  │   │
│  │  └──────────┘  └──────────┘  └──────────────────┘  │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                             │
│  Underlying Infrastructure:                                │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐     │
│  │ AWS/Azure│  │GitHub    │  │Artifactory│  │Datadog  │     │
│  │ /GCP     │  │Lab/GitHub│  │/Nexus    │  │/Grafana │     │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Backstage 插件开发示例

```typescript
// plugins/hermes-database-provisioner/src/plugin.ts

import {
  createPlugin,
  createApiFactory,
  discoveryApiRef,
  fetchApiRef,
  createRoutableExtension,
  createComponentExtension,
} from '@backstage/core-plugin-api';
import { 
  DatabaseProvisionerApi, 
  databaseProvisionerApiRef,
  DatabaseRequest,
  DatabaseInstance,
  ProvisioningStatus
} from '../api/types';

/**
 * Hermes Database Provisioner Plugin
 * 
 * 功能：
 * - 自助数据库实例创建（PostgreSQL, Redis, MongoDB）
 * - 自动配置备份、监控、访问控制
 * - 成本估算和配额管理
 * - 多环境支持（dev/staging/prod）
 */
export const hermesDatabaseProvisionerPlugin = createPlugin({
  id: 'hermes-database-provisioner',
  apis: [
    createApiFactory({
      api: databaseProvisionerApiRef,
      deps: { discoveryApi: discoveryApiRef, fetchApi: fetchApiRef },
      factory: ({ discoveryApi, fetchApi }) =>
        new DatabaseProvisionerClient({ discoveryApi, fetchApi }),
    }),
  ],
});

// ===== 前端组件 =====

import React, { useState } from 'react';
import { useApi } from '@backstage/core-plugin-api';
import { databaseProvisionerApiRef } from '../api/types';
import {
  Content,
  Header,
  Page,
  Progress,
  StatusOK,
  StatusWarning,
  Table,
  TableColumn,
  Alert,
  Select,
  Input,
  Button,
} from '@backstage/core-components';

interface DatabaseFormData {
  name: string;
  type: 'postgresql' | 'mongodb' | 'redis';
  environment: 'development' | 'staging' | 'production';
  size: 'small' | 'medium' | 'large' | 'xlarge';
  team: string;
  backup_enabled: boolean;
  retention_days: number;
}

const DATABASE_TEMPLATES = {
  postgresql: {
    small: { cpu: '0.5', memory: '1Gi', storage: '10Gi', cost_monthly: 20 },
    medium: { cpu: '2', memory: '4Gi', storage: '50Gi', cost_monthly: 80 },
    large: { cpu: '4', memory: '16Gi', storage: '200Gi', cost_monthly: 300 },
    xlarge: { cpu: '8', memory: '32Gi', storage: '500Gi', cost_monthly: 600 },
  },
  mongodb: {
    small: { cpu: '0.5', memory: '2Gi', storage: '10Gi', cost_monthly: 25 },
    medium: { cpu: '2', memory: '8Gi', storage: '50Gi', cost_monthly: 100 },
    // ... 其他规格
  },
  redis: {
    small: { cpu: '0.25', memory: '512Mi', mode: 'standalone', cost_monthly: 15 },
    medium: { cpu: '0.5', memory: '2Gi', mode: 'cluster', cost_monthly: 60 },
    // ...
  }
};

export const DatabaseProvisionerPage = () => {
  const api = useApi(databaseProvisionerApiRef);
  const [formData, setFormData] = useState<DatabaseFormData>({
    name: '',
    type: 'postgresql',
    environment: 'development',
    size: 'medium',
    team: '',
    backup_enabled: true,
    retention_days: 30,
  });
  
  const [status, setStatus] = useState<'idle' | 'submitting' | 'success' | 'error'>('idle');
  const [provisionedDb, setProvisionedDb] = useState<DatabaseInstance | null>(null);
  const [error, setError] = useState<string | null>(null);
  
  const selectedSpec = DATABASE_TEMPLATES[formData.type]?.[formData.size];
  
  const handleSubmit = async () => {
    setStatus('submitting');
    setError(null);
    
    try {
      const request: DatabaseRequest = {
        ...formData,
        requested_by: 'current-user',
        project_slug: `${formData.team}-${formData.name}`,
      };
      
      const result = await api.provisionDatabase(request);
      
      setProvisionedDb(result.instance);
      setStatus('success');
      
    } catch (err: any) {
      setError(err.message || 'Failed to provision database');
      setStatus('error');
    }
  };
  
  return (
    <Page themeId="tool">
      <Header title="🗄️ Database Provisioner" subtitle="Self-service database instance creation">
        <HeaderActionButton label="My Databases" to="/databases/my-instances" />
      </Header>
      
      <Content>
        <Grid container spacing={3} style={{ marginTop: '16px' }}>
          <Grid item xs={12} md={6}>
            <Paper elevation={2} style={{ padding: '24px' }}>
              <Typography variant="h6" gutterBottom>
                Request New Database
              </Typography>
              
              <form onSubmit={(e) => { e.preventDefault(); handleSubmit(); }}>
                <TextField
                  label="Database Name"
                  fullWidth
                  margin="normal"
                  value={formData.name}
                  onChange={e => setFormData({...formData, name: e.target.value})}
                  helperText="Lowercase, hyphens only"
                  required
                />
                
                <FormControl fullWidth margin="normal">
                  <InputLabel>Database Type</InputLabel>
                  <Select
                    value={formData.type}
                    label="Database Type"
                    onChange={e => setFormData({
                      ...formData, 
                      type: e.target.value as any
                    })}
                  >
                    <MenuItem value="postgresql">PostgreSQL</MenuItem>
                    <MenuItem value="mongodb">MongoDB</MenuItem>
                    <MenuItem value="redis">Redis</MenuItem>
                  </Select>
                </FormControl>
                
                <FormControl fullWidth margin="normal">
                  <InputLabel>Environment</InputLabel>
                  <Select
                    value={formData.environment}
                    label="Environment"
                    onChange={e => setFormData({
                      ...formData,
                      environment: e.target.value as any
                    })}
                  >
                    <MenuItem value="development">Development</MenuItem>
                    <MenuItem value="staging">Staging</MenuItem>
                    <MenuItem value="production">Production (requires approval)</MenuItem>
                  </Select>
                </FormControl>
                
                <FormControl fullWidth margin="normal">
                  <InputLabel>Size Tier</InputLabel>
                  <Select
                    value={formData.size}
                    label="Size Tier"
                    onChange={e => setFormData({
                      ...formData,
                      size: e.target.value as any
                    })}
                  >
                    <MenuItem value="small">Small ({selectedSpec?.cost_monthly}/mo)</MenuItem>
                    <MenuItem value="medium">Medium ({selectedSpec?.cost_monthly}/mo)</MenuItem>
                    <MenuItem value="large">Large ({selectedSpec?.cost_monthly}/mo)</MenuItem>
                    <MenuItem value="xlarge">X-Large ({selectedSpec?.cost_monthly}/mo)</MenuItem>
                  </Select>
                </FormControl>
                
                {/* Cost estimation card */}
                {selectedSpec && (
                  <Alert severity="info" style={{ marginTop: '16px' }}>
                    <strong>Estimated Monthly Cost:</strong> ${selectedSpec.cost_monthly}<br/>
                    <strong>Specs:</strong> {selectedSpec.cpu} CPU, {selectedSpec.memory} Memory, {selectedSpec.storage} Storage
                  </Alert>
                )}
                
                <Button
                  type="submit"
                  variant="contained"
                  color="primary"
                  fullWidth
                  disabled={status === 'submitting' || !formData.name}
                  style={{ marginTop: '24px' }}
                >
                  {status === 'submitting' ? (
                    <>
                      <CircularProgress size={20} style={{ marginRight: 8 }} />
                      Provisioning...
                    </>
                  ) : 'Provision Database'}
                </Button>
              </form>
              
              {error && (
                <Alert severity="error" style={{ marginTop: '16px' }}>
                  {error}
                </Alert>
              )}
            </Paper>
          </Grid>
          
          <Grid item xs={12} md={6}>
            {status === 'success' && provisionedDb ? (
              <Paper elevation={2} style={{ padding: '24px' }}>
                <StatusOK>Database Provisioned Successfully!</StatusOK>
                
                <Table
                  title="Instance Details"
                  options={{ search: false, paging: false }}
                  columns={[
                    { title: 'Property', field: 'property' },
                    { title: 'Value', field: 'value' },
                  ]}
                  data={[
                    { property: 'Instance Name', value: provisionedDb.name },
                    { property: 'Connection String', value: provisionedDb.connection_string },
                    { property: 'Host', value: provisionedDb.host },
                    { property: 'Port', value: provisionedDb.port.toString() },
                    { property: 'Username', value: provisionedDb.username },
                    { property: 'Password', value: '••••••••' },
                    { property: 'Dashboard', value: (
                      <Link href={provisionedDb.dashboard_url}>Open Dashboard</Link>
                    )},
                    { property: 'Backup Enabled', value: provisionedDb.backup_enabled ? 'Yes' : 'No' },
                    { property: 'Retention', value: `${provisionedDb.retention_days} days` },
                  ]}
                />
                
                <Alert severity="info" style={{ marginTop: '16px' }}>
                  Connection details are also available in your team's 1Password vault.
                  The database will be ready in approximately 2 minutes.
                </Alert>
              </Paper>
            ) : (
              <Paper elevation={2} style={{ padding: '24px' }}>
                <Typography variant="h6" gutterBottom>
                  Quick Start Guide
                </Typography>
                <Typography paragraph>
                  Follow these steps to get started with your new database:
                </Typography>
                <ol>
                  <li>Fill out the form and click "Provision Database"</li>
                  <li>Wait for provisioning to complete (~2 minutes)</li>
                  <li>Copy the connection string from the results</li>
                  <li>Add credentials to your application's secrets manager</li>
                  <li>Start building! 🚀</li>
                </ol>
                
                <Divider style={{ margin: '24px 0' }} />
                
                <Typography variant="subtitle2" gutterBottom>
                  Available Templates
                </Typography>
                <List dense>
                  <ListItem><ListItemText primary="PostgreSQL 15 - General purpose relational DB" /></ListItem>
                  <ListItem><ListItemText primary="MongoDB 6.0 - Document store for flexible schemas" /></ListItem>
                  <ListItem><ListItemText primary="Redis 7.0 - In-memory cache and message broker" /></ListItem>
                </List>
              </Paper>
            )}
          </Grid>
        </Grid>
      </Content>
    </Page>
  );
};
```

---

## 🛣️ Golden Path 实现

### 微服务创建黄金路径

```yaml
# golden-path-microservice.yaml - 微服务创建标准路径

golden_path:
  name: microservice-starter
  version: "3.0"
  description: "Standardized path for creating production-ready microservices"
  
  phases:
    
    - phase: discovery
      name: "Define Requirements"
      steps:
        - step: service_blueprint
          component: template-form
          required_fields:
            - name: service_name
              validation: regex("^[a-z][a-z0-9-]{2,30}$")
              help_text: "Lowercase, alphanumeric with hyphens, 3-31 chars"
            
            - name: owner_team
              type: select
              source: catalog:/teams
              help_text: "The team that will own this service"
            
            - name: criticality
              type: select
              options:
                - value: tier_1
                  label: "Tier 1 - Business Critical (99.99% SLA)"
                - value: tier_2
                  label: "Tier 2 - Important (99.9% SLA)"
                - value: tier_3
                  label: "Tier 3 - Standard (99% SLA)"
            
            - name: data_classification
              type: select
              options: [public, internal, confidential, restricted]
            
            - name: estimated_traffic_rps
              type: number
              default: 100
              help_text: "Expected peak requests per second"
          
          approval_rules:
            - if: criticality == "tier_1"
              then: require_approval_from: ["platform-team", "architecture-board"]
            - if: data_classification == "restricted"
              then: require_approval_from: ["security-team", "dpo"]
    
    - phase: scaffold
      name: "Generate Codebase"
      automation: true
      actions:
        - action: generate_project_skeleton
          template: microservice-template-python-v2
          output: repo:github.com/hermes/{service_name}
          includes:
            - src/
            - tests/
            - Dockerfile
            - docker-compose.yml
            - .github/workflows/ci.yml
            - .github/workflows/cd.yml
            - README.md
            - API.md
            - Makefile
        
        - action: initialize_git
          branch: main
          initial_commit: true
          protection_rules:
            main_branch:
              require_pr: true
              require_status_checks: [ci, security-scan, license-check]
              dismiss_stale_reviews: true
              required_reviewers: 1
    
    - phase: infrastructure
      name: "Provision Infrastructure"
      automation: true
      actions:
        - action: create_kubernetes_namespace
          name: {service_name}
          labels:
            team: {owner_team}
            environment: development
            managed-by: platform-team
          resource_quotas:
            requests.cpu: "4"
            requests.memory: "8Gi"
            limits.cpu: "10"
            limits.memory: "16Gi"
            pods: "50"
        
        - action: create_database_if_needed
          condition: requires_database == true
          auto_provision_from: database-provisioner-plugin
          type: postgresql
          size: medium
          backup_enabled: true
        
        - action: create_cache_if_needed
          condition: requires_cache == true
          type: redis
          mode: cluster
          size: small
        
        - action: setup_observability
          includes:
            - datadog_apm_tracing
            - prometheus_metrics_endpoint (/metrics)
            - structured_logging (JSON format)
            - health_check_endpoint (/healthz)
            - readiness_check (/readyz)
        
        - action: configure_networking
          ingress:
            class: nginx-internal
            tls: letsencrypt-production
            rate_limiting:
              rps: {estimated_traffic_rps * 2}  # 2x headroom
            
        - action: setup_secrets
          vault_path: secret/data/services/{service_name}/{environment}
          auto_rotate: true
          rotation_period: 90 days
    
    - phase: ci_cd_setup
      name: "Configure CI/CD Pipeline"
      automation: true
      pipeline_config:
        stages:
          - name: lint_and_format
            tools: [ruff, black, mypy]
            fail_on: error
          
          - name: unit_tests
            framework: pytest
            coverage_threshold: 70
            parallel: true
          
          - name: integration_tests
            framework: pytest-integration
            services: [postgres-test, redis-test]
            timeout_minutes: 15
          
          - name: security_scan
            tools: [semgrep, dependency-check, trivy-fs]
            block_on: critical + high
          
          - name: build_image
            registry: harbor.hermes.internal/{owner_team}
            multi_arch: [linux/amd64, linux/arm64]
            sbom_generation: true
            signing: cosign
          
          - name: deploy_to_dev
            trigger: push to main
            strategy: rolling_update
            health_check_grace_period: 30s
            auto_rollback: true
            
          - name: deploy_to_staging
            trigger: manual or tag v*.*.*-rc.*
            approval_required: 1 reviewer
            canary_deployment:
              initial_weight: 5%
              increment: 5%
              interval: 60s
            
          - name: deploy_to_production
            trigger: tag v*.*.*
            approval_required: 2 reviewers (one must be from platform-team)
            deployment_window: business-hours-only
            post_deploy_verification:
              error_rate_threshold: 1%
              p95_latency_threshold: 500ms
              duration: 15 minutes
    
    - phase: documentation
      name: "Create Documentation"
      automation: partial
      generates:
        - ADR (Architecture Decision Records)
        - RUNBOOK (incident response procedures)
        - API Documentation (OpenAPI 3.0)
        - ONCALL_GUIDE (for operations team)
        - COST_MODEL (resource allocation and billing)
    
    - phase: onboarding
      name: "Team Handoff"
      automation: notification
      notifies:
        - channel: slack#{owner_team}-announcements
          message: |
            🆕 **New Service Ready**: {service_name}
            📦 Repository: https://github.com/hermes/{service_name}
            📖 Docs: https://docs.hermes.internal/services/{service_name}
            📊 Dashboard: https://grafana.hermes.internal/d/{dashboard-id}
            👤 Owner: {owner_team}
        
        - channel: email:{team-leads}
          subject: "New Microservice Requires Review: {service_name}"
          body: "Please review the architecture and provide feedback within 5 business days."

  success_criteria:
    - code_generated: true
    - infrastructure_provisioned: true
    - ci_cd_configured: true
    - documentation_created: true
    - team_notified: true
  
  estimated_time_to_production: "2-3 days (with automation)"
```

---

## 📊 平台可观测性与指标

### 开发者体验评分卡

```python
# platform_metrics.py - 平台健康度与 DX 指标

from dataclasses import dataclass
from typing import Dict, List, Optional
from datetime import datetime, timedelta
from enum import Enum

class MetricCategory(Enum):
    VELOCITY = "velocity"
    QUALITY = "quality"
    EXPERIENCE = "experience"
    RELIABILITY = "reliability"
    SECURITY = "security"

@dataclass
class PlatformMetric:
    name: str
    category: MetricCategory
    current_value: float
    target_value: float
    unit: str
    trend: str  # improving, stable, declining
    weight: float = 1.0  # 用于加权综合评分
    
    @property
    def achievement_rate(self) -> float:
        if self.target_value == 0:
            return 1.0
        # 对于"越低越好"的指标，需要特殊处理
        return min(self.current_value / self.target_value, 2.0)

@dataclass
class DeveloperExperienceScorecard:
    """开发者体验评分卡"""
    period_start: datetime
    period_end: datetime
    overall_score: float  # 0-100
    grade: str  # A+, A, B, C, D, F
    category_scores: Dict[MetricCategory, float]
    metrics: List[PlatformMetric]
    recommendations: List[str]

class PlatformObservability:
    """
    平台可观测性系统
    
    收集和分析以下维度：
    1. 平台采用率 (Adoption Rate)
    2. 开发者生产力 (Developer Productivity)
    3. 平台可靠性 (Platform Reliability)
    4. 开发者满意度 (Developer Satisfaction/NPS)
    """
    
    def calculate_dx_scorecard(
        self,
        organization_id: str,
        period_days: int = 30
    ) -> DeveloperExperienceScorecard:
        """
        计算开发者体验评分卡
        """
        
        end_date = datetime.utcnow()
        start_date = end_date - timedelta(days=period_days)
        
        metrics = []
        
        # === Velocity Metrics (速度) ===
        metrics.extend([
            PlatformMetric(
                name="Time to First Hello World",
                category=MetricCategory.VELOCITY,
                current_value=self._get_avg_time_to_hello_world(start_date, end_date),
                target_value=30.0,  # 30 minutes
                unit="minutes",
                trend=self._get_trend("time_to_hello_world"),
                weight=0.15
            ),
            PlatformMetric(
                name="Time to Production",
                category=MetricCategory.VELOCITY,
                current_value=self._get_avg_time_to_prod(start_date, end_date),
                target_value=3.0,  # 3 days
                unit="days",
                trend=self._get_trend("time_to_production"),
                weight=0.20
            ),
            PlatformMetric(
                name="Deployment Frequency",
                category=MetricCategory.VELOCITY,
                current_value=self._get_avg_deploys_per_dev_per_week(start_date, end_date),
                target_value=2.0,  # 2 deploys per developer per week
                unit="deploys/dev-week",
                trend=self._get_trend("deploy_frequency"),
                weight=0.10
            ),
        ])
        
        # === Quality Metrics (质量) ===
        metrics.extend([
            PlatformMetric(
                name="Change Failure Rate",
                category=MetricCategory.QUALITY,
                current_value=self._get_change_failure_rate(start_date, end_date),
                target_value=0.05,  # < 5%
                unit="percentage",
                trend=self._get_trend("change_failure_rate"),
                weight=0.15
            ),
            PlatformMetric(
                name="Mean Time to Recovery (MTTR)",
                category=MetricCategory.RELIABILITY,
                current_value=self._get_mttr_hours(start_date, end_date),
                target_value=1.0,  # < 1 hour
                unit="hours",
                trend=self._get_trend("mttr"),
                weight=0.10
            ),
        ])
        
        # === Experience Metrics (体验) ===
        metrics.extend([
            PlatformMetric(
                name="Portal Daily Active Users (DAU)",
                category=MetricCategory.EXPERIENCE,
                current_value=self._get_portal_dau(),
                target_value=self._get_total_active_developers() * 0.8,
                unit="users",
                trend=self._get_trend("portal_dau"),
                weight=0.10
            ),
            PlatformMetric(
                name="Self-Service Rate",
                category=MetricCategory.EXPERIENCE,
                current_value=self._get_self_service_rate(start_date, end_date),
                target_value=0.85,  # 85% of requests handled without human intervention
                unit="ratio",
                trend=self._get_trend("self_service_rate"),
                weight=0.10
            ),
            PlatformMetric(
                name="Developer NPS Score",
                category=MetricCategory.EXPERIENCE,
                current_value=self._get_latest_nps_score(),
                target_value=50.0,  # Good NPS (>0 is good, >50 is excellent)
                unit="score",
                trend=self._get_trend("nps"),
                weight=0.10
            ),
        ])
        
        # 计算分类得分和总分
        category_scores = {}
        for category in MetricCategory:
            cat_metrics = [m for m in metrics if m.category == category]
            if cat_metrics:
                weighted_sum = sum(m.achievement_rate * m.weight for m in cat_metrics)
                total_weight = sum(m.weight for m in cat_metrics)
                category_scores[category] = (weighted_sum / total_weight) * 100 if total_weight > 0 else 0
        
        overall = sum(category_scores.values()) / len(category_scores) if category_scores else 0
        
        grade = self._score_to_grade(overall)
        
        recommendations = self._generate_recommendations(metrics, category_scores)
        
        return DeveloperExperienceScorecard(
            period_start=start_date,
            period_end=end_date,
            overall_score=round(overall, 1),
            grade=grade,
            category_scores={k: round(v, 1) for k, v in category_scores.items()},
            metrics=metrics,
            recommendations=recommendations
        )
    
    def _score_to_grade(self, score: float) -> str:
        if score >= 90: return 'A+'
        elif score >= 80: return 'A'
        elif score >= 70: return 'B'
        elif score >= 60: return 'C'
        elif score >= 40: return 'D'
        else: return 'F'
    
    def _generate_recommendations(self, metrics, scores):
        """基于指标数据生成改进建议"""
        recommendations = []
        
        for metric in metrics:
            if metric.achievement_rate < 0.7:  # 未达到目标 70%
                if metric.category == MetricCategory.VELOCITY:
                    recommendations.append(
                        f"⚡ Improve '{metric.name}' (currently {metric.current_value}{metric.unit}, "
                        f"target: {metric.target_value}{metric.unit}): "
                        f"Consider adding more templates or simplifying the onboarding process."
                    )
                elif metric.category == MetricCategory.EXPERIENCE:
                    recommendations.append(
                        f"😊 Enhance developer experience for '{metric.name}': "
                        f"Gather user feedback via surveys and focus groups."
                    )
                elif metric.category == MetricCategory.QUALITY:
                    recommendations.append(
                        f"🔧 Address quality issue '{metric.name}': "
                        f"Review CI/CD pipeline gates and add more automated testing."
                    )
        
        if scores.get(MetricCategory.EXPERIENCE, 0) < 60:
            recommendations.append(
                "📢 Overall developer experience is below target. "
                "Consider conducting a comprehensive DX survey."
            )
        
        return recommendations[:5]  # 返回 Top 5 建议


# ===== 使用示例 =====

if __name__ == "__main__":
    obs = PlatformObservability()
    
    scorecard = obs.calculate_dx_scorecard("hermes-tech", period_days=30)
    
    print("=" * 60)
    print(f"📊 Developer Experience Scorecard")
    print(f"Period: {scorecard.period_start.strftime('%Y-%m-%d')} to {scorecard.period_end.strftime('%Y-%m-%d')}")
    print("=" * 60)
    print(f"\nOverall Score: {scorecard.overall_score}/100 (Grade: {scorecard.grade})\n")
    
    print("Category Breakdown:")
    for cat, score in scorecard.category_scores.items():
        bar = '█' * int(score / 5)
        print(f"  {cat.value}: {score:5.1f} {bar}")
    
    print("\nTop Recommendations:")
    for i, rec in enumerate(scorecard.recommendations, 1):
        print(f"  {i}. {rec}")
```

---

## 🔗 相关技能

- [Hermes-Skill-DevRel-Manager](../community/devrel-manager/SKILL.md) - 开发者关系与反馈收集
- [Hermes-Skill-CI-CD-Pipeline-Manager](../devops/cicd-pipeline-manager/SKILL.md) - CI/CD 管道优化
- [Hermes-Skill-Cloud-Architect](../cloud/cloud-architect/SKILL.md) - 底层基础设施抽象

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| IDP 框架 | Backstage, Port, Custom |
| 平台模式 | Self-Service, Golden Paths, Platform Teams |
| 可观测性 | DX Scorecard, DORA Metrics, SPACE Framework |
| 治理框架 | 安全、合规、成本、质量全覆盖 |
| 代码示例 | TypeScript, Python, YAML, Kubernetes |
| 文档完整度 | ★★★★★ |

---

## ⭐ 支持本项目

如果这个技能对你有帮助，请考虑支持我们的持续开发：

💰 **微信支付**：查看 `images/wechat-pay.png`  
💰 **支付宝**：查看 `images/alipay.png`  
⭐ **GitHub Star**：给个 Star 让更多人发现这个项目！

---

**技能版本**: v1.0.0  
**最后更新**: 2026-04-28  
**适用场景**: 内部开发者平台 | 平台工程 | DevEx 优化 | 自助服务 | Golden Paths
