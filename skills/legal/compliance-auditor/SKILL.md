# Hermes-Skill-Compliance-Auditor

> 🛡️ **企业合规审计专家** | GDPR/CCPA/SOC2/HIPAA | 自动化合规检查与报告

---

## 📋 技能概述

Hermes-Skill-Compliance-Auditor 是一个专业的企业合规 AI 助手，专注于帮助组织实现和维护各种法规标准的合规性。涵盖数据隐私（GDPR、CCPA）、安全标准（SOC2、ISO27001）、行业特定法规（HIPAA、PCI-DSS）的自动化审计、风险评估和持续监控。

### 合规能力矩阵

```
┌─────────────────────────────────────────────────────────────┐
│                  Compliance Framework                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🔒 Data Privacy     │  🔐 Security        │  🏥 Industry   │
│  ├── GDPR (EU)      │  ├── SOC 2 Type II  │  ├── HIPAA    │
│  ├── CCPA (CA)      │  ├── ISO 27001      │  ├── PCI-DSS  │
│  ├── LGPD (Brazil)  │  ├── NIST CSF       │  ├── GLBA     │
│  └── POPIA (SA)     │  └── CIS Controls   │  └── FERPA    │
│                                                             │
│  📋 Audit Process    │  ⚠️ Risk Mgmt       │  📊 Reporting  │
│  ├── Gap Analysis   │  ├── Risk Assessment│  ├── Evidence │
│  ├── Control Testing│  ├── Mitigation     │  │   Collection│
│  ├── Remediation    │  └── Monitoring     │  ├── Reports   │
│  └── Continuous     │                     │  └── Dashboards│
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 合规评估

```bash
# GDPR 合规检查
/compliance gdpr-audit "user-data-processing"

# SOC2 控制测试
/compliance soc2-test "access-control"

# 风险评估
/compliance risk-assess "data-breach-scenario"

# 政策生成
/compliance generate-policy "data-retention"
```

### 监控配置

```bash
# 设置数据访问日志
/compliance setup-logging "api-access"

# 配置异常检测
/compliance anomaly-detection "unauthorized-access"

# 生成合规报告
/compliance report "Q1-2026-compliance"
```

---

## 🇪🇺 GDPR 合规框架

### 核心原则实施清单

```markdown
## GDPR 7 Principles Implementation Checklist

### 1. Lawfulness, Fairness, and Transparency (合法性、公平性、透明度)
- [ ] **Privacy Notice** 已发布且易于理解？
- [ ] 数据处理目的已明确告知数据主体？
- [ ] 法律依据已记录（同意/合同/合法利益等）？
- [ ] 使用清晰易懂的语言？

### 2. Purpose Limitation (目的限制)
- [ ] 数据仅用于收集时声明的目的？
- [ ] 新用途需要新的法律依据？
- [ ] 目的变更通知机制已建立？
- [ ] 数据最小化原则已应用？

### 3. Data Minimization (数据最小化)
- [ ] 仅收集必要的数据字段？
- [ ] 定期审查并删除不必要的数据？
- [ ] 匿名化/假名化技术已应用？
- [ ] 数据保留期限已定义？

### 4. Accuracy (准确性)
- [ ] 数据更新机制已建立？
- [ ] 错误纠正流程已文档化？
- [ ] 数据质量标准已定义？
- [ ] 不准确数据的处理程序？

### 5. Storage Limitation (存储限制)
- [ ] 每类数据的保留期限已定义？
- [ ] 自动删除/归档流程已实施？
- [ ] 数据映射（Data Mapping）已完成？
- [ ] 删除验证机制已存在？

### 6. Integrity and Confidentiality (完整性和保密性)
- [ ] 适当的技术和组织措施已实施？
- [ ] 访问控制基于最小权限原则？
- [ ] 加密（传输和静态）已启用？
- [ ] 安全事件响应计划已制定？

