---
name: agile-project-manager
description: Complete Agile project management with sprint planning, backlog grooming, velocity tracking, retrospective automation, story point estimation, burndown charts, and team coordination for Scrum/Kanban methodologies.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [agile, scrum, kanban, project-management, sprint-planning, velocity]
    category: productivity
    related_skills: [mission-control, github-integration, summarize]
    config:
      - key: methodology
        description: "Agile methodology preference"
        default: "scrum"
        prompt: "Scrum or Kanban or hybrid?"
      - key: sprint_length
        description: "Default sprint duration"
        default: "2-weeks"
        prompt: "Sprint length (1-week, 2-weeks, 3-weeks)?"
---

# Agile Project Manager Skill

Intelligent Agile workflow orchestrator that makes Scrum/Kanban effortless while maximizing team velocity.

## When to Use

- **Sprint Planning**: "Plan our next sprint"
- **Backlog Grooming": "Prioritize and estimate these stories"
- **Daily Standup**: "Generate standup summary"
- **Retrospective**: "Facilitate sprint retro"
- **Velocity Analysis**: "Are we on track?"
- **Story Estimation**: "Estimate complexity of this feature"

## Quick Reference

| Command | Description |
|---------|-------------|
| `/agile plan-sprint` | Sprint planning wizard |
| `/agile groom-backlog` | Prioritize and refine stories |
| `/agile standup` | Daily status summary |
| `/agile retro` | Retrospective facilitation |
| `/agile velocity` | Velocity trends & forecasts |
| `/agile board` | Kanban board view |

## Procedure

### Sprint Planning (2-hour session structure)
1. **Review Goal**: Product owner presents sprint goal
2. **Backlog Walkthrough**: PO explains each candidate story
3. **Estimation**: Team estimates (planning poker / t-shirt sizing)
4. **Capacity Planning**: Account for time-off, meetings, support
5. **Commitment**: Team commits to realistic scope
6. **Task Breakdown**: Stories → Tasks (technical steps)
7. **Risk Identification**: Dependencies, unknowns, blockers
8. **Output**: Sprint backlog + task board initialized

### Backlog Grooming (Ongoing)
- User story quality check (INVEST criteria acceptance criteria)
- Story splitting (epics → features → stories → tasks)
- Priority refinement (MoSCoW: Must/Should/Could/Won't)
- Dependency mapping (what blocks what?)
- Technical debt identification
- Spikes for research items (time-boxed investigation)

### Daily Standup (15-min max format)
```
For each team member:

👤 [Name]
✅ Yesterday I completed: [delivered item/s]
📋 Today I'm working on: [current focus]
⚠️ Blockers: [anything impeding progress?]

📊 Team Health:
- Total WIP: [N] stories in progress
- Blockers: [N] items blocked
- At Risk: [stories that might not complete]
- Capacity Remaining: [X] story points / [Y] days left
```

### Sprint Retrospective (1-hour structured)
```
Phase 1: Data Gathering (15 min)
- What metrics say (velocity, bugs introduced, scope changes)
- Sentiment check (team mood survey)
- What worked well (celebrate wins)

Phase 2: Insight Generation (20 min)
- Root cause analysis (5 Whys for problems)
- Pattern recognition (recurring issues)
- Wild ideas (blue sky improvements)

Phase 3: Action Items (20 min)
- Pick 1-3 improvements to implement NEXT sprint
- Assign owners and success metrics
- Define "done" for each action

Phase 4: Closure (5 min)
- Appreciation round (thank teammates)
- Retro of the retro (was this session useful?)
```

## Output Templates

### Sprint Plan Document
```markdown
# 🎯 Sprint [Number] Plan

**Dates:** [Start] – [End] ([N] days)  
**Goal:** [sprint goal statement]  
**Team Velocity:** [X] pts avg (capacity: [Y] pts)  

---

## Committed Stories ([N] stories, [total points] pts)

| ID | Story | Points | Owner | Status |
|----|-------|--------|-------|--------|
| US-[num] | [title] | [pts] | @who | Ready |

---

## Task Breakdown

### Story: [Title]
**Points:** [estimation]  
**Owner:** [@assignee]  

| Task | Estimate | Owner | Done? |
|------|----------|-------|-------|
| [task 1] | [hrs] | @who | ☐ |
| [task 2] | [hrs] | @who | ☐ |

---

## Risks & Dependencies

| Risk | Impact | Likelihood | Mitigation |
|------|--------|-----------|------------|
| [risk] | H/M/L | H/M/L | [plan] |

---

## Definition of Done ✅
- [ ] Code reviewed and approved
- [ ] Tests written and passing
- [ ] Deployed to staging
- - [ ] Product owner acceptance
- [ ] No known bugs
```

### Burndown Chart Data
```markdown
# 📉 Sprint [N] Burndown

**Ideal Line:** Linear from [total_pts] → 0  
**Actual Progress:**

| Day | Ideal | Actual | Variance |
|-----|-------|--------|----------|
| 1 | [x] | [y] | [+/-z] |
| 2 | [x] | [y] | [+/-z] |
... 
| 10 | 0 | [final] | [result] |

**Forecast:** On track / At risk / Behind (need [X] pts reduction)
```

### Velocity Report
```markdown
# 📈 Velocity Trends

## Last 6 Sprints

| Sprint | Committed | Completed | % Complete | Avg P/Day |
|--------|-----------|-----------|------------|----------|
| S-[N-5] | [X] | [Y] | [%] | [Z] |
| ... | ... | ... | ... | ... |
| S-[N] | [X] | [Y] | [%] | [Z] |

**Trend:** ↗️ Improving / → Stable / ↘️ Declining  
**Avg Velocity:** [X] pts/sprint (σ=[stddev])  
**Forecast:** Can commit ~[Y] pts next sprint (with 90% confidence)

**Recommendations:**
- [Based on velocity patterns]
```

## Integration Notes
**Works synergistically with:**
- **Mission Control**: Time-block sprint ceremonies
- **GitHub Integration**: Link stories to PRs/issues, track cycle time
- **Summarize**: Condense meeting notes, extract action items
- **Capability Evolver**: Learn team's estimation patterns over time

**Tool Integrations:**
- Jira / Linear / GitHub Projects (issue tracking)
- Notion / Confluence (documentation)
- Slack / Teams (standups, notifications)
- Miro / FigJam (retro boards, planning)

## Agile Metrics Dictionary

| Metric | Definition | Target | Warning |
|--------|------------|--------|---------|
| Velocity | Points completed per sprint | Stable (±15%) | Volatile (>30% variance) |
| Scope Creep | Points added mid-sprint | <10% of committed | >25% added |
| Carryover | Unfinished stories to next sprint | <20% of committed | >40% carried over |
| Bug Ratio | Bugs found in sprint / total points | <0.5 | >1.0 (quality debt) |
| Cycle Time | Story start → done (days) | Decreasing or stable | Increasing |
| WIP Limit | Max stories in progress simultaneously | Enforced | Exceeded frequently |

---

*Making Agile effortless with AI-powered project orchestration*
