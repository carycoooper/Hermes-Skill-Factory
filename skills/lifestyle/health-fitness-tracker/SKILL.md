---
name: health-fitness-tracker
description: Comprehensive health data aggregation from wearables, workout planning, nutrition tracking, sleep analysis, wellness insights, goal setting, and personalized fitness coaching with biometric trend analysis.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [health, fitness, wearables, workout, nutrition, wellness, sleep]
    category: lifestyle
    related_skills: [mission-control, finance-tracker, summarize]
    config:
      - key: fitness_goals
        description: "Primary fitness objectives"
        default: "[weight-loss, muscle-build, endurance, general-health]"
        prompt: "What are your main fitness goals?"
      - key: wearable_devices
        description: "Connected devices for data sync"
        default: "[apple-watch, fitbit, garmin, whoop]"
        prompt: "Which wearables do you use?"
---

# Health & Fitness Tracker Skill

Personal AI health coach that transforms raw biometric data into actionable wellness intelligence.

## When to Use

- **Daily Check-in**: "How's my health today?"
- **Workout Planning**: "Design a workout routine for me"
- **Nutrition Log**: "Log my meals and analyze nutrition"
- **Sleep Analysis**: "Am I getting enough quality sleep?"
- **Progress Review**: "Show my fitness trends this month"
- **Goal Setting**: "Help me set realistic health goals"

## Quick Reference

| Command | Description |
|---------|-------------|
| `/health dashboard` | Today's health overview |
| `/health workout [type]` | Generate workout plan |
| `/health log-meal [food]` | Track nutrition |
| `/health sleep-analysis` | Sleep quality report |
| `/health trends [period]` | Progress visualization |
| `/health coach [goal]` | Personalized advice |

## Procedure

### Phase 1: Data Aggregation (Continuous)
1. **Wearable Sync** (from connected devices)
   - Steps, floors climbed, distance
   - Heart rate (resting, active, zones)
   - Sleep (duration, stages, quality score)
   - Workouts (type, duration, calories, HR)
   - Weight, body fat % (if scale available)

2. **Manual Input** (for non-tracked data)
   - Meals (photos or descriptions)
   - Mood / energy levels (1-10 scale)
   - Symptoms / pain / illness
   - Water intake
   - Supplements / medications

3. **Environmental Factors**
   - Weather (affects outdoor activity plans)
   - Air quality (affects outdoor exercise decisions)
   - Stress indicators (workload, life events)

### Phase 2: Analysis Engine (Daily)
4. **Activity Analysis**
   ```
   Daily Activity Score Calculation:
   
   Base: Steps (goal: 10,000)
   Bonus: Exercise minutes (goal: 30 min moderate/vigorous)
   Bonus: Active hours (standing/moving vs sedentary)
   Penalty: Sedentary time (>8 hrs sitting)
   
   Grade: A (excellent) / B (good) / C (average) / D (below average) / F (sedentary)
   ```

5. **Nutrition Analysis**
   ```
   Per-Meal Breakdown:
   - Calories (vs TDEE target)
   - Macronutrients (protein/carbs/fat ratios)
   - Micronutrients (key vitamins/minerals)
   - Hydration status
   
   Daily Summary:
   - Total calories: [X] (Target: [Y], Gap: ±[Z])
   - Protein: [X]g (Target: [Y]g per kg bodyweight)
   - Fiber: [X]g (Target: 25-30g)
   - Sugar: [X]g (Target: <50g added)
   
   Meal Quality Scores:
   - 🟢 Nutrient-dense (vegetables, lean protein, whole grains)
   - 🟡 Balanced (mix of healthy + some processed)
   - 🔴 Processed-heavy (high sugar, sodium, saturated fat)
   ```

6. **Sleep Quality Assessment**
   ```
   Sleep Architecture Analysis:
   
   Duration: [X] hrs (Recommended: 7-9 hrs for adults)
   Efficiency: [X]% (Time asleep vs time in bed)
   
   Stages:
   - Deep Sleep: [X]% (Physical recovery, memory consolidation)
   - REM Sleep: [X]% (Cognitive restoration, dreaming)
   - Light Sleep: [X]% (Transition stages)
   - Awake: [X]% (Restlessness, disturbances)
   
   Sleep Score: [X]/100
   
   Factors Affecting Quality:
   - 😴 Bedtime consistency (±30 min variance)
   - 🍺 Alcohol within 3h of bedtime (disrupts REM)
   - ☕ Caffeine after 2pm (reduces deep sleep)
   - 📱 Screen time before bed (blue light, mental stimulation)
   - 🌡 Room temperature (ideal: 65-68°F / 18-20°C)
   - 🏃 Exercise timing (too close to bed = alertness)
   ```

### Phase 3: Personalized Recommendations (Actionable)

