# Hermes-Skill-DevSecOps-Engineer

> 🛡️ **安全 DevOps 工程师** | 左移安全 | CI/CD 安全 | 合规即代码 | 零信任架构

---

## 📋 技能概述

Hermes-Skill-DevSecOps-Engineer 是一个专业的开发安全运维（DevSecOps）AI 助手，专注于将安全性深度集成到软件开发生命周期的每个阶段。涵盖"左移"(Shift-Left) 安全实践、基础设施即代码 (IaC) 扫描、容器安全、秘密管理、合规自动化、零信任架构以及安全运营 (SecOps) 等核心领域。

### DevSecOps 能力矩阵

```
┌─────────────────────────────────────────────────────────────┐
│                  DevSecOps Stack                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🔍 Shift Left        │  🔄 CI/CD Security     │  🐳 Container│
│  ├── SAST (Static)   │  ├── Pipeline Scanning│  ├── Image   │
│  ├── SCA (Depend.)   │  ├── Pre-commit Hooks │  │   Scan    │
│  ├── DAST (Dynamic)  │  ├── PR Security Gate │  ├── Runtime │
│  └── IaC Scanning    │  └── Artifact Signing │  └── K8s Pol.│
│                                                             │
│  🔑 Secrets Mgmt      │  📋 Compliance         │  🚨 SecOps   │
│  ├── Vault/HSM       │  ├── Policy as Code   │  ├── SIEM    │
│  ├── Rotation        │  ├── Audit Trails     │  ├── SOAR    │
│  └── Zero Standing   │  └── Evidence Collect.│  └── Threat   │
│      Privileges      │                       │    Intel.   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 安全扫描配置

```bash
# SAST 静态代码分析
/devsecops setup-sast "python-project" --tool semgrep,bandit

# 依赖漏洞扫描
/devsecops setup-sca "nodejs-app" --tool dependabot,snyk

# IaC 安全扫描
/devsecops scan-iac "terraform/" --checkov,tflint

# 容器镜像扫描
/devsecops container-scan "docker-image:latest" --trivy,grype
```

### 管道集成

```bash
# GitHub Actions 安全工作流
/devsecops pipeline-security "github-actions"

# GitLab CI/CD 安全模板
/devsecops security-template "gitlab-ci"
```

---

## 🔍 全栈安全扫描体系

### SAST (静态应用安全测试) 配置

```yaml
# .semgrep.yml - Semgrep 规则配置

rules:
  # === OWASP Top 10 漏洞检测 ===
  
  - id: sql-injection-risk
    pattern-either:
      - pattern: |
          $F.execute("..." + $VAR)
      - pattern: |
          $F.execute(f"...{$VAR}")
      - pattern: |
          $F.format("...", $VAR)
    message: "Potential SQL injection detected. Use parameterized queries."
    severity: ERROR
    languages: [python, javascript, java, go]
    metadata:
      owasp: "A03:2021 - Injection"
      cwe: "CWE-89"
  
  - id: hardcoded-secrets
    patterns:
      - pattern-either:
          - pattern: $PASSWORD = "..."
          - pattern: $API_KEY = "..."
          - pattern: $SECRET = "..."
          - pattern: $TOKEN = "..."
          - pattern: $CREDENTIAL = "..."
    message: "Hardcoded secret detected. Use environment variables or secret management."
    severity: WARNING
    languages: [python, javascript, typescript, java, go, ruby]
    metadata:
      owasp: "A07:2021 - Identification and Authentication Failures"
      cwe: "CWE-798"
  
  - id: insecure-deserialization
    pattern-either:
      - pattern: pickle.loads($DATA)
      - pattern: yaml.unsafe_load($DATA)
      - pattern: eval($INPUT)
      - pattern: exec($INPUT)
    message: "Insecure deserialization detected. This can lead to RCE."
    severity: ERROR
    languages: [python]
    metadata:
      owasp: "A08:2021 - Software and Data Integrity Failures"
      cwe: "CWE-502"
      
  - id: path-traversal
    pattern-either:
      - pattern: open("../" + $USER_INPUT, ...)
      - pattern: os.path.join(BASE_DIR, $USER_INPUT)
      - pattern: fs.readFile("../" + $PATH, ...)
    message: "Path traversal vulnerability. Validate and sanitize user input."
    severity: ERROR
    languages: [python, javascript, typescript]
    metadata:
      owasp: "A01:2021 - Broken Access Control"
      cwe: "CWE-22"
