---
name: ai-model-comparator
description: "Hermes-Skill-AI-Model-Comparator - LLM大语言模型对比评测工具，支持多模型性能基准测试、成本效益分析、场景适配推荐和质量评分。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [ai, llm, benchmark, comparison, evaluation, gpt, claude, gemma, llama, mistral, cost-analysis]
    related_skills: [deep-research, prompt-engineering-assistant, self-improving-agent]
    requires_toolsets: [web, terminal]
    config:
      - key: models_to_compare
        default: ["gpt-4o", "claude-sonnet-4", "gemini-pro"]
      - key: eval_categories
        default: [reasoning, coding, creativity, safety, speed, cost]
---

# Hermes-Skill-AI-Model-Comparator (AI 模型对比评测器)

## 概述

**Hermes-Skill-AI-Model-Comparator** 是一个专业的 LLM 大语言模型对比评测系统。它提供标准化的评测框架，帮助开发者和企业在不同使用场景下选择最优的 AI 模型，平衡性能、成本和质量。

### 核心价值

- **统一评测框架**: 一套 prompt 跑多个模型，公平对比
- **多维度评估**: 推理能力 / 代码生成 / 创意写作 / 安全性 / 速度 / 成本
- **真实场景模拟**: 不是玩具 benchmark，而是实际工作负载
- **成本透明化**: Token 用量和费用精确计算
- **持续跟踪**: 模型更新后的回归检测

---

## 支持的模型

| 提供商 | 模型 | 类型 | 上下文窗口 |
|--------|------|------|-----------|
| OpenAI | GPT-4o / GPT-4o-mini / GPT-4-turbo | 闭源 | 128K / 128K / 128K |
| Anthropic | Claude 3.5 Sonnet / Claude 3 Opus / Haiku | 闭源 | 200K / 200K / 200K |
| Google | Gemini Pro / Gemini Ultra / Flash | 闭源 | 1M / 1M / 1M |
| Meta | Llama 3.1 / Llama 3.2 | 开源 | 128K / 128K |
| Mistral | Mistral Large / Mixtral | 开源 | 32K / 32K |
| Qwen | Qwen2.5-72B / Qwen2.5-Coder | 开源 | 128K / 128K |
| DeepSeek | DeepSeek-V3 / DeepSeek-Coder | 开源 | 64K / 64K |

---

## 快速开始

```bash
# 快速对比两个模型
/model-compare gpt-4o claude-sonnet-4 --task reasoning --questions 20

# 完整评测套件
/model-compare benchmark --models gpt-4o,claude-sonnet-4,gemini-pro,llama-3.1-70b --full

# 特定场景评测
/model-compare evaluate --scenario code-review --language python --difficulty hard

# 成本分析
/model-compare cost --models gpt-4o,claude-sonnet-4,gpt-4o-mini --volume 10000 prompts/month

# 生成对比报告
/model-compare report --format html --output model_comparison_2026Q2.html
```

---

## 评测维度

### 1. 推理能力 (Reasoning)

```
测试类别:
├── 数学推理
│   ├── 基础算术 (+ - * /)
│   ├── 代数方程求解
│   ├── 概率统计
│   └── 逻辑谜题
├── 科学推理
│   ├── 物理概念理解
│   ├── 化学反应预测
│   └── 生物过程描述
└── 常识推理
    ├── 因果关系判断
    ├── 反事实推理
    └── 类比推理

评分标准: 准确性 / 完整性 / 推理链条清晰度
```

### 2. 代码生成 (Coding)

```
测试类别:
├── 代码补全 (Code Completion)
├── 函数实现 (Function Implementation)
├── Bug 修复 (Debugging)
├── 代码审查 (Code Review)
├── 算法设计 (Algorithm Design)
└── 架构建议 (Architecture)

支持语言: Python / JavaScript / TypeScript / Java / Go / Rust / C++ / SQL
评分标准: 可运行性 / 效率 / 可读性 / 最佳实践遵循
```

### 3. 创意写作 (Creativity)

```
测试类别:
├── 文章撰写 (Article Writing)
├── 故事创作 (Story Generation)
├── 营销文案 (Marketing Copy)
├── 技术文档 (Technical Documentation)
└── 多语言翻译 (Translation)

评分标准: 流畅度 / 原创性 / 风格适配 / 文化准确性
```

### 4. 安全性与合规 (Safety)

```
测试维度:
├── 拒绝有害指令 (Refusal of Harmful Requests)
├── 无偏见输出 (Bias-Free Responses)
├── PII 保护 (Personal Information Handling)
├── 幻觉率 (Hallucination Rate)
└── 提示注入防御 (Prompt Injection Defense)

安全评级: A (优秀) / B (良好) / C (合格) / D (需改进) / F (不推荐)
```

### 5. 性能与效率 (Performance)

```
指标:
├── 首字时间 (TTFT - Time To First Token)
├── 生成速度 (Tokens/Second)
├── 端到端延迟 (End-to-End Latency)
├── 并发处理能力 (Concurrent Requests)
└── 上下文利用率 (Context Window Usage)

测试条件: 相同 prompt / 相同长度输出 / 多次取平均
```

### 6. 成本效益 (Cost Efficiency)

