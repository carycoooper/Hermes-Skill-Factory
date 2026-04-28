---
name: cybersecurity-auditor
description: "Hermes-Skill-Cybersecurity-Auditor - 网络安全审计专家，提供渗透测试、漏洞扫描、安全合规检查、威胁建模、安全代码审查和事件响应指南。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [cybersecurity, penetration-testing, vulnerability-scan, owasp, compliance, threat-modeling, incident-response, security-audit]
    related_skills: [security-auditor, api-testing-suite, cicd-pipeline-manager, code-review-pro]
    requires_toolsets: [web, terminal]
    config:
      - key: compliance_framework
        default: owasp_top_10
      - key: scan_depth
        default: comprehensive
---

# Hermes-Skill-Cybersecurity-Auditor (网络安全审计专家)

## 概述

**Hermes-Skill-Cybersecurity-Auditor** 是一个全面的网络安全审计与渗透测试辅助系统。它覆盖从威胁建模到漏洞扫描、从代码安全审查到合规性检查的全流程，帮助开发团队和安全工程师构建更安全的系统。

### 核心能力

- **威胁建模**: STRIDE/PASTA/DREAD 方法论自动应用
- **漏洞扫描**: OWASP Top 10 + SANS Top 25 + CWE 全面检测
- **渗透测试**: Web/API/Network/Mobile 全场景测试框架
- **安全代码审查**: 注入/XSS/CSRF/SSRF 等常见漏洞模式检测
- **合规检查**: GDPR/SOC2/ISO27001/PCI-DSS 自动化对照
- **事件响应**: IR 流程模板 + 取证指南 + 修复建议

---

## 威胁建模框架

### STRIDE 模型

```
Spoofing (身份伪装):
├── 攻击面: 登录接口 / API Key / Session Token
├── 缓解措施: MFA / Certificate Pinning / HSTS
└── 检查清单: 
    ├── [ ] 认证机制是否强制执行？
    ├── [ ] Session 是否有固定超时？
    └── [ ] Token 是否使用加密随机数？

Tampering (数据篡改):
├── 攻击面: 表单参数 / Cookie / HTTP Header / URL
├── 缓解措施: HMAC签名 / 数字签名 / 完整性校验
└── 检查清单:
    ├── [ ] 敏感操作是否有防篡改机制？
    ├── [ ] Cookie 是否设置 HttpOnly + Secure？
    └── [ ] 文件上传是否校验完整性？

Repudiation (抵赖):
├── 攻击面: 操作日志缺失 / 日志可被删除
├── 缓解措施: 不可变日志 / 审计追踪 / 多方签名
└── 检查清单:
    ├── [ ] 关键操作是否记录完整审计日志？
    ├── [ ] 日志是否写入不可变存储？
    └── [ ] 是否有时间戳和操作者身份？

Information Disclosure (信息泄露):
├── 攻击面: 错误信息泄露 / 调试模式 / 不安全的存储
├── 缓解措施: 加密存储 / 最小权限 / 数据脱敏
└── 检查清单:
    ├── [ ] 生产环境是否关闭调试模式？
    ├── [ ] 敏感数据是否加密存储（AES-256）？
    └── [ ] API 错误响应是否隐藏内部细节？

Denial of Service (拒绝服务):
├── 攻击面: 无限循环 / 资源耗尽 / 放大攻击
├── 缓解措施: Rate Limiting / 资源配额 / Circuit Breaker
└── 检查清单:
    ├── [ ] API 是否有速率限制？
    ├── [ ] 文件上传是否有大小限制？
    └── [ ] 查询是否有超时和结果集限制？

Elevation of Privilege (权限提升):
├── 攻击面: IDOR / 参数注入 / 权限绕过
├── 缓解措施: RBAC / ABAC / 最小权限原则
└── 检查清单:
    ├── [ ] 每个端点是否验证用户权限？
    ├── [ ] 是否存在 IDOR (不安全的直接对象引用)？
    └── [ ] 管理操作是否需要二次确认？
```

---

## OWASP Top 10 (2024) 检测

