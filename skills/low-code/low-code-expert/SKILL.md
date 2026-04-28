# Hermes-Skill-Low-Code-Expert

> 🚀 **低代码/无代码平台专家** | 企业应用快速开发 | 平台架构 | 扩展与集成 | 治理策略

---

## 📋 技能概述

Hermes-Skill-Low-Code-Expert 是一个专业的低代码/无代码（LCAP/NCDP）平台开发与治理 AI 助手，专注于帮助企业高效构建企业级应用、设计可扩展的低代码架构、实现与传统系统的深度集成，以及建立完善的低代码治理体系。涵盖 Mendix, OutSystems, Power Apps, Retool, Airtable 等主流平台。

### 低代码技术栈

```
┌─────────────────────────────────────────────────────────────┐
│                Low-Code/No-Code Stack                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
|  🏢 Enterprise LCAP   │  🔧 Internal Tools    │  📊 No-Code │
│  ├── Mendix          │  ├── Retool           │  ├── Airtable│
│  ├── OutSystems      │  ├── Appsmith         │  ├── Notion   │
│  ├── Power Platform │  ├── ToolJet          │  ├── Zapier   │
│  └── Salesforce LWC │  └── Internal Builder │  └── Make     │
│                                                             │
|  🔌 Integration       │  ⚙️ Governance         │  🎨 UX/UI    │
|  ├── API Connectors  │  ├── Security Policies│  ├── Design  │
|  ├── Webhooks        │  ├── Code Review     │  │   System  │
|  ├── RPA             │  ├── Version Control │  └── Comps.  │
│  └── Event Bus       │  └── Performance     │    Library  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 平台选型

```bash
# 低代码平台评估
/lowcode evaluate "customer-portal" --users 500 --complexity medium

# 架构设计建议
/lowcode architecture "inventory-management-system"

# 迁移规划
/lowcode migrate-from "legacy-excel-processes"
```

### 应用开发

```bash
# 快速生成 CRUD 应用
/lowcode scaffold "employee-directory" --platform mendix

# 集成配置
/lowcode integrate-sap "erp-data-sync"
```

---

## 🏗️ 低代码架构模式

### 企业低代码参考架构

```
┌─────────────────────────────────────────────────────────────┐
│            Enterprise Low-Code Reference Architecture        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Layer 4: Consumption (消费层)                             │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐         │
│  │ Web App │ │ Mobile  │ │ Portal  │ │ Chatbot │         │
│  │ (PWA)   │ │ (React) │ │ (SSO)   │ │ (AI)    │         │
│  └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘         │
│       └────────────┼────────────┼────────────┘              │
│                    ▼                                        │
│  Layer 3: Integration & Orchestration (集成层)            │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  API Gateway │ ESB/BPM │ Event Mesh │ Data Fabric   │   │
│  └────────────────────┬────────────────────────────────┘   │
│                       │                                     │
│  Layer 2: Low-Code Platforms (开发层)                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                │
│  │ Mendix   │  │PowerApps│  │ Retool   │                │
│  │(Core ERP)│  │(Dept.   │  │(Internal│                │
│  │          │  │ Tools)   │  │ Tools)   │                │
│  └─────┬────┘  └─────┬────┘  └─────┬────┘                │
│        │              │             │                     │
│  Layer 1: Data & Services (数据层)                        │
│  ┌──────┴──────┐ ┌────┴────┐ ┌──────┴──────┐            │
│  │ PostgreSQL │  │ SAP/ERP │  │ S3/Azure Blob│            │
│  │ MongoDB    │  │ Salesforce│  │ REST APIs    │            │
│  └────────────┘ └─────────┘ └──────────────┘            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Mendix 微服务架构示例