7. **Generate Wellness Action Items**
   
   ```markdown
   # 🏥 Your Personalized Health Briefing
   
   ## 📊 Today's Health Score: [X]/100 ⭐️⭐️⭐️⭐️⭐️
   
   ---
   
   ## ✅ What's Going Well
   
   - 💪 You've exercised [N] days in a row (streak active!)
   - 😴 Sleep efficiency improved by [X]% this week
   - 🥗 Protein intake meeting [X]% of daily target
   - 🚶 Stress levels below baseline (good!)
   
   ---
   
   ## ⚠️ Areas Needing Attention
   
   ### 🥤 Hydration Alert
   **Status:** Only [X] glasses today (Target: 8)
   **Impact:** Dehydration reduces cognitive function 15-20%, impairs exercise performance
   **Action:** Drink 2 more glasses before end of day
   **Tip:** Keep water bottle visible on desk; set hourly reminders
   
   ---   
   
   ### 🛌 Sleep Optimization Opportunity
   **Issue:** Average bedtime: [X]:[YY] AM (variance: ±[Z] min)
   **Problem:** Inconsistent schedule reducing sleep quality by [X]%
   **Recommendation:** 
   - Set fixed bedtime alarm (not just wake-up alarm)
   - Wind-down routine: No screens 60 min before bed
   - Bedroom temp: Currently [X]°C → Aim for 18-20°C
   
   **Expected Improvement:** +[X]% sleep score within 1 week
   
   ---
   
   ### 🏃 Workout Consistency Gap
   **Pattern:** You exercise intensely [X] days/week but skip [Y] days
   **Observation:** [Day(s)] consistently missed → [reason pattern]
   **Solution:** 
   Option A: Reduce intensity on busy days (20-min walk counts!)
   Option B: Morning workouts (before distractions accumulate)
   Option C: Activity snacking (3×10 min vs 1×30 min)
   
   **This Week's Goal:** Hit [N] active days (currently on track: yes/no)
   
   ---
   
   ## 🎯 Personalized Goals Progress
   
   ### Goal: [Primary Goal, e.g., "Lose 5kg"]
   **Started:** [date] | **Target:** [end-date]
   **Progress:**
   - Starting weight: [X]kg → Current: [Y]kg (Change: ±[Z]kg)
   - Pace needed: -[A]kg/week → Actual: -[B]kg/week
   - Forecast: Will reach goal by [date] ✅ / ⚠️ needs adjustment
   
   **Adjustment if off-track:**
   - Calorie deficit needed: [X] kcal/day (currently at [Y])
   - Exercise increase: Add [Z] min cardio 3×/week
   - Sleep priority: Get 7.5+ hrs/night (boosts metabolism)
   
   ---
   
   ## 📈 7-Day Trend Preview
   
   | Metric | Mon | Tue | Wed | Thu | Fri | Sat | Sun | Avg |
   |--------|-----|-----|-----|-----|-----|-----|-----|-----|
   | Steps | [..] | [..] | [..] | [..] | [..] | [..] | [..] | [Σ] |
   | Sleep | [..] | [..] | [..] | [..] | [..] | [..] | [..] | [avg] |
   | Mood | [..] | [..] | [..] | [..] | [..] | [..] | [..] | [avg] |
   | Workout| ✓/✗ | ✓/✗ | ✓/✗ | ✓/✗ | ✓/✗ | ✓/✗ | ✓/✗ | [N]/7 |
   
   ---
   
   ## 💡 Coach's Tip of the Day
   
   *[Rotating daily tips based on your current patterns]*
   
   *Today's insight based on your data:* [personalized observation]
   
   *Try this tomorrow:* [one specific, achievable action]
   ```

### Phase 4: Long-term Trend Analysis (Weekly/Monthly)