| # | 风险 | 检测方法 | 自动化工具 |
|---|------|---------|-----------|
| A01 | **Broken Access Control** | IDOR Testing / Permission Matrix | Burp Suite Access Control Scanner |
| A02 | **Cryptographic Failures | Weak Cipher Detection / Key Management | TestSSL.js / SSLyze |
| A03 | **Injection** | SQLi / NoSQLi / LDAPi / Command Injection | SQLMap / NoSQLMap |
| A04 | **Insecure Design** | Architecture Review / Threat Modeling | OWASP Threat Dragon |
| A05 | **Security Misconfiguration | Config Audit / Default Credential Check | Nmap / Nikto / WPScan |
| A06 | **Vulnerable Components | Dependency Scan / Known CVE Check | Snyk / Dependabot / OWASP DC |
| A07 | **Auth Failures | Auth Bypass / Brute Force Testing | Hydra / Burp Intruder |
|08 | **Software/Data Integrity | Deserialization / Supply Chain Attack | ysoserial / Sigstore |
| A09 | **Logging/Monitoring Failures | Log Analysis / Error Handling Review | ELK Stack Analyzer |
| A10 | **Server-Side Request Forgery | SSRF Payload Testing | SSRFmap |

### 自动化扫描流程

```bash
# 一键全扫描
/cyber scan full --target https://api.example.com --depth deep

# 分类扫描
/cyber scan injection --target app --types sqli,xss,cmdi
/cyber scan auth --target app --test login,register,password-reset

# 依赖漏洞扫描
/cyber scan dependencies --file package.json,requirements.txt,Cargo.toml

# 配置安全检查
/cyber scan config --target .env,.yaml,.json --check secrets,exposure

# 生成报告
/cyber report generate --format html,pdf,json --severity critical,high
```

---

## 渗透测试方法论

### PTES (Penetration Testing Execution Standard)

```
Phase 1: Pre-Engagement (前期交互)
├── 定义范围 (In-Scope / Out-of-Scope)
├── 签署 Rules of Engagement (RoE)
├── 法律授权确认
└── 准备测试环境

Phase 2: Intelligence Gathering (情报收集)
├── Passive Recon (被动收集)
│   ├── WHOIS / DNS Records
│   ├── Google Dorking
│   ├── GitHub/GitLab 信息泄露
│   └── Shodan/Censys 搜索
│
├── Active Recon (主动探测)
│   ├── Port Scanning (Nmap)
│   ├── Service Enumeration
│   ├── Directory Bruteforce
│   └── Subdomain Discovery
│
└── OSINT (开源情报)
    ├── Social Media Profile
    ├── Employee Information
    └── Technology Stack Fingerprint

Phase 3: Vulnerability Analysis (漏洞分析)
├── Automated Scanning (Nessus / OpenVAS)
├── Manual Testing (Burp Suite)
├── Code Review (SAST)
└── Business Logic Testing

Phase 4: Exploitation (漏洞利用)
├── Proof of Concept (PoC)
├── Impact Assessment
├── Lateral Movement (内网横向)
└── Privilege Escalation

Phase 5: Post-Exploitation (后渗透)
├── Data Exfiltration Simulation
├── Persistence Mechanisms
├── Covering Tracks
└── Documentation

Phase 6: Reporting (报告编写)
├── Executive Summary
├── Technical Findings
├── Risk Rating (CVSS v3.1)
├── Remediation Recommendations
└── Retesting Plan
```

---

## 安全代码审查

### 常见漏洞模式库

#### SQL Injection (SQLi)

```python
# ❌ VULNERABLE - 直接拼接
query = f"SELECT * FROM users WHERE id = {user_id}"

# ✅ SAFE - 参数化查询
cursor.execute("SELECT * FROM users WHERE id = %s", (user_id,))

# ✅ SAFEST - ORM + 参数化
User.objects.filter(id=user_id).first()
```

#### Cross-Site Scripting (XSS)

```javascript
// ❌ VULNERABLE - 直接渲染用户输入
<div>{user_input}</div>

// ✅ SAFE - HTML转义
<div>{escapeHtml(user_input)}</div>

// ✅ SAFEST - CSP + DOMPurify + Context-Aware Encoding
```

#### Server-Side Request Forgery (SSRF)