```yaml
# mendix-microservices.yaml - Mendix 微服务拆分策略

project: hermes-enterprise-suite

modules:
  # === 核心域服务 ===
  
  - name: core-user-management
    type: domain-service
    platform: mendix
    responsibility:
      - User authentication (OAuth2/SAML)
      - Role-based access control (RBAC)
      - User profile management
      - Session management
    api_exposure: public
    data_stores:
      - postgresql://user-db.internal
    integration_points:
      - core-audit-log
      - core-notification
    
  - name: core-audit-log
    type: cross-cutting-service
    platform: mendix
    responsibility:
      - Immutable audit trail
      - Compliance logging (GDPR/SOX)
      - Change tracking
    retention_policy: 7 years
    api_exposure: internal-only
    
  # === 业务域服务 ===
  
  - name: inventory-management
    type: business-service
    platform: mendix
    bounded_context: inventory
    responsibility:
      - Product catalog (100K+ SKUs)
      - Stock level management
      - Warehouse locations
      - Inventory movements
    performance_requirements:
      read_ops_per_second: 500
      write_ops_per_second: 50
      p99_latency_ms: <200
    data_model:
      entities:
        - Product (attributes: sku, name, category, price, stock_count)
        - Warehouse (attributes: location_id, capacity, type)
        - StockTransaction (attributes: product_ref, quantity, type, timestamp)
    
  - name: order-processing
    type: business-service
    platform: mendix
    bounded_context: orders
    responsibility:
      - Order lifecycle management
      - Pricing engine integration
      - Payment processing orchestration
      - Order fulfillment workflow
    workflows:
      - name: order-to-cash
        steps:
          - Order Creation → Inventory Check → Payment → Fulfillment → Shipping → Completion
        sla_targets:
          standard_order: 4 hours
          express_order: 2 hours
        escalation_rules:
          - if: stuck > 24 hours
            then: escalate-to-manager
            
  # === 集成层 ===
  
  - name: sap-integration-layer
    type: integration-service
    platform: mendix + custom-java
    responsibility:
      - SAP RFC/BAPI calls
      - IDoc processing
      - SAP OData services proxy
      - Data synchronization (bidirectional)
    connection_config:
      sap_system: PROD
      rfc_destination: HERMES_INT
      max_connections: 10
      timeout_seconds: 30
      
  - name: external-api-gateway
    type: api-gateway
    platform: kong/apigee (external) + mendix (internal)
    responsibility:
      - Rate limiting per client
      - API key management
      - Request/response transformation
      - Circuit breaker pattern
    routes:
      - path: /api/v1/inventory/*
        target: inventory-management
        auth: oauth2
        rate_limit: 1000/min
      - path: /api/v1/orders/*
        target: order-processing
        auth: oauth2 + mfa-for-high-value
        rate_limit: 500/min

governance:
  code_review_required: true
  security_scan: sast + sca
  deployment_approval: production requires 2 approvers
  documentation: required for all public APIs
  
monitoring:
  apm: Dynatrace/New Relic
  logging: centralized ELK stack
  metrics: Prometheus + Grafana dashboards
  alerting: PagerDuty for P0/P1 issues
```

---

## 🔌 低代码集成模式

### 复杂系统集成框架