### 7. Accountability (问责制)
- [ ] DPO（数据保护官）已任命？
- [ ] 处理活动记录（RoPA）已维护？
- [ ] DPIA（数据保护影响评估）流程已建立？
- [ ] 员工培训计划已实施？
```

### 技术实现：数据主体权利 (DSR) 系统

```python
# dsr_automation.py - 数据主体权利请求自动化处理

from dataclasses import dataclass
from enum import Enum
from typing import List, Dict, Optional
from datetime import datetime, timedelta
import json
import hashlib
import logging

class DSRType(Enum):
    ACCESS = "access"                    # 访问权
    RECTIFICATION = "rectification"      # 更正权
    ERASURE = "erasure"                 # 删除权（被遗忘权）
    RESTRICT = "restrict_processing"    # 限制处理权
    PORTABILITY = "portability"         # 可携带权
    OBJECT = "object"                   # 反对权

@dataclass
class DSRRequest:
    request_id: str
    dsr_type: DSRType
    subject_id: str                    # 数据主体标识
    identity_verified: bool
    created_at: datetime
    due_date: datetime                 # 法定期限（通常30天）
    status: str = "pending"
    evidence: List[Dict] = None
    
    @property
    def is_overdue(self) -> bool:
        return datetime.now() > self.due_date and self.status != "completed"

