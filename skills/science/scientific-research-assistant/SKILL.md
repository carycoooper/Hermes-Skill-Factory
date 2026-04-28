---
name: scientific-research-assistant
description: "Hermes-Skill-Scientific-Research-Assistant - 科研助手，支持文献检索与管理、实验设计、统计分析、学术写作(LaTeX)、同行评审准备和科研项目管理。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [scientific-research, academic-writing, latex, literature-review, statistics, experiment-design, paper-writing, citation-management]
    related_skills: [deep-research, knowledge-base-manager, summarize, prompt-engineering-assistant]
    requires_toolsets: [web, terminal]
    config:
      - key: research_field
        default: computer_science
      - key: citation_style
        default: ieee
---

# Hermes-Skill-Scientific-Research-Assistant (科研助手)

## 概述

**Hermes-Skill-Scientific-Research-Assistant** 是一个面向研究人员和学生的全栈科研辅助系统。从文献检索到实验设计，从数据分析到论文撰写，覆盖科研工作的每一个环节。

### 核心能力

- **文献管理**: Semantic Scholar / arXiv / PubMed 多源检索 + Zotero 集成
- **实验设计**: 对照实验 / A/B 测试 / 统计功效分析 / 样本量计算
- **数据分析**: 描述统计 / 推断统计 / 回归分析 / 可视化
- **学术写作**: LaTeX 模板 / Overleaf 集成 / 图表自动编号 / BibTeX 管理
- **同行评审**: 审稿意见回复 / Rebuttal Letter 撰写 / 伦理声明生成
- **科研规划**: 项目甘特图 / 里程碑跟踪 / 合作者协调

---

## 文献管理系统

### 多源检索

```bash
# 语义搜索
/research search "transformer architecture survey" --source semantic_scholar,arxiv,pubmed --limit 50 --year-range 2023-2026

# 高级筛选
/research filter --citations:>100 --venue:"NeurIPS,ICML,ACL" --type:survey

# 文献去重 (跨数据库)
/research deduplicate --by title,doi,authors --fuzzy-match 0.9

# 引用网络分析
/research citation-network "Attention Is All You Need" --depth 2 --visualize
```

### 文献笔记模板

```markdown
# 📄 Paper Notes Template

## 元信息
- **Title**: 
- **Authors**: 
- **Venue**: 
- **Year**: 
- **DOI**: 
- **PDF**: 
- **Citations**: 

## 核心贡献 (3 bullets)
1. 
2. 
3. 

## 方法论概要
(用自己的话重述核心方法，不超过200字)

## 关键发现
- Finding 1: 
- Finding 2: 
- Finding 3: 

## 局限性
(作者自己承认的 + 你发现的)

## 与我研究的关系
- ✅ 相关度: High / Medium / Low
- 💡 启发点: 
- 🔗 可以引用的场景: 

## 待解决问题 (Gap)
(这篇文章没有解决但值得研究的)

## 引用格式
BibTeX: 
APA: 
```

---

## 实验设计框架

### 统计功效分析

```bash
# 计算所需样本量
/research power-analysis \
  --effect-size medium \
  --alpha 0.05 \
  --power 0.80 \
  --design two-sample-t-test \
  --tails two-sided
  
# 输出示例:
# For medium effect size (d=0.5), α=0.05, power=0.80
# Required sample size per group: n = 64
# Total sample size needed: N = 128
```

### 实验类型决策树

```
你的研究问题是什么类型?
│
├─ "比较两个/多个组之间的差异?"
│   ├─ 两组独立? → Independent Samples t-test / Mann-Whitney U
│   ├─ 同一组前后测量? → Paired t-test / Wilcoxon signed-rank
│   └─ 三组及以上? → One-way ANOVA / Kruskal-Wallis
│
├─ "探索变量之间的关系?"
│   ├─ 两个连续变量? → Pearson / Spearman correlation
│   ├─ 一个因变量+多个自变量? → Multiple Regression
│   └─ 分类预测? → Logistic Regression / Random Forest
│
├─ "检验分布或频率差异?"
│   ├─ 分类变量关联? → Chi-square test / Fisher's exact
│   └─ 分布拟合? → Kolmogorov-Smirnov test
│
└─ "比较模型性能?"
    ├─ 单一指标? → t-test on metric scores
    └─ 多指标? → Wilcoxon signed-rank + Cliff's delta
```

### 实验报告模板

