---
name: learning-path-generator
description: "Hermes-Skill-Learning-Path-Generator - 学习路径生成器，支持技能图谱构建、个性化课程规划、学习资源推荐、进度追踪和自适应学习路径调整。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [learning-path, education, curriculum, skill-tree, personalized-learning, roadmap, career-development, upskilling]
    related_skills: [knowledge-base-manager, prompt-engineering-assistant, content-calendar, system-design-prep]
    requires_toolsets: [web, terminal]
    config:
      - key: learning_goal
        default: software_engineer
      - key: current_level
        default: beginner
      - key: time_commitment
        default: 10h_per_week
---

# Hermes-Skill-Learning-Path-Generator (学习路径生成器)

## 概述

**Hermes-Skill-Learning-Path-Generator** 是一个智能化的学习和职业发展规划系统。它根据用户的目标、当前水平和时间投入，自动生成个性化的学习路线图，推荐最优的学习资源和实践项目。

### 核心能力

- **技能图谱**: 可视化技能依赖关系和进阶路径
- **能力评估**: 多维度诊断当前水平 (知识/实践/软技能)
- **路径生成**: 基于目标的自适应学习计划
- **资源推荐**: 课程/书籍/项目/社区 精选推荐
- **进度追踪**: 学习时长/完成度/掌握度量化
- **路径优化**: 根据学习效果动态调整计划

---

## 技能图谱系统

### 全栈工程师技能树 (示例)

```
                          🎯 Full Stack Engineer
                                   │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
   Frontend          Backend           DevOps
   Engineer         Engineer          Engineer
        │                  │                  │
   ┌────┼────┐       ┌────┼────┐       ┌────┼────┐
   ▼    ▼    ▼       ▼    ▼    ▼       ▼    ▼    ▼
  HTML  CSS  JS   SQL  API  DB    CI/CD Cloud
        │         │    │    │    │       │    │
   React Vue Svelte Go Python Node Docker K8s
   Swift Flutter     Rust Java  Ruby AWS GCP
```

### AI/ML 工程师技能树

```
                    🤖 AI / ML Engineer
                              │
    ┌─────────────────┼─────────────────┬───────────────┐
    ▼                 ▼                 ▼               ▼
 Mathematics      ML Theory        Engineering      MLOps
    & Stats                           Frameworks
    │                 │                 │               │
 ├─ Linear Algebra ├─ Supervised      ├─ PyTorch      ├─ Feature Store
 ├─ Calculus        │   Learning      ├─ TensorFlow   ├─ Model Serving
 ├─ Probability    ├─ Unsupervised    ├─ JAX          ├─ A/B Testing
 ├─ Statistics     │   Learning      ├─ Scikit-learn ├─ Monitoring
 └─ Optimization  └─ RL / GenAI      └─ HuggingFace  └─ Pipeline
```

### 技能等级定义

```
Level 1: Awareness (认知) ⭐
  "听说过这个概念，知道它存在"
  → 阅读博客/教程概览
  
Level 2: Understanding (理解) ⭐⭐
  "明白基本原理和使用场景"
  → 完成在线课程 / 读一本入门书
  
Level 3: Application (应用) ⭐⭐⭐
  "能在指导下完成实际任务"
  → 跟着 Tutorial 做 / Code-along
  
Level 4: Proficiency (熟练) ⭐⭐⭐⭐
  "独立解决常见问题"
  → 完成个人项目 / 贡献开源
  
Level 5: Expert (精通) ⭐⭐⭐⭐⭐
  "能处理复杂场景并指导他人"
  → 工作中主导相关技术 / 写技术文章
  
Level 6: Mastery (大师) ⭐⭐⭐⭐⭐⭐
  "创造新方法/推动领域发展"
  ├── 发表论文 / 开创框架
  └── 行业公认的权威
```

---

## 能力评估引擎

### 多维度诊断问卷

```bash
/path-assess --goal fullstack-developer --format interactive
```

