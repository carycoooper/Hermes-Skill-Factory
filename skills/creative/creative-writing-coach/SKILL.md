---
name: creative-writing-coach
description: "Hermes-Skill-Creative-Writing-Coach - 创意写作教练，提供故事结构设计、角色塑造、对话优化、文风调整、类型小说写作指南和出版准备。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [creative-writing, fiction, storytelling, character-development, dialogue, novel-writing, screenwriting, copywriting]
    related_skills: [summarize, prompt-engineering-assistant, content-calendar, content-pipeline]
    requires_toolsets: [web, terminal]
    config:
      - key: writing_genre
        default: general_fiction
      - key: target_audience
        default: adult
---

# Hermes-Skill-Creative-Writing-Coach (创意写作教练)

## 概述

**Hermes-Skill-Creative-Writing-Coach** 是一个全方位的创意写作辅助系统。无论你在写小说、剧本、短篇故事还是商业文案，它都能从构思到成稿的每个阶段提供专业指导。

### 核心能力

- **故事架构**: Hero's Journey / Save the Cat / Three-Act / Kishōtenketsu
- **角色系统**: 角色档案 / 关系图谱 / 弧光设计 / 对话声音
- **场景工程**: 场景卡片 / 节奏控制 / 感官描写 / Show Don't Tell
- **文风工具**: 风格检查 / 词汇丰富度 / 句式变化 / 基调一致性
- **类型指南**: 科幻 / 奇幻 / 悬疑 / 爱情 / 文学 / 商业
- **出版准备**: Query Letter / Synopsis / 提案撰写

---

## 故事架构系统

### The Hero's Journey (Campbell 12 Stages)

```
Act I: Departure (出发)
┌─────────────────────────────────────┐
│ 1. Ordinary World (平凡世界)          │ ← 展示主角日常，建立共情基础
│ 2. Call to Adventure (冒险召唤)       │ ← 打破现状的事件/机会
│ 3. Refusal of the Call (拒绝召唤)     │ ← 恐惧、怀疑、内在阻力
│ 4. Meeting the Mentor (遇见导师)     │ ← 获得知识/物品/鼓励
│ 5. Crossing the Threshold (跨越门槛)   │ ← 正式进入"特殊世界"
│                                     │
Act II: Initiation (试炼)              │
│ 6. Tests, Allies, Enemies (试炼)     │ ← 学习新世界的规则
│ 7. Approach to Inmost Cave (接近洞穴) │ ← 为最大挑战做准备
│ 8. Ordeal (磨难/至暗时刻)            │ ← 面对最大恐惧，濒临失败
│ 9. Reward (奖赏)                    │ ← 获得关键之物（宝剑/知识）
│                                     │
Act III: Return (归来)                 │
│ 10. The Road Back (回归之路)         │ ← 追逐开始，最终决战临近
│ 11. Resurrection (复活)              │ ← 最终考验，证明蜕变完成
│ 12. Return with Elixir (带着灵药归来) │ ← 回归平凡世界但已不同
└─────────────────────────────────────┘
```

### Save the Cat! Writes a Novel (15 Beats)

```markdown
## Beat Sheet Template

1. **Opening Image** (开篇画面)
   - 一句话描述主角出场状态
   - 奠定基调：观众应该感受到什么情绪？

2. **Theme Stated** (主题陈述)
   - 有人（不一定是主角）说出故事的核心主题
   - 例："贪婪终将毁灭你"

3. **Setup** (铺垫)
   - 展示主角的生活、缺陷、渴望
   - A故事（外部目标）和B故事（内部需求）建立

4. **Catalyst** (催化剂)
   - 打破平衡的事件（不可逆转）

5. **Debate** (辩论)
   - 主角犹豫：要不要接受挑战？
   - 列出3个不去的理由 → 然后逐一打破

6. **Break into Two** (第二幕开启)
   - 主角主动踏入新世界
   - 新旧世界形成鲜明对比

7. **B Story** (B故事线)
   - 通常是一个关系（爱情/友谊/师徒）
   - 这条线承载着主题的真正含义

8. **Fun and Games** (游戏时间)
   - 承诺前提的实现：观众买票想看的内容
   - 展示新世界的规则和乐趣

9. **Midpoint** (中点)
   - 伪胜利 或 伪失败（事件升级）
   - 主角从被动变主动（或反过来）

10. **Bad Guys Close In** (坏人逼近)
    - 内部压力 + 外部压力同时增大
    - 孤立无援的感觉

11. **All Is Lost** (一无所有)
    - 至暗时刻：看似彻底失败
    - 需要一个"死亡"（隐喻或真实）

12. **Dark Night of the Soul** (灵魂黑夜)
    - 绝望后的反思
    - 吸取 B 故事的教训

13. **Break into Three** (第三幕开启)
    - 解决方案出现（来自 A+B 故事的综合领悟）
    - 最后冲刺

14. **Finale** (结局)
    - 应用所学，面对终极挑战
    - 证明主角已经改变

15. **Final Image** (终章画面)
    - 与开篇画面呼应，但已截然不同
    - 视觉化展示蜕变
```

---

## 角色塑造引擎

### 完整角色档案