```markdown
## Experimental Setup

### Dataset
- Name & Source:
- Size: (train / val / test split)
- Class distribution:
- Preprocessing steps:

### Baselines
| Method | Year | Key Hyperparameters |
|--------|------|-------------------|
| Baseline 1 | ... | ... |
| Baseline 2 | ... | ... |
| Ours | - | ... |

### Implementation Details
- Framework: PyTorch / TensorFlow / JAX
- Hardware: GPU model(s), RAM, training time
- Training details:
  - Optimizer: Adam (lr=1e-4, β1=0.9, β2=0.999)
  - Batch size: 32
  - Epochs: 100 (with early stopping, patience=10)
  - Regularization: Dropout(0.3) + Weight Decay(1e-5)

### Evaluation Metrics
- Primary Metric: (justify choice)
- Secondary Metrics: 
- Statistical significance testing method:

## Results

### Main Results (Table)
| Method | Metric 1 ↑ | Metric 2 ↓ | Parameters |
|--------|-----------|-----------|------------|
| Baseline 1 | xx.x ± x.x | xx.x ± x.x | x.xM |
| Baseline 2 | xx.x ± x.x | xx.x ± x.x | x.xM |
| **Ours** | **xx.x ± x.x** | **xx.x ± x.x** | x.xM |
| Improvement | +x.x%* | -x.x%* | -x.x% |

*Bold indicates best result. * indicates statistical significance (p<0.05)*

### Ablation Study
| Component | Removed | Metric Δ | Analysis |
|-----------|----------|----------|----------|
| Module A | ✓ | -x.x% | Critical component |
| Module B | ✓ | -x.x% | Moderate impact |
| Module C | ✓ | -0.x% | Minor effect |

### Qualitative Analysis
(Case studies / Error analysis / Visualization)
```

---

## LaTeX 写作系统

### 论文结构模板

```latex
\documentclass[11pt]{article}
\usepackage{amsmath,amssymb}
\usepackage{graphicx}
\usepackage{booktabs}
\usepackage{hyperref}
\usepackage{algorithm}
\usepackage{algorithmic}

\title{Your Paper Title Here}
\author{Author Name\thanks{Corresponding author. Email: ...}}
\date{\today}

\begin{document}

\maketitle

\begin{abstract}
Your abstract here (150-250 words).
\textbf{Keywords}: keyword1, keyword2, keyword3
\end{abstract}

\section{Introduction}
\label{sec:introduction}
Background → Problem Statement → Our Contribution → Paper Organization

\section{Related Work}
\label{sec:related}
Organize by theme, not chronologically.

\section{Method}
\label{sec:method}
\begin{algorithm}[t]
\caption{Our Method}
\begin{algorithmic}[1]
\STATE Input: ...
\STATE Output: ...
\FOR{each $x$ in dataset $\mathcal{D}$}
    \STATE Process $x$
\ENDFOR
\RETURN result
\end{algorithmic}
\end{algorithm}

\section{Experiments}
\label{sec:experiments}
Datasets, baselines, implementation, results, ablation.

\section{Conclusion}
\label{sec:conclusion}
Summary + limitations + future work.

\bibliographystyle{IEEEtran}
\bibliography{references}

\end{document}
```

### 常用数学符号速查

```
集合: $\mathcal{X}$, $\mathbb{R}^n$, $\emptyset$, $\in$, $\subset$, $\cup$, $\cap$
向量: $\mathbf{x}$, $\vec{x}$, $\|\mathbf{x}\|_2$, $\mathbf{x}^\top$
概率: $\mathbb{P}(X)$, $\mathbb{E}[X]$, $\mathrm{Var}(X)$, $\mathcal{N}(\mu,\sigma^2)$
求导: $\frac{\partial f}{\partial x}$, $\nabla f$, $\frac{d^2f}{dx^2}$
矩阵: $\mathbf{I}_n$, $\mathbf{A}^{-1}$, $\mathbf{A}^\top$, $\mathrm{tr}(\mathbf{A})$, $\det(\mathbf{A})$
算法: $\mathcal{O}(n \log n)$, $\Omega(n)$, $\Theta(n)$, $\tilde{\mathcal{O}}(n)$
```

---

## 同行评审应对

### Rebuttal Letter 模板

```markdown
# Response to Reviewers

We sincerely thank all reviewers for their constructive comments, which have significantly improved our manuscript. Below we address each concern point-by-point.

---

## General Response to All Reviewers

[Summarize major changes made to the manuscript]

---

## Response to Reviewer XXXX

### Comment X.Y: [Quote reviewer's exact comment]

**Response**: [Polite acknowledgment]

**Action Taken**: 
- [✅] We have revised Section X.Y to address this concern.
- [Specific changes made, with line numbers if possible]
- [If applicable: Added new experiments/results shown below]

**Evidence**: [Quote from revised manuscript or show new table/figure]

---

### Comment X.Z: [Another comment]

**Response**: ...

**Action Taken**: ...

---

## Summary of Major Changes
1. Added [new content] (Section X.Y)
2. Revised [clarification] (Section Z.W)
3. Additional experiments: [Table/Figure X]
4. Updated references: [added N new citations]
```

---

*基于 LaTeX / Overleaf / Zotero / Semantic Scholar 最佳实践*
*版本: 2.0.0 | 最后更新: 2026-04-27*