class DSRAutomationSystem:
    """
    GDPR 数据主体权利自动化处理系统
    符合 GDPR Article 12-22 要求
    """
    
    def __init__(self, db_connection, encryption_key: str):
        self.db = db_connection
        self.encryption_key = encryption_key
        self.logger = logging.getLogger(__name__)
        
    async def submit_dsr_request(
        self,
        dsr_type: DSRType,
        subject_email: str,
        verification_data: Dict
    ) -> DSRRequest:
        """
        提交 DSR 请求
        GDPR Art. 12: Controller must provide information without undue delay
        """
        
        # 1. 身份验证
        subject_id = await self._verify_identity(subject_email, verification_data)
        if not subject_id:
            raise ValueError("Identity verification failed")
        
        # 2. 创建请求记录
        request = DSRRequest(
            request_id=self._generate_request_id(),
            dsr_type=dsr_type,
            subject_id=subject_id,
            identity_verified=True,
            created_at=datetime.utcnow(),
            due_date=datetime.utcnow() + timedelta(days=30),  # GDPR 30天期限
            status="pending",
            evidence=[]
        )
        
        await self._save_request(request)
        
        # 3. 发送确认邮件（GDPR Art. 19）
        await self._send_confirmation(request)
        
        # 4. 启动自动处理流程
        asyncio.create_task(self.process_dsr_request(request))
        
        self.logger.info(f"DSR request {request.request_id} submitted for {dsr_type.value}")
        
        return request
    
    async def process_dsr_request(self, request: DSRRequest):
        """根据 DSR 类型执行相应操作"""
        
        try:
            if request.dsr_type == DSRType.ACCESS:
                await self._handle_access_request(request)
            
            elif request.dsr_type == DSRType.ERASURE:
                await self._handle_erasure_request(request)
            
            elif request.dsr_type == DSRType.PORTABILITY:
                await self._handle_portability_request(request)
            
            elif request.dsr_type == DSRType.RECTIFICATION:
                await self._handle_rectification_request(request)
            
            # 更新状态为完成
            request.status = "completed"
            await self._update_request_status(request)
            
            # 发送完成通知
            await self._send_completion_notification(request)
            
        except Exception as e:
            request.status = "failed"
            await self._update_request_status(request)
            self.logger.error(f"DSR processing failed: {e}")
            raise
    
    async def _handle_access_request(self, request: DSRRequest):
        """
        处理访问权请求 (GDPR Art. 15)
        返回数据主体所有个人数据的副本
        """
        
        personal_data = {}
        
        # 1. 从各数据源收集数据
        data_sources = [
            ('users', 'SELECT * FROM users WHERE id = $1'),
            ('orders', 'SELECT * FROM orders WHERE user_id = $1'),
            ('analytics', 'SELECT * FROM user_analytics WHERE user_id = $1'),
            ('support_tickets', 'SELECT * FROM support_tickets WHERE user_id = $1'),
            ('marketing_consents', 'SELECT * FROM consents WHERE user_id = $1')
        ]
        
        for table_name, query in data_sources:
            rows = await self.db.fetch(query, request.subject_id)
            if rows:
                # 脱敏处理（如需要）
                personal_data[table_name] = [
                    self._sanitize_for_export(row) for row in rows
                ]
        
        # 2. 生成机器可读格式 (JSON/CSV)
        export_data = {
            'request_id': request.request_id,
            'export_date': datetime.utcnow().isoformat(),
            'data_controller': 'Hermes Inc.',
            'data_subject': {
                'id': request.subject_id,
                'type': 'hashed_identifier'
            },
            'personal_data': personal_data,
            'categories': list(personal_data.keys()),
            'retention_info': {
                'legal_basis': 'consent',
                'retention_period': '3 years from last interaction'
            }
        }
        
        # 3. 加密存储（GDPR 要求安全传输）
        encrypted_export = self._encrypt_data(json.dumps(export_data))
        
        # 4. 生成下载链接（7天有效）
        download_url = await self._generate_secure_download_link(encrypted_export)
        
        # 5. 记录证据
        request.evidence.append({
            'action': 'data_export',
            'timestamp': datetime.utcnow().isoformat(),
            'data_categories': list(personal_data.keys()),
            'record_count': sum(len(v) for v in personal_data.values())
        })
    
    async def _handle_erasure_request(self, request: DSRRequest):
        """
        处理删除权请求 (GDPR Art. 17)
        被遗忘权 - 删除所有个人数据
        """
        
        # 1. 识别所有数据存储位置
        deletion_tasks = []
        
        # 主数据库
        deletion_tasks.append(('users', f"DELETE FROM users WHERE id = '{request.subject_id}'"))
        deletion_tasks.append(('orders', f"UPDATE orders SET user_id = NULL, anonymized = true WHERE user_id = '{request.subject_id}'"))
        deletion_tasks.append(('analytics', f"DELETE FROM user_analytics WHERE user_id = '{request.subject_id}'"))
        
        # 缓存层
        deletion_tasks.append(('redis_cache', f"DEL user:{request.subject_id}:*"))
        
        # 备份系统（标记待删除）
        deletion_tasks.append(('backups', f"MARK_FOR_DELETION user_data_{request.subject_id}"))
        
        # 日志系统（保留匿名化版本用于审计）
        deletion_tasks.append(('audit_logs', f"ANONYMIZE audit_logs WHERE user_id = '{request.subject_id}'"))
        
        # 2. 执行删除（考虑合法保留义务）
        legal_holds = await self._check_legal_holds(request.subject_id)
        
        executed_deletions = []
        blocked_deletions = []
        
        for source, command in deletion_tasks:
            if not any(hold['source'] == source for hold in legal_holds):
                await self.db.execute(command)
                executed_deletions.append(source)
            else:
                blocked_deletions.append({
                    'source': source,
                    'reason': 'Legal hold in place',
                    'hold_details': next(
                        (h for h in legal_holds if h['source'] == source),
                        None
                    )
                })
        
        # 3. 通知第三方数据处理者 (GDPR Art. 19)
        third_parties = ['analytics_vendor', 'email_provider', 'cloud_storage']
        for party in third_parties:
            await self._notify_third_party_erasure(party, request.subject_id)
        
        # 4. 记录证据
        request.evidence.append({
            'action': 'data_erasure',
            'timestamp': datetime.utcnow().isoformat(),
            'deleted_from': executed_deletions,
            'blocked_by_legal_hold': blocked_deletions,
            'third_parties_notified': third_parties
        })
    
    def _check_legal_holds(self, subject_id: str) -> List[Dict]:
        """检查是否存在法律保留义务"""
        # 示例：税务记录需保留 7 年
        # 合同纠纷中的证据需保留
        return [
            {'source': 'orders', 'reason': 'Tax retention requirement', 'expires': '2030-12-31'}
        ]