```python
# ❌ VULNERABLE - 用户控制URL
response = requests.get(user_provided_url)

# ✅ SAFE - 白名单验证
ALLOWED_DOMAINS = ['api.internal.com', 'cdn.example.com']
parsed = urlparse(url)
if parsed.netloc not in ALLOWED_DOMAINS:
    raise ValueError("URL not allowed")
    
# ✅ SAFEST - 内部DNS解析 + 禁止私有IP
def is_safe_url(url):
    parsed = urlparse(url)
    ip = socket.gethostbyname(parsed.hostname)
    return not ipaddress.ip_address(ip).is_private
```

---

## 合规检查矩阵

```yaml
compliance_matrix:
  gdpr:
    articles: ["Art.5 (Data Minimization)", "Art.25 (Design by Default)", "Art.32 (Security)"]
    checks:
      - data_encryption_at_rest: required
      - data_encryption_in_transit: required
      - access_logging: required
      - data_retention_policy: required
      - breach_notification_72h: required
      
  soc2_type2:
    categories:
      - Security: CC6.1-CC7.3
      - Availability: A1.1-A1.3
      - Processing Integrity: PI1.1-PI1.3
      - Confidentiality: C1.1-C1.3
      - Privacy: P1.1-P1.3
      
  pci_dss:
    requirements:
      - REQ_1: Install and maintain network security controls
      - REQ_2: Apply secure configurations
      - REQ_3: Protect stored cardholder data
      - REQ_4: Encrypt transmission of CHD
      - REQ_6: Develop and maintain secure systems
      - REQ_7: Restrict access to system components
      - REQ_8: Identify and authenticate users
      - REQ_10: Track and monitor access
      - REQ_11: Regularly test security systems
      - REQ_12: Maintain information security policy
```

---

## 事件响应 (IR)

### IR Playbook 模板

```
🚨 INCIDENT RESPONSE PLAYBOOK

Phase 1: Preparation (准备阶段)
├── IR Team Roster & Contact Info
├── Communication Channels (Slack/Phone/Bridge)
├── Tool Access Credentials (SIEM/EDR/Ticket System)
├── Runbooks for Common Incident Types
└── Vendor Escalation Contacts

Phase 2: Detection & Analysis (检测与分析)
├── Triage Checklist:
│   ├── What happened? (Initial classification)
│   ├── When did it start? (Timeline reconstruction)
│   ├── What's affected? (Scope assessment)
│   ├── How severe? (Impact rating P1-P4)
│   └── Is it ongoing? (Containment urgency)
│
├── Evidence Collection:
│   ├── Memory dump (if host accessible)
│   ├── Disk image (forensic copy)
│   ├── Network capture (PCAP)
│   ├── Log aggregation (all relevant sources)
│   └── Screenshot/documentation
│
└── Root Cause Analysis:
    ├── 5 Whys technique
    ├── Timeline reconstruction
    ├── Attack chain mapping (MITRE ATT&CK)
    └── Hypothesis testing

Phase 3: Containment (遏制阶段)
├── Immediate Actions:
│   ├── Isolate affected systems (network segmentation)
│   ├── Revoke compromised credentials
│   ├── Block malicious IPs/domains
│   └── Disable affected user accounts
│
├── Short-term Fixes:
│   ├── Patch identified vulnerability
│   ├── Update WAF rules
│   ├── Reset passwords (affected users)
│   └── Force re-authentication
│
└── Decision Point:
    ├── Continue operations with mitigations?
    ├── Or shut down affected service?
    └── Document decision rationale

Phase 4: Eradication (根除阶段)
├── Remove malware/persistence mechanisms
├── Close attack vectors completely
├── Verify no backdoors remain
└── Update detection rules (YARA/Sigma/YARA)

Phase 5: Recovery (恢复阶段)
├── Restore from clean backup (verified integrity)
├── Gradual ramp-up (monitor closely)
├── Enhanced monitoring during recovery period
└── Validate business functionality

Phase 6: Lessons Learned (事后复盘)
├── Timeline reconstruction (detailed)
├── What worked well?
├── What could be improved?
├── Action items with owners and deadlines
├── Update runbooks and playbooks
└── Executive summary report
```

---

*基于 OWASP ASVS / NIST Cybersecurity Framework / MITRE ATT&CK 设计*
*版本: 2.0.0 | 最后更新: 2026-04-27*