8. **Biometric Trend Reports**
   
   ```markdown
   # 📈 Monthly Health Analytics: [Month Year]
   
   ## Executive Health Scorecard
   
   | Dimension | This Month | Last Month | Change | Trend |
   |-----------|-----------|------------|--------|-------|
   | Overall Health | [X]/100 | [Y]/100 | ±[Z] | ↗️↘️→ |
   | Fitness Level | [score] | [score] | ±[%] | ... |
   | Nutrition Quality | [grade] | [grade] | ↑↓→ | ... |
   | Sleep Health | [score] | [score] | ±[pts] | ... |
   | Stress Management | [level] | [level] | better/worse | ... |
   | Consistency | [X]% | [Y]% | ±[%] | ... |
   
   ---
   
   ## 🏃 Fitness Progression
   
   **Cardiovascular Endurance:**
   - Resting heart rate: [X] bpm → [Y] bpm ([better/same/worse])
   - Recovery rate: Improved [X]% (HR drops faster post-exercise)
   - VO2max estimate: [X] mL/kg/min (fitness age: [Y] years)
   
   **Strength Indicators:**
   - Workout volume: [X] total sets/week (+[Z]% from last month)
   - Progressive overload: [Y]% of exercises increased weight/reps
   - Body composition trend: Muscle mass [↑/↓/→], Body fat [↑/↓/→]
   
   **Flexibility/Mobility:**
   - Range of motion improvements noted in: [areas]
   - Consistency: Stretched [N]/[M] days this month
   
   ---
   
   ## 🥗 Nutrition Patterns
   
   **Macro Averages (30-day):**
   - Calories: [X] avg/day (Target: [Y]) → Deficit/Surplus of [±Z]
   - Protein: [X]g avg (Target: [Y]g) → [meeting%]%
   - Carbs: [X]% of diet (Target: [Y]%) → [adjustment needed?]
   - Fat: [X]% of diet (Target: [Y]%) → [adjustment needed?]
   
   **Food Quality Distribution:**
   - 🟢 Whole/unprocessed: [X]%
   - 🟡 Minimally processed: [Y]%
   - 🔴 Ultra-processed: [Z]%
   
   **Hydration:** Avg [X] glasses/day (Target: 8) → [above/below/at]
   
   **Notable Patterns:**
   - [Pattern 1 discovered, e.g., "You tend to undereat on workout days"]
   - [Pattern 2, e.g., "Weekend calorie intake 30% higher than weekdays"]
   - [Improvement opportunity]
   
   ---
   
   ## 😴 Sleep Architecture Trends
   
   **Duration:** Avg [X] hrs/night (Target: 7-9) → [adequate/short/long]
   **Quality Score Trend:** [X] → [Y] → [Z] (improving/stable/declining)
   
   **Sleep Debt:** [X] hours accumulated this month (need to repay)
   
   **Chronotype Alignment:**
   - Your natural rhythm appears to be: [owl/lark/neither]
   - Current schedule alignment: [well/poorly/misaligned]
   - Recommendation: [shift earlier/later/maintain]
   
   **Sleep Disruptors Identified:**
   - Top disruptor: [factor, e.g., "Late-night screen time"] ([X]% of poor nights)
   - Second: [factor]
   - Mitigation priority: [what to fix first]
   ```

## Output Templates

### Daily Dashboard (Quick View)
```
☀️ Good Morning! Here's your health snapshot:

💓 Heart: Resting [X]bpm (Normal ✅)
👟 Steps: [X]/10,000 ([X]% of goal) [↗️↑↓→]
😴 Last Night: [X]hrs sleep, [X]% quality [Grade]
🥗 Yesterday: [X]cal ([X] protein, [X] carbs, [X] fat)
🏃 Workout: [Type] [X]min, [X]cal burned [✅ Strong / ⚠️ Light / ❌ Rest]
🧠 Mood: [X]/10 Energy: [X]/10 Stress: [Low/Med/High]

Overall Today: [Score]/100 [Emoji]
[One-sentence summary: "You're doing great!" / "Focus on X today"]
```

### Weekly Summary (Email/Report Format)
```
📊 Your Week in Health (# [Week of Year])

🏆 Highlights:
• Best workout: [day] - [achievement]
• Best sleep: [night] - [hours]hrs, [score]%
• Longest streak: [activity] for [N] days
• Goal progress: [X]% closer to [goal name]

📈 Numbers:
• Active days: [N]/7
• Avg steps: [X] (↑[Y]% from last week)
• Avg sleep: [X]hrs (quality: [Z]%)
• Total exercise: [X] minutes
• Calories burned: [X] (BMR: [Y], Activity: [Z])

🎯 Next Week Focus:
1. [Priority 1 based on gaps]
2. [Priority 2]
3. [Habit to maintain]

Keep it up! You're building momentum 💪
```

## Privacy & Ethics

**Data Sensitivity:**
- All health data is HIGHLY personal (PHI - Protected Health Information)
- Never share without explicit consent
- Anonymize before any analytics/aggregation
- Local processing preferred over cloud

**Safety Boundaries:**
- Not a medical device (no diagnoses)
- Cannot replace doctors/medical professionals
- Always recommend professional consultation for:
  - Persistent symptoms
  - Significant changes in biometrics
  - New exercise programs (especially with conditions)
  - Major dietary changes
  - Mental health concerns

**Responsible AI:**
- Frame suggestions as options, not prescriptions
- Acknowledge individual variation
- Encourage listening to body over rigid rules
- Promote sustainable habits, not quick fixes
- Celebrate non-scale victories (consistency, effort, mood)

## Integration Notes
**Works synergistically with:**
- **Mission Control**: Schedule workouts, meal prep reminders, sleep hygiene
- **Finance Tracker**: Budget for healthy food/gym/supplements ROI
- **Utility Toolkit**: Convert workout data between formats, calculate metrics
- **Self-Improving Agent**: Learn your health preferences and patterns

**Device Ecosystem Support:**
- Apple Health / HealthKit
- Google Fit / Samsung Health
- Fitbit / Garmin / Whoop
- Oura Ring / Withings
- Strava / MyFitnessPal integration

---

*Your personal AI wellness companion — because health is wealth*