```

---

## 🇺🇸 CCPA/CPRA 合规指南

### 消费者隐私权实施

```python
# ccpa_compliance.py - 加州消费者隐私法案合规系统

from enum import Enum
from typing import Optional, List
from dataclasses import dataclass
from datetime import datetime

class CCPARight(Enum):
    KNOW = "know"                      # 知情权
    DELETE = "delete"                  # 删除权
    OPT_OUT_SALE = "opt_out_sale"      # 选择退出销售权
    CORRECT = "correct"                # 更正权
    LIMIT_USE = "limit_use"            # 限制使用敏感信息权
    NON_DISCRIMINATION = "non_discrimination"  # 非歧视权
    DATA_PORTABILITY = "portability"   # 数据可携带权

@dataclass
class ConsumerRequest:
    request_id: str
    right: CCPARight
    consumer_identity: str
    verification_token: str
    created_at: datetime
    deadline: datetime  # CCPA: 45 days (可延长45天)
    status: str = "received"
    business_response: Optional[str] = None

class CCPAComplianceEngine:
    """
    CCPA/CPRA 合规引擎
    实现 California Consumer Privacy Act 要求
    """
    
    def __init__(self, config: dict):
        self.config = config
        self.opt_out_registry = OptOutRegistry()
        self.sensitive_data_classifier = SensitiveDataClassifier()
    
    async def process_consumer_request(self, request: ConsumerRequest) -> dict:
        """处理消费者隐私请求"""
        
        if request.right == CCPARight.OPT_OUT_SALE:
            return await self._handle_opt_out_sale(request)
        
        elif request.right == CCPARight.KNOW:
            return await self._handle_know_request(request)
        
        elif request.right == CCPARight.DELETE:
            return await self._handle_delete_request(request)
        
        elif request.right == CCPARight.LIMIT_USE:
            return await self._handle_limit_sensitive_use(request)
    
    async def _handle_opt_out_sale(self, request: ConsumerRequest) -> dict:
        """
        处理选择退出个人信息销售请求
        CCPA §1798.120 & CPRA 扩展
        """
        
        # 1. 注册到 Do Not Sell 列表
        opt_out_record = await self.opt_out_registry.register(
            consumer_id=request.consumer_identity,
            opt_out_method="direct_request",  # 或 "global_signal"
            request_id=request.request_id,
            effective_immediately=True
        )
        
        # 2. 通知所有数据共享合作伙伴
        partners_to_notify = await self._get_sharing_partners()
        for partner in partners_to_notify:
            await self._send_opt_out_notice(partner, opt_out_record)
        
        # 3. 更新内部标记
        await self._update_consumer_flags(
            consumer_id=request.consumer_identity,
            flags={
                'do_not_sell': True,
                'opt_out_date': datetime.utcnow(),
                'opt_out_request_id': request.request_id
            }
        )
        
        # 4. 识别并停止跨上下文行为广告
        await self._disable_behavioral_advertising(request.consumer_identity)
        
        return {
            'status': 'success',
            'message': 'Consumer has opted out of sale of personal information',
            'effective_date': opt_out_record.effective_date,
            'confirmation_code': opt_out_record.confirmation_code,
            'next_steps': [
                'Opt-out is effective immediately',
                'Previously shared data will be covered by updated agreements with partners',
                'Consumer may revoke this opt-out at any time'
            ]
        }
    
    async def _handle_limit_sensitive_use(self, request: ConsumerRequest) -> dict:
        """
        CPRA 新增：限制使用敏感个人信息
        敏感信息包括：精确地理位置、种族、宗教、性取向、
        工会成员资格、私人通信内容、基因数据等
        """
        
        # 1. 分类消费者的敏感数据
        sensitive_data_inventory = await self._inventory_consumer_sensitive_data(
            request.consumer_identity
        )
        
        # 2. 应用限制
        limited_categories = []
        for data_item in sensitive_data_inventory:
            category = self.sensitive_data_classifier.classify(data_item)
            
            if category.is_sensitive:
                await self._apply_usage_limit(
                    consumer_id=request.consumer_identity,
                    data_category=category.name,
                    allowed_uses=['necessary_service_provision', 'security'],
                    prohibited_uses=['advertising', 'profiling', 'sale']
                )
                limited_categories.append(category.name)
        
        return {
            'status': 'success',
            'limited_categories': limited_categories,
            'total_sensitive_data_points': len(sensitive_data_inventory),
            'consumer_rights_statement': (
                "Your sensitive personal information use has been limited. "
                "We will only use this information for performing services, "
                "providing security, and complying with laws."
            )
        }
    
    def detect_global_privacy_signal(self, request_headers: dict) -> bool:
        """
        检测全局隐私信号 (GPC - Global Privacy Control)
        规范: https://globalprivacycontrol.org/
        """
        gpc_signal = False
        
        # 检查 Sec-GPC header
        if request_headers.get('sec-gpc') == '1':
            gpc_signal = True
        
        # 检查 DNT (Do Not Track) + 特定上下文
        if request_headers.get('dnt') == '1':
            # DNT 在某些情况下可视为 GPC 信号
            if self.config.get('interpret_dnt_as_gpc'):
                gpc_signal = True
        
        # 检查浏览器 API (未来浏览器原生支持)
        # navigator.globalPrivacyControl
        
        return gpc_signal
