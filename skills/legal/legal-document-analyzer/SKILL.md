---
name: legal-document-analyzer
description: "Hermes-Skill-Legal-Document-Analyzer - 法律文件智能分析器，支持合同条款提取、风险评估、合规检查和法律文书起草辅助。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [legal, contract, compliance, risk-assessment, document-analysis, nlp, gdpr, privacy]
    related_skills: [summarize, deep-research, utility-toolkit]
    requires_toolsets: [web, terminal]
    config:
      - key: jurisdiction
        default: "CN"  # CN/US/EU/UK
      - key: risk_threshold
        default: medium
      - key: output_language
        default: zh-CN
---

# Hermes-Skill-Legal-Document-Analyzer (法律文件分析器)

## 概述

**Hermes-Skill-Legal-Document-Analyzer** 是一个专业的法律文档智能分析系统。它利用 AI 能力对各类法律文件进行深度分析，包括合同审查、条款提取、风险识别、合规检查和文书起草辅助，大幅提升法务工作效率。

### 核心能力

- **合同智能审查**: 自动识别关键条款和潜在风险点
- **条款结构化提取**: 将非结构化文本转为结构化数据
- **风险评估引擎**: 基于多维度的风险打分和分级
- **合规性检查**: GDPR / 劳动法 / 行业法规自动对照
- **文书起草助手**: 基于模板和上下文的法律文书生成
- **跨语言支持**: 中英文法律术语精准互译

---

## 适用场景

| 角色 | 使用场景 |
|------|---------|
| **企业法务** | 合同审查、供应商准入、IP 管理 |
| **律师** | 案件材料整理、判例检索、文书准备 |
| **HR** | 劳动合同审核、员工手册合规检查 |
| **创业者** | 投资协议解读、公司章程起草 |
| **个人** | 租房合同审查、消费维权材料准备 |

---

## 快速开始

```bash
# 分析合同文件
/legal-analyze review contract.pdf --type service_agreement --jurisdiction CN

# 条款提取
/legal-analyze extract terms --document nda.docx --output json

# 风险评估
/legal-analyze risk assess --contract supplier_agreement.pdf --threshold high

# 合规检查
/legal-analyze compliance check --document privacy_policy.html --framework gdpr

# 对比两份合同差异
/legal-analyze diff contract_v1.pdf contract_v2.pdf

# 起草法律文书
/legal-analyze draft --template nda --parties "[Company A]" "[Company B]" --scope "technical_collaboration"
```

---

## 支持的文档类型

### 商业合同类

| 类型 | 说明 | 关键关注点 |
|------|------|-----------|
| **NDA (保密协议)** | 信息保护约定 | 保密范围 / 期限 / 例外 / 违约责任 |
| **服务合同 (MSA)** | 服务交付条款 | SLA / 付款条件 / IP归属 / 终止权 |
| **采购合同** | 商品/服务购买 | 交货/验收 / 质量保证 / 违约赔偿 |
| **劳动合同** | 雇佣关系确立 | 薪酬福利 / 竞业限制 / 解雇条件 |
| **投资协议** | 股权融资条款 | 估值 / 董事席位 / 优先权 / 回购 |
| **许可协议** | IP授权使用 | 授权范围 / 使用费 / 侵权责任 |

### 其他法律文件

- 公司章程 (Articles of Association)
- 股东协议 (Shareholders Agreement)
- 租赁合同 (Lease Agreement)
- 软件许可协议 (EULA/TOS)
- 隐私政策 (Privacy Policy)
- 用户协议 (Terms of Service)

---

## 核心功能

### 1. 合同智能审查

```yaml
# 审查输出结构
review_result:
  document_info:
    type: Service Agreement
    parties: [甲方公司, 乙方服务商]
    effective_date: 2026-01-01
    term: 12 months
    
  clause_analysis:
    # 识别出的关键条款
    - section: "第三条 服务内容"
      type: scope_of_services
      summary: "乙方为甲方提供技术开发服务..."
      risk_level: low
      comments: "范围明确，无歧义"
      
    - section: "第五条 付款条款"
      type: payment_terms
      summary: "分期付款，首期30%..."
      risk_level: medium
      findings:
        - "付款节点与服务里程碑不完全对应"
        - "逾期付款违约金比例偏高(日0.1%)"
        
    - section: "第十条 知识产权"
      type: ip_rights
      risk_level: high
      findings:
        - "IP归属条款对甲方不利：开发成果全部归乙方"
        - 建议: 增加"委托开发作品著作权归甲方"条款
        
  overall_risk_score: 72/100  # 分数越高风险越大
  risk_summary: "中等风险，需重点修订IP和终止条款"
```

### 2. 条款结构化提取