```python
# lowcode_integrator.py - 低代码平台外部系统集成引擎

from typing import Dict, List, Any, Optional, Callable
from dataclasses import dataclass, field
from enum import Enum
import asyncio
import json
from datetime import datetime
import hashlib

class IntegrationType(Enum):
    REST_API = "rest_api"
    SOAP = "soap"
    DATABASE = "database"
    FILE = "file_sftp"
    EVENT = "event_bus"
    RPA = "rpa_automation"
    WEBHOOK = "webhook"

@dataclass
class IntegrationEndpoint:
    """集成端点定义"""
    id: str
    name: string
    type: IntegrationType
    base_url: Optional[str] = None
    credentials_ref: str = ""  # 引用秘密管理器中的凭证
    rate_limit_rpm: int = 60
    timeout_seconds: int = 30
    retry_config: Dict = field(default_factory=lambda: {
        'max_retries': 3,
        'backoff_factor': 2,
        'retryable_errors': [429, 500, 502, 503, 504]
    })
    transformation_rules: List[Dict] = field(default_factory=list)

@dataclass
class DataMapping:
    """数据映射规则"""
    source_field: str
    target_field: str
    transformation: Optional[str] = None  # "uppercase", "date_format", "lookup", etc.
    default_value: Any = None
    validation_rule: Optional[str] = None

class LowCodeIntegrator:
    """
    低代码集成编排器
    
    功能：
    - 多系统数据同步
    - API 聚合与代理
    - 数据转换管道
    - 错误处理与重试
    - 审计日志
    """
    
    def __init__(self, config: dict):
        self.config = config
        self.endpoints: Dict[str, IntegrationEndpoint] = {}
        self.mappings: Dict[str, List[DataMapping]] = {}
        self.audit_log: List[Dict] = []
        
    def register_endpoint(self, endpoint: IntegrationEndpoint):
        """注册集成端点"""
        self.endpoints[endpoint.id] = endpoint
        
    def define_mapping(
        self,
        mapping_name: str,
        source_system: str,
        target_system: str,
        field_mappings: List[DataMapping]
    ):
        """定义数据映射关系"""
        self.mappings[mapping_name] = {
            'source': source_system,
            'target': target_system,
            'fields': field_mappings
        }
    
    async def sync_data(
        self,
        mapping_name: str,
        source_data: List[Dict],
        options: dict = None
    ) -> Dict:
        """
        执行数据同步
        
        Args:
            mapping_name: 映射名称
            source_data: 源数据列表
            options: 同步选项 (upsert_mode, batch_size, etc.)
            
        Returns:
            同步结果统计
        """
        mapping = self.mappings.get(mapping_name)
        if not mapping:
            raise ValueError(f"Mapping '{mapping_name}' not found")
        
        stats = {
            'total_source_records': len(source_data),
            'transformed': 0,
            'sent_to_target': 0,
            'errors': [],
            'started_at': datetime.utcnow().isoformat()
        }
        
        batch_size = (options or {}).get('batch_size', 100)
        
        for i in range(0, len(source_data), batch_size):
            batch = source_data[i:i + batch_size]
            
            try:
                # 1. 数据转换
                transformed_batch = await self._transform_data(batch, mapping['fields'])
                stats['transformed'] += len(transformed_batch)
                
                # 2. 发送到目标系统
                target_endpoint = self.endpoints.get(mapping['target'])
                if target_endpoint:
                    result = await self._send_to_target(
                        target_endpoint, 
                        transformed_batch,
                        upsert_mode=(options or {}).get('upsert_mode', 'match_on_key')
                    )
                    stats['sent_to_target'] += result.get('success_count', 0)
                    
                    if result.get('errors'):
                        stats['errors'].extend(result['errors'])
                        
            except Exception as e:
                error_entry = {
                    'batch_index': i // batch_size,
                    'error': str(e),
                    'timestamp': datetime.utcnow().isoformat()
                }
                stats['errors'].append(error_entry)
        
        stats['completed_at'] = datetime.utcnow().isoformat()
        stats['success_rate'] = (
            stats['sent_to_target'] / max(stats['total_source_records'], 1) * 100
        )
        
        # 记录审计日志
        self._log_audit('data_sync', mapping_name, stats)
        
        return stats
    
    async def _transform_data(
        self, 
        records: List[Dict], 
        mappings: List[DataMapping]
    ) -> List[Dict]:
        """数据转换管道"""
        transformed = []
        
        for record in records:
            new_record = {}
            
            for mapping in mappings:
                source_value = record.get(mapping.source_field)
                
                # 应用默认值（如果源字段缺失）
                if source_value is None and mapping.default_value is not None:
                    source_value = mapping.default_value
                
                # 应用转换函数
                if mapping.transformation and source_value is not None:
                    source_value = await self._apply_transformation(
                        mapping.transformation,
                        source_value,
                        record
                    )
                
                # 验证
                if mapping.validation_rule:
                    is_valid = await self._validate(
                        mapping.validation_rule,
                        source_value
                    )
                    if not is_valid:
                        continue  # 跳过无效记录
                
                new_record[mapping.target_field] = source_value
            
            # 计算记录哈希（用于去重和变更检测）
            new_record['_hash'] = hashlib.md5(
                json.dumps(new_record, sort_keys=True).encode()
            ).hexdigest()[:12]
            
            transformed.append(new_record)
        
        return transformed
    
    async def _apply_transformation(
        self, 
        transform_type: str, 
        value: Any, 
        context: Dict
    ) -> Any:
        """数据转换函数"""
        
        transformations = {
            'uppercase': lambda v: str(v).upper() if v else v,
            'lowercase': lambda v: str(v).lower() if v else v,
            'trim': lambda v: str(v).strip() if v else v,
            'date_format': lambda v: self._format_date(v),
            'currency': lambda v: float(v) if v else 0.0,
            'boolean': lambda v: str(v).lower() in ('true', '1', 'yes'),
            'lookup': lambda v: self._lookup_value(v, context),
            'concat': lambda v: f"{context.get('prefix', '')}{v}{context.get('suffix', '')}",
            'split': lambda v: str(v).split(',') if v else [],
        }
        
        transformer = transformations.get(transform_type)
        if transformer:
            try:
                return transformer(value)
            except Exception as e:
                return value  # 转换失败时返回原值
        return value
    
    async def _send_to_target(
        self,
        endpoint: IntegrationEndpoint,
        data: List[Dict],
        upsert_mode: str = 'insert'
    ) -> Dict:
        """发送数据到目标端点"""
        result = {'success_count': 0, 'errors': []}
        
        for record in data:
            try:
                # 根据端点类型调用不同的发送逻辑
                if endpoint.type == IntegrationType.REST_API:
                    response = await self._call_rest_api(endpoint, record, method='POST')
                    if response.status_code in [200, 201]:
                        result['success_count'] += 1
                    else:
                        result['errors'].append({
                            'record_hash': record.get('_hash'),
                            'status': response.status_code,
                            'body': response.text[:200]
                        })
                        
                elif endpoint.type == IntegrationType.DATABASE:
                    await self._upsert_database(endpoint, record, upsert_mode)
                    result['success_count'] += 1
                    
            except Exception as e:
                result['errors'].append({
                    'record_hash': record.get('_hash'),
                    'error': str(e)
                })
        
        return result
    
    def _log_audit(self, action: str, target: str, details: Dict):
        """记录审计日志"""
        log_entry = {
            'timestamp': datetime.utcnow().isoformat(),
            'action': action,
            'target': target,
            'details': details,
            'user': 'system'  # 或从上下文获取实际用户
        }
        self.audit_log.append(log_entry)
        
        # 在生产环境中，这里应该写入持久化存储或 SIEM


# ===== 使用示例 =====

async def main():
    integrator = LowCodeIntegrator(config={'environment': 'production'})
    
    # 注册端点
    integrator.register_endpoint(IntegrationEndpoint(
        id='salesforce',
        name='Salesforce CRM',
        type=IntegrationType.REST_API,
        base_url='https://crm.salesforce.com/services/data/v56.0',
        credentials_ref='secret:sf-oauth-token',
        rate_limit_rpm=100
    ))
    
    integrator.register_endpoint(IntegrationEndpoint(
        id='postgres_inventory',
        name='Inventory Database',
        type=IntegrationType.DATABASE,
        credentials_ref='secret:db-connection-string'
    ))
    
    # 定义映射：库存数据 → Salesforce 产品
    integrator.define_mapping(
        mapping_name='inv_to_sf_product',
        source_system='postgres_inventory',
        target_system='salesforce',
        field_mappings=[
            DataMapping(source_field='sku', target_field='ProductCode'),
            DataMapping(source_field='product_name', target_field='Name'),
            DataMapping(source_field='description', target_field='Description'),
            DataMapping(
                source_field='unit_price', 
                target_field='UnitPrice',
                transformation='currency'
            ),
            DataMapping(
                source_field='is_active',
                target_field='IsActive',
                transformation='boolean'
            ),
            DataMapping(
                source_field='category',
                target_field='Family',
                default_value='Standard'
            )
        ]
    )
    
    # 模拟源数据
    inventory_items = [
        {'sku': 'SKU-001', 'product_name': 'Widget A', 'unit_price': '29.99', 'is_active': '1'},
        {'sku': 'SKU-002', 'product_name': 'Widget B', 'unit_price': '49.99', 'is_active': '0'},
        # ... 更多记录
    ]
    
    # 执行同步
    result = await integrator.sync_data('inv_to_sf_product', inventory_items)
    
    print(f"\nSync Results:")
    print(f"  Processed: {result['total_source_records']} records")
    print(f"  Success: {result['sent_to_target']} ({result['success_rate']:.1f}%)")
    print(f"  Errors: {len(result['errors'])}")
```