```

---

## 🔐 SOC 2 Type II 合规

### Trust Service Criteria (TSC) 映射

```yaml
# soc2_controls.yaml - SOC 2 控制目标映射

trust_service_criteria:
  common_criteria:
    CC6.1:  # Logical Access Security
      name: "Logical and Physical Access Controls"
      description: "Entity restricts logical access to system components and data"
      implementation:
        - IAM policy enforcement via AWS IAM/Azure AD
        - MFA required for all privileged access
        - Just-in-time (JIT) access for production systems
        - Session timeout: 15 minutes idle, 8 hours maximum
        - Password complexity: 14+ chars, mixed case, numbers, symbols
      evidence_artifacts:
        - IAM policy documents
        - MFA enrollment reports
        - Access review logs
        - Session configuration screenshots
      
    CC6.2:  # System Operation Controls
      name: "System Operations"
      description: "Entity uses system software and infrastructure monitoring"
      implementation:
        - 24/7 monitoring via Datadog/New Relic
        - Automated alerting for anomalies
        - Change management via approved ticketing system
        - Incident response procedures documented
        - Backup verification testing (quarterly)
      evidence_artifacts:
        - Monitoring dashboard configurations
        - Alert runbooks
        - Change management tickets
        - Incident reports
        - Backup test results
        
    CC6.3:  # Change Management
      name: "Change Management"
      description: "Entity authorizes, designs, develops, configures..."
      implementation:
        - All changes require approval via Jira/ServiceNow
        - Production deployments follow CI/CD pipeline
        - Rollback procedures tested quarterly
        - Emergency change process defined (< 1 hour approval)
        - Change advisory board (CAB) reviews weekly
      evidence_artifacts:
        - Change requests with approvals
        - Deployment logs
        - Rollback test documentation
        - CAB meeting minutes
        
    CC6.6:  # Risk Mitigation
      name: "Risk Mitigation"
      description: "Entity designs, develops, implements, and operates..."
      implementation:
        - Annual risk assessment completed
        - Vendor risk management program
        - Business continuity plan (tested annually)
        - Disaster recovery plan (RTO < 4 hours, RPO < 1 hour)
        - Cyber insurance coverage maintained
      evidence_artifacts:
        - Risk assessment report
        - Vendor assessment results
        - BCP/DR test results
        - Insurance certificates

  security_criteria:
    CCM1.1:  # Access Control (EC2)
      name: "Access to assets is limited to authorized users"
      control_mapping: "CC6.1"
      
    CCM2.1:  # Data Encryption
      name: "Data is encrypted at rest and in transit"
      implementation:
        - AES-256 encryption at rest (AWS KMS-managed keys)
        - TLS 1.3 for all data in transit
        - Certificate management via AWS Certificate Manager
        - Key rotation every 90 days
      evidence_artifacts:
        - Encryption configuration screenshots
        - TLS certificate details
        - Key rotation logs
        
  availability_criteria:
    A1.1:  # System Availability
      name: "Systems are available as committed"
      sla_targets:
        uptime_percentage: 99.9%
        planned_maintenance_window: "Sunday 02:00-06:00 UTC"
        incident_response_time_p1: "< 15 minutes"
        mean_time_to_recover: "< 4 hours"
      monitoring:
        - Synthetic monitoring (5 locations globally)
        - Real user monitoring (RUM)
        - Dependency health checks
        
  confidentiality_criteria:
    C1.1:  # Data Classification
      name: "Data is classified and handled appropriately"
      classification_levels:
        - Public: "No restrictions"
        - Internal: "Employee access only"
        - Confidential: "Need-to-know basis"
        - Restricted: "Highest sensitivity, encryption required"
      data_handling_procedures:
        - DLP (Data Loss Prevention) rules configured
        - Email encryption for Confidential+
        - USB port disabled on workstations
        - Cloud access security broker (CASB) deployed
        
  privacy_criteria:
    P1.1:  # Personal Information Handling
      name: "Personal information is handled per commitments"
      implementation:
        - Privacy notice published and accessible
        - Consent management platform implemented
        - Data subject rights request workflow automated
        - Cross-border transfer mechanisms (SCCs) in place
        - Privacy impact assessments for new projects