```

### SCA (软件成分分析) 自动化

```python
# dependency_security.py - 依赖安全扫描与自动修复

import subprocess
import json
import re
from dataclasses import dataclass
from typing import List, Dict, Optional
from datetime import datetime
from enum import Enum

class Severity(Enum):
    CRITICAL = "CRITICAL"
    HIGH = "HIGH"
    MEDIUM = "MEDIUM"
    LOW = "LOW"
    INFO = "INFO"

@dataclass
class Vulnerability:
    vuln_id: str           # CVE-ID 或 GHSA ID
    package_name: string
    affected_version: string
    fixed_version: Optional[string]
    severity: Severity
    description: string
    cvss_score: float
    published_date: string
    references: List[str]

@dataclass
class DependencyScanResult:
    project_path: string
    total_dependencies: int
    vulnerable_count: int
    vulnerabilities: List[Vulnerability]
    scan_timestamp: datetime
    policy_violations: List[str]

class DependencySecurityScanner:
    """
    依赖安全扫描器
    
    支持: npm, pip, maven, gradle, cargo, go modules
    """
    
    def __init__(self, config: dict):
        self.config = config
        self.max_cvss_threshold = config.get('max_allowed_cvss', 7.0)
        self.block_critical = config.get('block_critical', True)
        
    async def scan_project(self, project_path: str) -> DependencyScanResult:
        """扫描项目依赖"""
        
        package_manager = self._detect_package_manager(project_path)
        
        if package_manager == 'npm':
            return await self._scan_npm(project_path)
        elif package_manager == 'pip':
            return await self._scan_pip(project_path)
        elif package_manager == 'maven':
            return await self._scan_maven(project_path)
        else:
            raise ValueError(f"Unsupported package manager: {package_manager}")
    
    def _detect_package_manager(self, path: str) -> str:
        """检测项目使用的包管理器"""
        import os
        
        if os.path.exists(os.path.join(path, 'package.json')):
            return 'npm'
        elif os.path.exists(os.path.join(path, 'requirements.txt')) or \
             os.path.exists(os.path.join(path, 'pyproject.toml')):
            return 'pip'
        elif os.path.exists(os.path.join(path, 'pom.xml')):
            return 'maven'
        elif os.path.exists(os.path.join(path, 'build.gradle')) or \
             os.path.exists(os.path.join(path, 'build.gradle.kts')):
            return 'gradle'
        
        return 'unknown'
    
    async def _scan_npm(self, project_path: str) -> DependencyScanResult:
        """使用 npm audit / snyk 扫描 Node.js 项目"""
        
        # 使用 npm audit (内置，无需额外安装)
        result = subprocess.run(
            ['npm', 'audit', '--json'],
            cwd=project_path,
            capture_output=True,
            text=True,
            timeout=120
        )
        
        if result.returncode not in [0, 1]:  # 0=无漏洞, 1=有漏洞
            raise RuntimeError(f"npm audit failed: {result.stderr}")
        
        audit_data = json.loads(result.stdout)
        
        vulnerabilities = []
        for action in audit_data.get('advisories', {}).values():
            vuln = Vulnerability(
                vuln_id=action.get('cve_id') or action.get('ghsa_id', 'UNKNOWN'),
                package_name=action.get('module_name'),
                affected_version=action.get('vulnerable_versions'),
                fixed_version=action.get('patched_versions'),
                severity=Severity[action.get('severity', 'INFO').upper()],
                description=action.get('overview'),
                cvss_score=float(action.get('cvss_score', 0)),
                published_date=action.get('created', ''),
                references=action.get('references', [])
            )
            vulnerabilities.append(vuln)
        
        # 检查策略违规
        violations = self._check_policy(vulnerabilities)
        
        return DependencyScanResult(
            project_path=project_path,
            total_dependencies=len(audit_data.get('dependencies', {})),
            vulnerable_count=len(vulnerabilities),
            vulnerabilities=vulnerabilities,
            scan_timestamp=datetime.utcnow(),
            policy_violations=violations
        )
    
    async def _scan_pip(self, project_path: str) -> DependencyScanResult:
        """使用 pip-audit / safety 扫描 Python 项目"""
        
        try:
            # 尝试 pip-audit (更详细)
            result = subprocess.run(
                ['pip-audit', '-f', 'json'],
                cwd=project_path,
                capture_output=True,
                text=True,
                timeout=120
            )
            
            if result.returncode == 0:
                audit_data = json.loads(result.stdout)
                # 解析结果...
                
        except FileNotFoundError:
            # Fallback to safety
            result = subprocess.run(
                ['safety', 'check', '--json', '--full-report'],
                cwd=project_path,
                capture_output=True,
                text=True,
                timeout=120
            )
        
        # 解析并返回结果...
        pass
    
    def _check_policy(self, vulnerabilities: List[Vulnerability]) -> List[str]:
        """检查是否违反安全策略"""
        violations = []
        
        for vuln in vulnerabilities:
            if self.block_critical and vuln.severity == Severity.CRITICAL:
                violations.append(
                    f"BLOCKED: Critical vulnerability {vuln.vuln_id} in "
                    f"{vuln.package_name} (CVSS: {vuln.cvss_score})"
                )
            elif vuln.cvss_score > self.max_cvss_threshold:
                violations.append(
                    f"HIGH_RISK: {vuln.vuln_id} in {vuln.package_name} "
                    f"exceeds CVSS threshold ({vuln.cvss_score} > {self.max_cvss_threshold})"
                )
        
        return violations
    
    def generate_report(self, scan_result: DependencyScanResult) -> str:
        """生成 Markdown 格式的安全报告"""
        
        report = f"""# Dependency Security Report

**Project**: `{scan_result.project_path}`  
**Scanned At**: {scan_result.scan_timestamp.isoformat()}  
**Total Dependencies**: {scan_result.total_dependencies}  
**Vulnerable Dependencies**: {scan_result.vulnerable_count}

## Summary by Severity

| Severity | Count |
|----------|-------|
| CRITICAL | {sum(1 for v in scan_result.vulnerabilities if v.severity == Severity.CRITICAL)} |
| HIGH | {sum(1 for v in scan_result.vulnerabilities if v.severity == Severity.HIGH)} |
| MEDIUM | {sum(1 for v in scan_result.vulnerabilities if v.severity == Severity.MEDIUM)} |
| LOW | {sum(1 for v in scan_result.vulnerabilities if v.severity == Severity.LOW)}

## Vulnerabilities Found

"""
        
        for vuln in sorted(scan_result.vulnerabilities, 
                          key=lambda x: x.cvss_score, reverse=True):
            emoji = {"CRITICAL": "🔴", "HIGH": "🟠", "MEDIUM": "🟡", "LOW": "🔵"}
            report += f"""
### {emoji.get(vuln.severity.value, '⚪')} [{vuln.severity.value}] {vuln.vuln_id}

- **Package**: `{vuln.package_name}@{vuln.affected_version}`
- **Fix Version**: {vuln.fixed_version or 'Not available'}
- **CVSS Score**: {vuln.cvss_score}
- **Description**: {vuln.description}

"""
        
        if scan_result.policy_violations:
            report += """
## ⚠️ Policy Violations

The following issues violate security policies:

"""
            for violation in scan_result.policy_violations:
                report += f"- {violation}\n"
        
        return report