```yaml
character_profile:
  basics:
    name: "林晓雨"
    age: 28
    gender: 女
    occupation: "AI研究员"
    location: "上海"
    
  physical_appearance:
    height: "165cm"
    build: "纤细但有力量感"
    distinguishing_features: "左眉角有一道浅疤，总是戴着一副黑框眼镜"
    style: "极简风格，黑白灰为主色调"
    
  personality:
    mbti: "INTJ - 建筑师型"
    enneagram: "5号 - 研究者"
    strengths: ["深度分析能力", "独立思考", "在压力下保持冷静"]
    weaknesses: ["社交笨拙", "过度理性", "难以表达情感"]
    quirks: ["思考时习惯转笔", "咖啡必须加两块糖", "走路时数步子"]
    
  background:
    family: "父母都是教授，从小被期望优秀"
    education: "清华本科 → MIT博士"
    defining_moment: "22岁时，她的AI模型预测了一场灾难却无人相信，灾难真的发生了"
    secrets: "她至今保留着那场灾难的所有数据，每晚都会重新分析"
    
  internal_conflict:
    want: "创造一个真正理解人类情感的AI"
    need: "学会信任他人，接受自己的情感脆弱性"
    lie_she_believes: "情感是算法可以优化的变量"
    truth: "有些东西永远无法被量化"
    
  external_goals:
    main_goal: "发布具有情感理解能力的AI模型"
    obstacles: ["资金不足", "学术界的偏见", "竞争对手抄袭", "自己内心的恐惧"]
    
  relationships:
    protagonist: "故事主角"
    allies: 
      - name: "陈默"
        relationship: "实验室搭档 → 暗恋对象"
        dynamic: "他是唯一能让她笑的人，但她不敢承认"
    antagonists:
      - name: "Dr. Richard Vance"
        relationship: "前导师 / 学术对手"
        conflict: "他认为情感AI是伪科学，试图毁掉她的研究"
        
  character_arc:
    beginning: "封闭、理性、不相信任何人"
    midpoint: "被迫与他人合作，发现合作的力量"
    end: "敞开心扉，接受不完美，找到真正的连接"
    
  voice:
    speech_patterns: "句子简短精准，喜欢用技术术语比喻生活，极少使用感叹词"
    catchphrase: "让我们看看数据怎么说。"
    silence_meaning: "当她沉默时，她在进行最激烈的内心斗争"
```

---

## 场景工程

### 场景卡片模板

```markdown
## Scene Card: [场景名称]

**Chapter**: X | **POV**: [角色名] | **Time**: [时长约X分钟]

### 目的 (Purpose)
这个场景存在的唯一理由是什么？
- 推进情节: ...
- 展示角色: ...
- 建立世界观: ...

### 开始 (Entry)
- 人物在哪里？在做什么？
- 当前情绪状态：
- 第一句台词/动作：

### 冲突 (Conflict)
- 表面冲突:
- 深层冲突:
- 张力来源:

### 转折 (Turning Point)
- 本场景的关键变化时刻:
- 信息揭示 / 决策 / 发现:

### 结束 (Exit)
- 结束时人物的状态 vs 开始时:
- 悬念钩子 (Hook for next scene):
- 最后一句台词/动作:

### 感官清单 (Sensory Details)
- 👁️ Visual (视觉):
- 👂 Auditory (听觉):
- 👃 Olfactory (嗅觉):
- ✋ Tactile (触觉):
- 👅 Gustatory (味觉):

### Show Don't Tell 改写
❌ Tell: "她很紧张"
✅ Show: "她的手指在键盘上悬停了三秒，敲下又删除，删除又重写。"

❌ Tell: "房间很乱"
✅ Show: "论文堆到了天花板，三杯冷掉的咖啡在桌面上排成一列，像等待检阅的士兵。"

### 节奏标记
⚡ Fast-Paced (动作/冲突): ...
🐢 Slow-Paced (反思/情感): ...
```

---

## 文风诊断与优化

### 常见问题检测

```
问题 1: "Tell" vs "Show" 过多
检测模式:
├── 使用情感形容词: "他感到很伤心" → 应该用行为表现
├── 使用抽象描述: "气氛很紧张" → 应该用具体细节构建
└── 工具: 标记所有 "felt", "seemed", "was [adjective]" 

问题 2: 句式单调
检测模式:
├── 连续3句以上相同开头 (He/She/The)
├── 全是简单句 或 全是从属句
├── 平均句长 < 8 词 或 > 35 词
└── 修复: 变换长短句节奏 (短→冲击力, 长→氛围)

问题 3: 对话不自然
检测模式:
├── 太过完整/书面化的口语
├── 缺乏打断、犹豫、重复
├── 每个人说话方式一样
└── 修复: 给每个角色独特的语言指纹

问题 4: 副词滥用
检测模式:
├── "非常"、"很"、"极其" 出现频率 > 每500字3次
├── "-ly"副词过多 (quickly, slowly, angrily)
└── 修复: 用强动词替代 weak verb + adverb

问题 5: 被动语态过多
检测模式:
├── 被动句占比 > 20%
├── "被"字句连续出现
└── 修复: 改为主动语态，除非有意模糊施动者
```

---

*基于 Robert McKee / Blake Snyder / Lisa Cron 写作理论*
*版本: 2.0.0 | 最后更新: 2026-04-27*