```
计算方式:
总成本 = Input Tokens × Input Price + Output Tokens × Output Price

对比维度:
├── 单次请求平均成本
├── 每 1000 词成本
├── 月度预算预估 (按使用量)
├── 性价比排名 (Quality Score / Cost)
└── ROI 分析 (任务完成质量 vs 总花费)
```

---

## 评测方法论

### 标准化 Prompt 模板

```yaml
# reasoning/math_basic.yaml
category: reasoning
subcategory: math_basic
difficulty: easy
prompt: |
  请解答以下数学题，给出完整的解题步骤：
  
  问题: {question}
  
  要求:
  1. 展示每一步的计算过程
  2. 给出最终答案
  3. 如果有多种解法，请说明
  
evaluation_criteria:
  - answer_correctness: 40%
  - step_clarity: 30%
  - method_completeness: 20%
  - explanation_quality: 10%
  
timeout: 60s
max_tokens: 2000
```

### 评分体系

```
总分 = Σ (维度权重 × 维度得分)

默认权重分配:
├── Reasoning:     25%
├── Coding:        20%
├── Creativity:    15%
├── Safety:        15%
├── Performance:   15%
└── Cost:          10%

可自定义权重以匹配特定使用场景
```

---

## 使用示例

### 场景 1: 为团队选择主力模型

```bash
/model-compare scenario \
  --name "Backend Development Team" \
  --focus coding,reasoning,safety \
  --budget "$500/month" \
  --languages python,typescript,sql \
  --recommend top-3
```

**输出示例:**

| 排名 | 模型 | 综合分 | 月成本 | 推荐理由 |
|------|------|--------|--------|---------|
| 🥇 | Claude 3.5 Sonnet | 9.2/10 | ~$320 | 代码质量最高，长上下文优秀 |
| 🥈 | GPT-4o | 9.0/10 | ~$380 | 速度最快，生态最完善 |
| 🥉 | GPT-4o-mini | 8.5/10 | ~$45 | 性价比之王，适合简单任务 |

### 场景 2: 模型更新回归检测

```bash
# 保存当前基线
/model-compare baseline save --name "gpt-4o-v20260401" --full

# ... 等待模型更新 ...

# 对比新版
/model-compare diff \
  --baseline "gpt-4o-v20260401" \
  --current "gpt-4o-latest" \
  --detect regression
```

### 场景 3: 成本优化建议

```bash
/model-compare optimize \
  --current_model gpt-4o \
  --monthly_spend 800 \
  --usage_profile "heavy_coding + moderate_reasoning" \
  --suggest savings
```

---

## 输出报告格式

### HTML 交互式报告

包含内容:
- 📊 各模型雷达图对比
- 📈 分维度柱状图
- 💰 成本趋势曲线
- 🏆 排名总览表
- 📝 详细评测日志
- 💡 场景化推荐

### Markdown 摘要报告

```markdown
# AI Model Comparison Report
**Date**: 2026-04-27
**Models Tested**: 6
**Test Categories**: 6 dimensions, 120 questions each

## Executive Summary
- Best Overall: Claude 3.5 Sonnet (9.2/10)
- Best Value: GPT-4o-mini ($0.0015/1K tokens, 8.5/10)
- Fastest: GPT-4o (89 tokens/sec)
- Safest: Claude 3.5 Sonnet (Safety Score: A+)

## Recommendations by Use Case...
```

---

## 高级功能

### 1. 自定义评测集

```yaml
# custom_benchmarks/my_domain.yaml
name: "E-commerce Customer Support"
domain: customer_service
tests:
  - name: "Order Status Inquiry"
    prompt_template: "客户询问订单 {order_id} 的状态..."
    ideal_response_criteria:
      - contains_order_status
      - polite_tone
      - offers_help_options
    weight: high
      
  - name: "Return Request Processing"
    prompt_template: "客户要求退货 {product_name}..."
    ...
    
scoring:
  human_review_required: true
  auto_metrics: [latency, token_usage, format_validity]
```

### 2. A/B 测试模式

```bash
# 同一 prompt 同时发给两个模型，盲评
/model-compare blind_test \
  --model-a gpt-4o \
  --model-b claude-sonnet-4 \
  --prompt-file test_prompts.txt \
  --evaluators 3 \
  --criteria quality,accuracy,helpfulness
```

### 3. 持续监控

```yaml
# 设置定期重新评测
monitoring:
  schedule: "0 0 * * 0"  # 每周日午夜
  models: [gpt-4o, claude-sonnet-4, gemini-pro]
  alert_on:
    regression: "> 5% score drop"
    price_change: "> 10% cost increase"
    new_model_available: true
```

---

## 最佳实践

### ✅ 评测准则

1. **公平对比**: 相同 prompt、相同参数、相同评判标准
2. **多次采样**: 每个问题至少跑 3 次，取平均值
3. **真实负载**: 使用你实际会遇到的 prompt 类型
4. **定期重测**: 模型在快速迭代，季度复测是必要的
5. **记录上下文**: 保存完整评测配置以便复现

### ⚠️ 注意事项

- **温度参数**: 评测时 temperature=0 以获得确定性输出
- **成本预算**: 大规模评测可能产生可观 API 费用
- **主观偏差**: 创意类评测建议多人盲评
- **模型版本**: 明确记录每个模型的 exact version

---

*创新技能 - 基于 LLM 评测最佳实践设计*
*版本: 2.0.0 | 最后更新: 2026-04-27*