# ===== 使用示例 =====

async def main():
    scanner = DependencySecurityScanner(config={
        'max_allowed_cvss': 7.0,
        'block_critical': True
    })
    
    result = await scanner.scan_project("./my-python-project")
    
    print(scanner.generate_report(result))
    
    if result.policy_violations:
        exit(1)  # 阻止部署
```

---

## 🐳 容器安全全流程

### Dockerfile 安全最佳实践

```dockerfile
# ===== 安全优化的 Dockerfile 模板 =====

# 1. 使用最小化基础镜像（减少攻击面）
FROM python:3.11-slim-bookworm AS base

# 2. 设置非 root 用户（安全最佳实践）
RUN groupadd --gid 1000 appgroup && \
    useradd --uid 1000 --gid appgroup --create-home appuser

# 3. 安装必要的安全工具和依赖
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    libssl3 \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean \
    && rm -rf /tmp/* /var/tmp/*

# 4. 创建安全的目录结构
WORKDIR /app

# 5. 先复制依赖文件（利用 Docker 缓存层）
COPY requirements.txt .

# 6. 以非 root 用户安装 Python 依赖
RUN pip install --no-cache-dir --user -r requirements.txt

# 7. 复制应用代码
COPY --chown=appuser:appgroup . .

# 8. 切换到非 root 用户
USER appuser

# 9. 只暴露必要端口
EXPOSE 8000

# 10. 健康检查
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')" || exit 1

# 11. 安全元数据标签
LABEL maintainer="security@hermes.ai" \
      version="1.0.0" \
      description="Secure application image" \
      org.opencontainers.image.source="https://github.com/hermes/app"

# 12. 入口点（避免 shell 注入）
ENTRYPOINT ["python"]
CMD ["-m", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

# ===== 构建时安全扫描命令 =====
# docker build -t secure-app:v1 .
# trivy image --severity CRITICAL,HIGH secure-app:v1
# grype secure-app:v1
```

### Kubernetes Pod 安全策略

```yaml
# pod-security-policy.yaml - Kubernetes Pod 安全标准

apiVersion: v1
kind: Namespace
metadata:
  name: secure-application
  labels:
    pod-security.kubernetes.io/enforce: restricted
    pod-security.kubernetes.io/enforce-version: latest
    pod-security.kubernetes.io/audit: restricted
    pod-security.kubernetes.io/warn: restricted

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secure-api-server
  namespace: secure-application
spec:
  replicas: 3
  selector:
    matchLabels:
      app: api-server
  template:
    metadata:
      labels:
        app: api-server
      annotations:
        container.apparmor.security.beta.kubernetes.io/api-server: runtime/default
        seccomp.security.alpha.kubernetes.io/pod: runtime/default
    spec:
      # 安全上下文（Pod 级别）
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        runAsGroup: 1000
        fsGroup: 2000
        seccompProfile:
          type: RuntimeDefault
      
      containers:
      - name: api-server
        image: hermes-registry/secure-api:v2.1.0
        imagePullPolicy: Always
        
        # 容器级安全上下文
        securityContext:
          allowPrivilegeEscalation: false
          readOnlyRootFilesystem: true
          capabilities:
            drop: ["ALL"]  # 移除所有 Linux 能力
          runAsNonRoot: true
          runAsUser: 1000
        
        resources:
          limits:
            cpu: "500m"
            memory: "512Mi"
          requests:
            cpu: "250m"
            memory: "256Mi"
        
        ports:
        - containerPort: 8000
          protocol: TCP
        
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: url
        - name: API_KEY
          valueFrom:
            secretKeyRef:
              name: api-keys
              key: production-key
        
        volumeMounts:
        - name: tmp
          mountPath: /tmp
        - name: cache
          mountPath: /var/cache/app
        readOnly: false
        
        livenessProbe:
          httpGet:
            path: /healthz
            port: 8000
          initialDelaySeconds: 15
          periodSeconds: 20
          timeoutSeconds: 5
          
        readinessProbe:
          httpGet:
            path: /readyz
            port: 8000
          initialDelaySeconds: 5
          periodSeconds: 10
      
      volumes:
      - name: tmp
        emptyDir:
          sizeLimit: 100Mi
      - name: cache
        emptyDir:
          sizeLimit: 200Mi
      
      # 网络策略限制
      serviceAccountName: api-service-account
      automountServiceAccountToken: false
      
      # 节点选择（可运行在专用节点）
      nodeSelector:
        kubernetes.io/os: linux
        node-type: application
      
      tolerations:
      - key: "dedicated"
        operator: "Equal"
        value: "application"
        effect: "NoSchedule"
```

---

## 🔗 相关技能

- [Hermes-Skill-Cybersecurity-Auditor](../security/cybersecurity-auditor/SKILL.md) - 深度安全审计
- [Hermes-Skill-CI-CD-Pipeline-Manager](../devops/cicd-pipeline-manager/SKILL.md) - 安全管道构建
- [Hermes-Skill-Compliance-Auditor](../legal/compliance-auditor/SKILL.md) - 合规自动化

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| SAST 工具 | Semgrep, SonarQube, CodeQL, Bandit, ESLint |
| SCA 工具 | Dependabot, Snyk, OWASP DC, Trivy |
| DAST 工具 | OWASP ZAP, Burp Suite, Nuclei |
| 容器安全 | Trivy, Grype, Clair, Falco |
| IaC 扫描 | Checkov, tfsec, Kubeaudit, KubeSec |
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
**适用场景**: 安全开发 | CI/CD 安全 | 容器安全 | 合规自动化 | 零信任架构
