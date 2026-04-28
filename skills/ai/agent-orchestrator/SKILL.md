---
name: agent-orchestrator
description: "Hermes-Skill-Agent-Orchestrator - AI Agent编排器，支持多Agent协作、任务分配、工作流编排、工具调度、上下文共享和集体决策机制。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [agent-orchestration, multi-agent, workflow, task-planning, tool-use, context-sharing, collective-intelligence, swarm]
    related_skills: [self-improving-agent, mission-control, ai-model-comparator, prompt-engineering-assistant, knowledge-base-manager]
    requires_toolsets: [web, terminal]
    config:
      - key: orchestration_mode
        default: hierarchical
      - key: max_agents
        default: 8
---

# Hermes-Skill-Agent-Orchestrator (AI Agent 编排器)

## 概述

**Hermes-Skill-Agent-Orchestrator** 是一个多 AI Agent 协作编排系统。它能将复杂任务拆解为子任务，动态分配给专门的 Agent 协同执行，并通过上下文共享、结果汇总和质量控制确保最终输出的一致性和高质量。

### 核心能力

- **任务分解**: 复杂目标 → 可执行的原子任务 DAG（有向无环图）
- **Agent 注册中心**: 专业 Agent 发现、能力匹配、负载均衡
- **工作流引擎**: 串行/并行/条件分支/循环/异常恢复
- **上下文总线**: Agent 间的状态、中间结果、约束传递
- **集体决策**: 投票/辩论/专家仲裁多种共识机制
- **执行监控**: 实时进度追踪、超时处理、失败重试

---

## 架构总览

```
┌─────────────────────────────────────────────────────────────┐
│                     User Request                         │
│  "帮我做一个完整的市场调研报告"                            │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                   Orchestrator Core                       │
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │ Task Planner│→│ Dispatcher  │→│ Monitor     │        │
│  │ (分解任务)  │  (分配Agent)  │  (进度跟踪)  │        │
│  └─────────────┘  └──────┬──────┘  └──────┬──────┘        │
│                          │                │                 │
│  ┌───────────────────────▼───────────────────────┐       │
│  │              Context Bus (上下文总线)          │       │
│  │                                               │       │
│  │  ┌────────┐  ┌────────┐  ┌────────┐         │       │
│  │  │Shared  │  │Shared  │  │Shared  │         │       │
│  │  │State   │  │Results │  │Constraints│        │       │
│  │  └────────┘  └────────┘  └────────┘         │       │
│  └───────────────────────────────────────────────┘       │
│                          │                             │
│  ┌───────────────────────▼───────────────────────┐       │
│  │              Agent Pool (Agent池)              │       │
│  │                                               │       │
│  │  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐  │       │
│  │  │Research│ │Writer│ │Coder │ │Analyst│ │Reviewer│ │       │
│  │  │Agent  │ │Agent │ │Agent │ │Agent  │ │Agent  │ │       │
│  │  └──┬───┘ └──┬───┘ └──┬───┘ └──┬───┘ └──┬───┘  │       │
│  │     │        │        │        │        │        │       │
│  │  ┌──▼──┐  ┌──▼──┐  ┌──▼──┐  ┌──▼──┐  ┌──▼──┐  │       │
│  │  │Tools │  │Tools│  │Tools│  │Tools│  │Tools│  │       │
│  │  └──────┘  └──────┘  └──────┘  └──────┘  └──────┘  │       │
│  └───────────────────────────────────────────────┘       │
│                          │                             │
│  ┌───────────────────────▼───────────────────────┐       │
│  │              Consensus Engine (共识引擎)        │       │
│  │  Voting | Debate | Expert Review | Merger      │       │
│  └───────────────────────────────────────────────┘       │
│                          │                             │
│                          ▼                             │
│              Final Output (聚合结果)                  │
└─────────────────────────────────────────────────────┘
```

---

## 任务分解引擎

### 分解策略