```
提取字段清单:

基础信息:
  ├── 合同编号 / 签订日期 / 生效日期 / 到期日期
  ├── 当事人信息 (名称 / 地址 / 法定代表人)
  └── 合同标的 (金额 / 数量 / 规格)

权利义务:
  ├── 甲方权利 / 甲方义务
  ├── 乙方权利 / 乙方义务
  └── 共同义务

商务条款:
  ├── 价格 / 付款方式 / 付款节点 / 发票要求
  ├── 交付物 / 验收标准 / 验收期限
  ├── 服务级别 (SLA) / 响应时间 / 解决时限
  └── 保修期 / 售后服务

法律责任:
  ├── 违约情形 / 违约责任 / 赔偿限额
  ├── 不可抗力 / 争议解决 (诉讼/仲裁)
  ├── 保密义务 / 期限 / 范围
  └── 知识产权 / 归属 / 许可

其他:
  ├── 通知条款 / 送达地址
  ├── 完整性协议 / 修改方式
  ├── 适用法律 / 管辖法院
  └── 签章页 / 附件清单
```

### 3. 风险评估引擎

```
风险维度 (5级评分):

1️⃣ 财务风险 (Financial Risk)
   ├── 付款条件是否合理
   ├── 违约金比例是否过高
   ├── 价格调整机制是否存在
   └── 预外费用是否有上限

2️⃣ 法律风险 (Legal Risk)
   ├── 条款是否违反强制性法律规定
   ├── 管辖法院/仲裁地是否便利
   ├── 适用法律是否明确
   └── 争议解决机制是否公正

3️⃣ 运营风险 (Operational Risk)
   ├── SLA 是否可量化考核
   ├── 终止权是否对等
   ├── 过渡期安排是否充分
   └── 数据安全/隐私保护要求

4️⃣ 战略风险 (Strategic Risk)
   ├── IP 归属是否有利于己方
   ├── 竞业限制是否合理
   ├── 排他性条款是否受限
   └── 合作灵活性是否足够

5️⃣ 声誉风险 (Reputational Risk)
   ├── 品牌使用权限制
   ├── 联合宣传要求
   ├── 客户信息披露限制
   └── 公开声明审批流程

风险等级:
🟢 低风险 (0-30): 可直接签署
🟡 中等风险 (31-50): 建议小幅修改
🟠 较高风险 (51-70): 需要重要条款谈判
🔴 高风险 (71-85): 强烈建议法务介入
⛔ 极高风险 (86-100): 不建议签署
```

### 4. 合规检查

```yaml
# GDPR 合规检查示例
compliance_check:
  framework: gdpr
  document_type: privacy_policy
  
  checks:
    - article: Art. 13 (信息告知义务)
      status: PASS
      detail: "已明确说明数据收集目的和处理方式"
      
    - article: Art. 17 (被遗忘权)
      status: PARTIAL
      detail: "提及了删除请求，但未说明处理时限"
      recommendation: "补充'将在收到请求后30日内完成删除'"
      
    - article: Art. 20 (数据可携带权)
      status: FAIL
      detail: "完全未提及用户导出数据的权利"
      recommendation: "新增'数据导出'章节"
      
  compliance_score: 78%  # 合规程度百分比
  critical_gaps: 2
  recommendations: 5
```

---

## 文书起草辅助

### 模板系统

```
可用模板:
├── 商务合同
│   ├── NDA (单向/双向)
│   ├── 服务合同 (固定价格/T&M)
│   ├── 采购合同 (货物/服务)
│   ├── 经销商协议
│   └── 战略合作协议
├── 公司治理
│   ├── 公司章程
│   ├── 股东协议
│   ├── 董事任命书
│   └── 股权转让协议
├── 劳动人事
│   ├── 劳动合同 (全职/兼职/实习)
│   ├── 保密协议 (员工版)
│   ├── 竞业限制协议
│   └── 员工手册
└── 知识产权
    ├── 软件许可协议
    ├── 技术转让协议
    ├── 联合开发协议
    └── 开源贡献协议 (CLA)
```

### 智能填充

```bash
/legal-analyze draft \
  --template service_agreement \
  --fill \
  party_a.name="Acme Corp" \
  party_a.representative="张三" \
  party_b.name="Tech Solutions Ltd." \
  services.description="企业级CRM系统开发" \
  payment.total=500000 \
  payment.currency=CNY \
  payment.schedule="30%-40%-30%" \
  term.months=12 \
  ipOwnership=client \
  --output draft_contract.md
```

---

## 重要声明

> ⚠️ **免责声明**: 本工具提供的分析和建议仅供参考，不构成法律意见。
> 对于重大法律决策，请务必咨询持牌律师。

---

## 最佳实践

### ✅ 使用建议

1. **分步审查**: 先看整体风险评分，再深入具体条款
2. **交叉验证**: 关键条款人工复核 AI 提取结果
3. **版本管理**: 每次修改保存新版本，保留修改痕迹
4. **知识积累**: 将常见风险点和谈判话术沉淀为团队知识库

### ❌ 局限性

- 无法替代律师的专业判断
- 复杂法律关系可能遗漏
- 地方法规和行业特殊规定需额外确认
- 涉及金额巨大的合同必须人工审查

---

*基于 OpenClaw Legal Document Analyzer (12K+ 安装) 优化迁移*
*版本: 2.0.0 | 最后更新: 2026-04-27*