```

---

## 📊 自动化合规仪表盘

```python
# compliance_dashboard.py - 实时合规状态监控

from dataclasses import dataclass
from typing import Dict, List
from enum import Enum
from datetime import datetime, timedelta

class ComplianceStatus(Enum):
    COMPLIANT = "compliant"
    PARTIALLY_COMPLIANT = "partially_compliant"
    NON_COMPLIANT = "non_compliant"
    NOT_ASSESSED = "not_assessed"
    OVERDUE = "overdue"

@dataclass
class ControlStatus:
    control_id: str
    control_name: string
    framework: string  # GDPR, SOC2, etc.
    status: ComplianceStatus
    last_tested: datetime
    next_review_due: datetime
    evidence_count: int
    findings: List[Dict]
    owner: string

@dataclass
class FrameworkSummary:
    framework_name: string
    total_controls: int
    compliant: int
    partially_compliant: int
    non_compliant: int
    compliance_percentage: float
    critical_findings: int
    overdue_items: int

class ComplianceDashboard:
    def __init__(self, database_client):
        self.db = database_client
    
    async def get_overview(self) -> Dict:
        """获取合规概览"""
        
        frameworks = ['GDPR', 'CCPA', 'SOC2', 'ISO27001']
        summaries = []
        
        for framework in frameworks:
            summary = await self._get_framework_summary(framework)
            summaries.append(summary)
        
        # 关键风险指标
        critical_risks = await self._get_critical_risks()
        
        # 即将到期项目
        upcoming_deadlines = await self._get_upcoming_deadlines(days=30)
        
        return {
            'generated_at': datetime.utcnow().isoformat(),
            'framework_summaries': summaries,
            'overall_health_score': self._calculate_health_score(summaries),
            'critical_risks': critical_risks,
            'upcoming_action_items': upcoming_deadlines,
            'trend_analysis': await self._get_trend_data(months=6)
        }
    
    async def _get_framework_summary(self, framework: str) -> FrameworkSummary:
        """获取单个框架的合规摘要"""
        
        controls = await self.db.fetch("""
            SELECT 
                status,
                COUNT(*) as count
            FROM compliance_controls
            WHERE framework = $1
            GROUP BY status
        """, framework)
        
        status_counts = {row['status']: row['count'] for row in controls}
        total = sum(status_counts.values())
        
        compliant = status_counts.get(ComplianceStatus.COMPLIANT.value, 0)
        non_compliant = status_counts.get(ComplianceStatus.NON_COMPLIANT.value, 0)
        
        # 计算关键发现
        critical_findings = await self.db.fetchval("""
            SELECT COUNT(*) FROM findings
            WHERE framework = $1 AND severity = 'critical' AND status != 'remediated'
        """, framework)
        
        # 过期项目
        overdue = await self.db.fetchval("""
            SELECT COUNT(*) FROM compliance_controls
            WHERE framework = $1 AND next_review_due < NOW() 
            AND status != $2
        """, framework, ComplianceStatus.COMPLIANT.value)
        
        return FrameworkSummary(
            framework_name=framework,
            total_controls=total,
            compliant=compliant,
            partially_compliant=status_counts.get(ComplianceStatus.PARTIALLY_COMPLIANT.value, 0),
            non_compliant=non_compliant,
            compliance_percentage=(compliant / total * 100) if total > 0 else 0,
            critical_findings=critical_findings,
            overdue_items=overdue
        )
    
    def generate_executive_report(self) -> str:
        """生成高管摘要报告"""
        
        overview = await self.get_overview()
        
        report = f"""# Executive Compliance Report
**Date**: {datetime.utcnow().strftime('%B %d, %Y')}
**Reporting Period**: {(datetime.utcnow() - timedelta(days=30)).strftime('%B %d')} - {datetime.utcnow().strftime('%B %d')}

## Overall Health Score: {overview['overall_health_score']}/100

### Framework Status

| Framework | Compliance % | Critical Issues | Overdue |
|-----------|-------------|-----------------|---------|
"""
        
        for summary in overview['framework_summaries']:
            emoji = "🟢" if summary.compliance_percentage >= 90 else \
                    "🟡" if summary.compliance_percentage >= 70 else "🔴"
            
            report += f"| {summary.framework_name} | {summary.compliance_percentage:.1f}% | {summary.critical_findings} | {summary.overdue_items} |\n"
        
        report += f"""

## Critical Risks Requiring Immediate Attention

"""
        
        for i, risk in enumerate(overview['critical_risks'], 1):
            report += f"""{risk['title']}
- **Risk Level**: {risk['level'].upper()}
- **Framework**: {risk['framework']}
- **Owner**: {risk['owner']}
- **Recommended Action**: {risk['recommendation']}

"""
        
        return report
```

---

## 🔗 相关技能

- [Hermes-Skill-Cybersecurity-Auditor](../security/cybersecurity-auditor/SKILL.md) - 安全审计
- [Hermes-Skill-Legal-Document-Analyzer](../legal/legal-document-analyzer/SKILL.md) - 法律文档分析
- [Hermes-Skill-Cloud-Architect](../cloud/cloud-architect/SKILL.md) - 云端合规架构

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 支持法规 | GDPR, CCPA/CPRA, SOC2, ISO27001, HIPAA, PCI-DSS |
| 控制模板库 | 200+ 预构建控制 |
| 自动化工作流 | DSR 处理、风险评估、证据收集 |
| 报告模板 | 高管报告、审计师报告、董事会报告 |
| 集成能力 | SIEM、IAM、DLP、GRC 平台 |
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
**适用场景**: 企业合规 | 数据隐私 | 安全审计 | 风险管理