```python
class TaskPlanner:
    """将高层目标拆解为可执行的任务DAG"""
    
    def decompose(self, goal: str) -> TaskGraph:
        """
        示例: "分析竞品X并给出差异化建议"
        
        Level 0 (Root): 
        └── "竞品分析与差异化建议"
        
        Level 1 (Major Phases):
        ├── Phase 1: 信息收集
        │   ├── 1.1 收集竞品基本信息 (官网/定价/功能列表)
        │   ├── 1.2 收集用户评价 (App Store / G2 / Twitter)
        │   └── 1.3 收集技术信息 (Tech Stack / Blog / Open Source)
        │
        ├── Phase 2: 分析评估
        │   ├── 2.1 功能对比矩阵 (Feature Comparison Matrix)
        │   ├── 2.2 SWOT 分析 (Strengths/Weaknesses/Opportunities/Threats)
        │   └── 2.3 定价策略分析 (Pricing Intelligence)
        │
        ├── Phase 3: 差异化建议
        │   ├── 3.1 定位声明生成 (Positioning Statement)
        │   ├── 3.2 功能差异化机会 (Feature Gaps)
        │   └── 3.3 Go-to-Market Strategy
        │
        └── Phase 4: 报告整合
            ├── 4.1 汇总所有分析结果
            ├── 4.2 生成可视化图表
            └── 4.3 撰写最终报告
        
        Dependencies (依赖关系):
        Phase 2 depends on Phase 1 (需要先收集数据才能分析)
        Phase 3 depends on Phase 2 (需要先分析才能建议)
        Phase 4 depends on Phase 1,2,3 (需要全部信息)
        
        Parallelism (并行可能):
        1.1 || 1.2 || 1.3 (可并行: 三个信息源独立)
        2.1 && 2.2 && 2.3 (可并行: 三个分析维度独立)
        3.1 || 3.2 || 3.3 (可并行: 三种建议维度独立)
        """
```

---

## Agent 注册与匹配

### Agent 能力模型

```yaml
agent_registry:
  research_agent:
    name: "Deep Research Agent"
    capabilities:
      - web_search
      - document_analysis
      - data_extraction
      - citation_verification
    tools:
      - web_browser
      - pdf_reader
      - search_api
    max_concurrent_tasks: 3
    cost_per_hour: 0.05  # API costs
    
  writer_agent:
    name: "Technical Writer Agent"
    capabilities:
      - technical_writing
      - markdown_formatting
      - diagram_generation
      - copy_editing
    tools:
      - text_editor
      - template_engine
    style: professional_technical
    
  coder_agent:
    name: "Code Expert Agent"
    capabilities:
      - code_generation
      - code_review
      - debugging
      - architecture_design
    languages: [python, javascript, typescript, go, rust]
    tools:
      - ide
      - interpreter
      - linter
      
  analyst_agent:
    name: "Data Analyst Agent"
    capabilities:
      - statistical_analysis
      - visualization
      - pattern_recognition
      - forecasting
    tools:
      - pandas
      - matplotlib
      - jupyter
      
  reviewer_agent:
    name: "Quality Assurance Agent"
    capabilities:
      - fact_checking
      - consistency_review
      - quality_scoring
      - bias_detection
    review_dimensions:
      - accuracy
      - completeness
      - clarity
      - actionability
```

### 匹配算法

```python
def match_agent(task: Task, available_agents: List[Agent]) -> Agent:
    """
    匹配最适合执行该任务的 Agent
    """
    scores = []
    
    for agent in available_agents:
        if agent.current_load >= agent.max_concurrent_tasks:
            continue  # 过载，跳过
            
        score = 0
        
        # 1. 能力匹配度 (40%)
        capability_overlap = len(set(task.required_capabilities) & set(agent.capabilities))
        score += (capability_overlap / len(task.required_capabilities)) * 40
        
        # 2. 历史表现 (25%)
        historical_success_rate = agent.get_success_rate(task.type)
        score += historical_success_rate * 25
        
        # 3. 当前可用性 (20%)
        availability = 1 - (agent.current_load / agent.max_concurrent_tasks)
        score += availability * 20
        
        # 4. 成本效率 (15%)
        cost_efficiency = 1 / (agent.cost_per_hour * task.estimated_hours)
        score += min(cost_efficiency * 15, 15)  # cap at 15
        
        scores.append((score, agent))
    
    return max(scores, key=lambda x: x[0])[1]
```