**评估维度 (100分制):**

### 维度 1: 编程基础 (25分)
```
Q1.1: 数据类型与结构 (数组/链表/哈希表) — 你能选择合适的数据结构吗？
Q1.2: 算法复杂度 (Big O) — 你能分析代码的时间/空间复杂度吗？
Q1.3: 面向对象设计 — 你能合理使用类/继承/接口吗？
Q1.4: 并发编程 — 你理解线程/异步/锁的概念吗？
Q1.5: 内存管理 — 你了解栈/堆/GC 的工作原理吗？

评分标准:
├── 0-8分: 初学者 — 需要从 CS101 开始
├── 9-16分: 中级 — 掌握基础但缺乏深度
├── 17-21分: 高级 — 扎实基础，可以进阶
└── 22-25分: 专家级 — 基础非常扎实
```

### 维度 2: 工程实践 (25分)
```
Q2.1: 版本控制 (Git) — 分支/合并/冲突解决
Q2.2: 测试 — 单元测试/集成测试/TDD
Q2.3: CI/CD — 自动化构建/部署流水线
Q2.4: 设计模式 — 至少熟悉 10+ 种常用模式
Q2.5: 代码质量 — Linting/Code Review/文档
```

### 维度 3: 系统设计 (25分)
```
Q3.1: 数据库设计 — Schema/索引/查询优化
Q3.2: API 设计 — RESTful/GraphQL/WebSocket
Q3.3: 缓存策略 — Redis/Memory Cache/CDN
Q3.4: 分布式基础 — CAP/一致性/消息队列
Q3.5: 安全性 — 认证/授权/OWASP Top 10
```

### 维度 4: 软技能与经验 (25分)
```
Q4.1: 项目经验 — 独立完成的完整项目数量
Q4.2: 问题解决能力 — Debugging / 故障排查
Q4.3: 沟通协作 — 技术写作/Code Review/团队协作
Q4.4: 学习能力 — 新技术上手速度
Q4.5: 英语能力 — 文档阅读/国际交流
```

---

## 路径生成算法

```python
def generate_learning_path(user_profile):
    """
    输入: 用户画像 (目标/当前水平/时间/偏好)
    输出: 个性化学习路径 (有序的模块 + 资源 + 时间线)
    """
    
    goal = user_profile.goal              # e.g., "fullstack-developer"
    current_level = user_profile.level    # e.g., {"frontend": 3, "backend": 1}
    time_budget = user_profile.hours_weekly  # e.g., 10 hours/week
    learning_style = user_profile.style     # e.g., "project-based"
    deadline = user_profile.target_date    # optional
    
    # Step 1: Gap Analysis (差距分析)
    required_skills = get_required_skills_for(goal)
    gaps = {}
    for skill in required_skills:
        current = current_level.get(skill.domain, 0)
        required = skill.required_level
        if current < required:
            gaps[skill] = {
                'current': current,
                'target': required,
                'delta': required - current,
                'estimated_hours': estimate_hours(current, required)
            }
    
    # Step 2: Dependency Resolution (依赖解析)
    # 某些技能有前置要求，需要先学
    ordered_skills = topological_sort(gaps.keys(), dependency_graph)
    
    # Step 3: Time Allocation (时间分配)
    total_available_hours = calculate_total_hours(
        start=date.today(),
        end=deadline or date.today() + timedelta(days=180),
        per_week=time_budget
    )
    
    for skill in ordered_skills:
        priority = calculate_priority(skill, gaps[skill], goal)
        allocated_hours = allocate_hours(
            total=total_available_hours,
            needed=gaps[skill]['estimated_hours'],
            priority=priority
        )
        skill['allocated_hours'] = allocated_hours
        skill['weeks_needed'] = ceil(allocated_hours / time_budget)
    
    # Step 4: Resource Matching (资源匹配)
    for skill in ordered_skills:
        level = gaps[skill]['current'] + 1  # next level to achieve
        resources = recommend_resources(
            skill=skill,
            level=level,
            style=learning_style,
            budget=gaps[skill]['allocated_hours']
        )
        skill['resources'] = resources
    
    # Step 5: Milestone Planning (里程碑规划)
    path = create_timeline(ordered_skills, time_budget)
    
    return LearningPath(
        user=user_profile,
        skills=ordered_skills,
        timeline=path,
        milestones=extract_milestones(path),
        estimated_completion=deadline or estimate_end_date(path)
    )
```