---

## ⚙️ 低代码治理框架

### 安全与合规策略

```yaml
# lowcode_governance.yaml - 低代码平台治理配置

governance_framework:
  name: Hermes Low-Code Governance Policy
  version: "2.0"
  effective_date: "2026-01-01"
  
security_policies:
  authentication:
    required: true
    allowed_methods:
      - oauth2
      - saml
      - oidc
    session_timeout_minutes: 480  # 8 hours
    mfa_required_for:
      - admin_roles
      - financial_apps
      - personal_data_access
  
  authorization:
    model: rbac  # Role-Based Access Control
    roles:
      - name: app_admin
        permissions: ['read', 'write', 'delete', 'configure', 'deploy']
        approval_required_for: ['production_deploy']
      
      - name: app_developer
        permissions: ['read', 'write', 'test']
        scope: assigned_applications_only
      
      - name: business_user
        permissions: ['read']
        scope: assigned_modules_only
        
  data_classification:
    levels:
      - level: PUBLIC
        encryption: optional
        audit_logging: false
        retention_days: 365
        
      - level: INTERNAL
        encryption: at_rest
        audit_logging: true
        retention_days: 1095  # 3 years
        
      - level: CONFIDENTIALIAL
        encryption: at_rest_and_in_transit
        audit_logging: detailed
        retention_days: 2555  # 7 years
        masking_rules:
          - fields: [ssn, credit_card, bank_account]
            mask_pattern: "***-***-****-{last_4}"
            
      - level: RESTRICTED
        encryption: end_to_end
        audit_logging: real_time_alerting
        retention_days: permanent
        access_control: need_toKnow_basis
        dlp_enabled: true

code_quality:
  review_requirements:
    min_reviewers: 1
    auto_approve_rules:
      - author_is_admin: false
      - changes_only_in_documentation: true
      - test_coverage_above: 80
      - no_security_sensitive_changes: true
    
  static_analysis:
    tools:
      - name: mendix-static-analysis
        ruleset: strict
        fail_on:
          - critical_severity
          - security_findings
          
  testing:
    unit_test_coverage_threshold: 70
    integration_test_required: true
    uat_signoff_required_for: production

deployment_pipeline:
  environments:
    - name: development
      auto_deploy: true
      required_checks: ['unit_tests_pass', 'no_critical_vulnerabilities']
      rollback_allowed: true
      
    - name: acceptance
      auto_deploy: false
      approval_required_from: ['business_owner', 'qa_lead']
      required_checks: ['integration_tests_pass', 'performance_benchmark']
      
    - name: production
      auto_deploy: false
      approval_required_from: ['app_admin', 'security_team', 'change_manager']
      required_checks: ['all_tests_pass', 'security_scan_clean', 'backup_completed']
      deployment_window:
        allowed_days: [Monday, Tuesday, Wednesday, Thursday]
        allowed_hours: ["06:00-22:00"]
        blackout_periods:
          - start: "2026-11-25"
            end: "2026-11-28"
            reason: "Black Friday - freeze period"
      rollback_plan_required: true
      post_deployment_verification:
        health_check_url: "/api/health"
        expected_status_code: 200
        max_response_time_ms: 2000

performance_standards:
  page_load_time:
    target_seconds: 3.0
    warning_threshold: 5.0
    critical_threshold: 10.0
    
  api_response_time:
    p50_target_ms: 200
    p95_target_ms: 500
    p99_target_ms: 1000
    
  concurrent_users:
    expected_peak: 500
    load_test_required: true
    scaling_policy: horizontal_auto_scale

compliance_mappings:
  gdpr:
    applicable: true
    dpia_required_for: new_applications_processing_personal_data
    data_subject_rights_implemented:
      - right_to_access
      - right_to_erasure
      - right_to_portability
      - right_to_rectification
      
  sox:
    applicable: true
    segregation_of_duties_enforced: true
    change_management_audit_trail: immutable
    financial_report_data_integrity: verified_monthly
```

---

## 🔗 相关技能

- [Hermes-Skill-Cloud-Architect](../cloud/cloud-architect/SKILL.md) - 低代码云部署
- [Hermes-Skill-API-Doc-Generator](../development/api-doc-generator/SKILL.md) - API 集成文档
- [Hermes-Skill-Agile-Project-Manager](../productivity/agile-project-manager/SKILL.md) - 低代码项目管理

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 支持平台 | Mendix, OutSystems, Power Platform, Retool, Airtable, etc. |
| 集成模式 | 15+ 种标准模式 |
| 数据映射 | 自动化转换管道 |
| 治理框架 | 安全、合规、性能、质量全覆盖 |
| 代码示例 | Python, YAML, JavaScript |
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
**适用场景**: 企业应用快速开发 | 数字化转型 | 内部工具建设 | 集成平台 | 低代码治理