---

## 共识机制

### 场景 1: 多数投票 (Simple Majority Vote)

```
适用: 客观事实类问题 (日期/数字/存在性)

Question: "竞品 X 的月费是 $29 吗？"

Agent A (Researcher): ✅ Yes, confirmed from pricing page
Agent B (Researcher): ✅ Yes, also shows $290/year option  
Agent C (Researcher): ❌ No, I see $19 (maybe different plan)

Result: 2/3 → Yes ($29 is correct for Pro monthly plan)
Confidence: 67% (中等，建议人工确认)
```

### 场景 2: 专家加权 (Expert Weighted Consensus)

```
适用: 专业判断类问题 (架构选择/方案评估)

Question: "应该选择 GraphQL 还是 REST?"

Agent weights based on expertise:
├── API Expert (weight: 0.35): "GraphQL for complex queries, REST for simplicity"
├── Mobile Expert (weight: 0.25): "REST is better for mobile (mature tooling)"
├── Performance Expert (weight: 0.25): "GraphQL can reduce over-fetching by 60%"
└── Security Expert (weight: 0.15): "Both need proper auth; GraphQL has query complexity risks"

Aggregated Result:
├── GraphQL Score: 0.35×8 + 0.25×6 + 0.25×9 + 0.15×7 = 7.65
└── REST Score: 0.35×6 + 0.25×9 + 0.25×6 + 0.15×8 = 6.95

Recommendation: GraphQL (with caveat: consider mobile first approach)
```

### 场景 3: 辩论式 (Debate & Synthesis)

```
适用: 战略/创意类问题 (没有唯一正确答案)

Topic: "产品应该先做 Web 版还是 Mobile 版?"

Pro-Mobile Arguments:
├── Agent A: "Mobile-first captures the largest market opportunity"
├── Agent C: "Mobile usage patterns are more consistent (daily use)"
└── Agent E: "App store discovery is easier than SEO"

Pro-Web Arguments:
├── Agent B: "Web allows faster iteration and A/B testing"
├── Agent D: "Web has lower development cost (one codebase)"
└── Agent F: "SEO provides organic growth channel"

Synthesis (Orchestrator):
"The optimal strategy depends on your stage:
- Stage 1 (MVP): Web-first (speed + cost advantage)
- Stage 2 (Growth): Add Mobile (capture daily users)
- Stage 3 (Scale): Parity features on both platforms"

This is a 'both-and' synthesis rather than either-or.
```

---

## 异常处理与恢复

```yaml
error_handling:
  agent_timeout:
    duration: 300s (5 minutes per task)
    action: retry_with_different_agent
    max_retries: 2
    
  agent_failure:
    error_types:
      - api_error: retry with exponential backoff
      - parsing_error: reformat input, retry same agent
      - logic_error: escalate to human or try different approach
      - timeout: assign to faster/more capable agent
      
  consensus_failure:
    no_consensus_achieved:
      options:
        - escalate_to_human: "无法达成一致，请人工裁决"
        - present_all_options: "列出所有观点，让用户选择"
        - merge_conflicting: "保留分歧点，标记为需确认"
        
  partial_completion:
    when_some_agents_fail:
      action: complete with available results
      flag_incomplete_sections: true
      suggest_manual_followup: true
```

---

*基于 AutoGen / CrewAI / LangGraph Multi-Agent 最佳实践*
*版本: 2.0.0 | 最后更新: 2026-04-27*