---

## 学习资源推荐引擎

### 资源质量评级

```
资源类型权重:

📚 书籍 (30%):
├── ⭐⭐⭐⭐⭐ 经典必读 (Clean Code / Designing Data-Intensive Apps)
├── ⭐⭐⭐⭐   强烈推荐 (最新版 / 高评分)
├── ⭐⭐⭐     值得一读 (特定方向深入)
└── ⭐⭐       参考备选

🎥 视频/课程 (25%):
├── Official Docs (官方文档优先)
├── University Courses (Stanford/MIT/Coursera)
├── High-Quality YouTube (频道订阅 > 100K)
└── Paid Platforms (Udemy/Pluralsight — 选高评分)

💻️ 动手实践 (30%):
├── Official Tutorials (语言/框架官方教程)
├── Build-from-Scratch Projects (从零实现)
├── Open Source Contribution (给真实项目提 PR)
├── Coding Challenges (LeetCode/HackerRank — 有针对性)
└── Clone & Modify (克隆优秀项目并改造)

🛠️ 工具/平台 (15%):
├── IDE Plugins (提升效率)
├── Sandboxes (安全实验环境)
├── Playground (快速原型验证)
└── Community (Discord/Slack/Reddit)
```

### 各阶段推荐示例

```
Phase 1: Web Development Fundamentals (Week 1-4)
┌─────────────────────────────────────────────┐
│ Goal: 能够独立构建静态网页 + 部署上线       │
│ Hours: ~40h (10h/week × 4 weeks)             │
├─────────────────────────────────────────────┤
│ Week 1-2: HTML + CSS Fundamentals            │
│ ├─ Resource: MDN Web Docs (Free) ★★★★★     │
│ ├─ Practice: FreeCodeCamp Responsive Design   │
│ ├─ Project: Clone your favorite landing page  │
│ └─ Checkpoint: Can you build a page from Figma?│
│                                             │
│ Week 3: JavaScript Basics                      │
│ ├─ Resource: JavaScript.info (Free) ★★★★★   │
│ ├─ Practice: JavaScript30 (Wes Bos) ★★★★     │
│ ├─ Project: Build a Todo App with local storage│
│ └─ Checkpoint: Can you manipulate DOM?          │
│                                             │
│ Week 4: Git + Deployment Basics                │
│ ├─ Resource: Pro Git (Scott Chacon) ★★★★★   │
│ ├─ Practice: GitHub Skills (Interactive)       │
│ ├─ Project: Deploy your app to Vercel/Netlify  │
│ └── ✅ MILESTONE: First deployed website live!   │
└─────────────────────────────────────────────┘
```

---

## 进度追踪仪表盘

```bash
# 查看学习进度
/learning progress --goal fullstack-developer

# 更新完成状态
/learning complete --module react-basics --level 3

# 调整计划 (基于实际进度)
/learning adapt --behind-schedule --suggest catch-up-plan

# 导出报告
/learning report --format pdf --include resources,time-log,certificate
```

**追踪指标:**
- ✅ 已完成模块数 / 总模块数
- 📊 每周学习时长 (目标达成率)
- 📈 技能等级变化 (前后对比)
- 🎯 里程碑完成率
- 💡 学习笔记/项目产出数量
- 🔥 连续学习天数 (Streak)

---

*基于 Bloom's Taxonomy / deliberate practice / spaced repetition 理论*
*版本: 2.0.0 | 最后更新: 2026-04-27*
