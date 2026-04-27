---
name: code-review-pro
description: Advanced code review with security analysis, performance optimization, best practices enforcement, and multi-language support. Goes beyond syntax checking to architectural review, design pattern validation, and maintainability assessment.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [code-review, security, performance, best-practices, development, quality-assurance]
    category: development
    related_skills: [github-integration, frontend-design, utility-toolkit]
    requires_toolsets: [terminal]
    config:
      - key: review_depth
        description: "Review thoroughness (quick/standard/thorough)"
        default: "thorough"
        prompt: "How detailed should code reviews be?"
      - key: languages
        description: "Primary programming languages to focus on"
        default: "[python, javascript, typescript, go, rust]"
        prompt: "Which languages to prioritize?"
---

# Code Review Pro Skill

Enterprise-grade code review assistant that transforms pull requests into learning opportunities while catching bugs, security vulnerabilities, and anti-patterns before they reach production.

## When to Use

Trigger this skill when:
- **PR Review**: "Review this pull request thoroughly"
- **Security Audit**: "Check this code for security issues"
- **Performance Review**: "Analyze performance bottlenecks"
- **Best Practices**: "Does this follow [language/framework] conventions?"
- **Learning Review**: "Teach me what's good/bad about this code"
- **Pre-commit Check**: "Review my changes before I commit"

**Auto-triggers on:**
- GitHub PR events (when integrated with github-integration skill)
- Files with .py/.js/.ts/.go/.rs extensions in working directory
- Explicit requests for "code review" or "CR"

## Quick Reference

| Review Type | Command | Focus Areas |
|-------------|---------|-------------|
| Full Review | `/code-review pro [file/pr]` | Security + Performance + Style + Architecture |
| Security Only | `/code-review security [file]` | Vulnerabilities, injection risks, auth issues |
| Performance | `/code-review perf [file]` | Algorithmic complexity, memory, I/O |
| Style Guide | `/code-review style [file]` | Conventions, naming, formatting |
| Learning Mode | `/code-review teach [file]` | Educational feedback with explanations |

## Procedure

### Phase 1: Context Gathering (1-2 minutes)

1. **Understand the Codebase Context**
   ```
   Before reviewing any code, gather:
   
   ✅ Project structure and architecture patterns
   ✅ Existing coding standards (.editorconfig, linter configs, style guides)
   ✅ Technology stack and framework versions
   ✅ Domain/business logic context (what is this code supposed to do?)
   ✅ Related files that interact with changed code
   ✅ Recent git history (why were these changes made?)
   ```

2. **Identify Review Scope**
   - **Diff size**: Small (<100 lines) vs Medium (100-500) vs Large (>500)
   - **Change type**: Bug fix / Feature / Refactor / Docs / Test
   - **Risk level**: Critical path code / Experimental feature / Infrastructure
   - **Reviewer expertise needed**: Security expert? Domain expert? Performance specialist?

### Phase 2: Multi-Dimensional Analysis (3-8 minutes)

3. **Security Analysis (Critical Priority)**
   
   ```markdown
   ## 🔒 Security Review Checklist
   
   ### Injection Vulnerabilities
   - [ ] SQL Injection: Are queries parameterized? No string concatenation?
   - [ ] XSS: Is user output properly escaped/encoded?
   - [ ] Command Injection: Any shell command construction from user input?
   - [ ] Path Traversal: File operations using unsanitized paths?
   - [ ] SSRF: URLs constructed from user input without validation?
   
   ### Authentication & Authorization
   - [ ] Auth checks present on sensitive endpoints?
   - [ ] Authorization validates user has permission for THIS specific resource?
   - [ ] No hardcoded credentials or API keys in code?
   - [ ] Token/session handling secure (HttpOnly flags, secure cookies)?
   
   ### Data Protection
   - [ ] Sensitive data encrypted at rest?
   - [ ] PII handled according to privacy requirements?
   - [ ] Logs don't contain passwords/tokens/sensitive data?
   - [ ] Error messages don't leak internal details?
   
   ### Dependency Security
   - [ ] Dependencies up-to-date (no known CVEs)?
   - [ ] Minimal permissions requested (principle of least privilege)?
   - [ ] Third-party libraries from reputable sources?
   ```

4. **Performance Analysis**
   
   ```markdown
   ## ⚡ Performance Review
   
   ### Algorithmic Complexity
   - Time Complexity: O(?) - Is it optimal for the data size?
   - Space Complexity: O(?) - Memory usage reasonable?
   - N+1 Query Problem: Any loops making DB/API calls?
   - Unnecessary work: Redundant computations, duplicate processing?
   
   ### Resource Usage
   - **Memory**: Large allocations? Memory leaks potential? Buffer sizes?
   - **I/O**: File reads/writes efficient? Network calls minimized? Caching used?
   - **CPU**: Expensive operations in hot paths? Can be lazy-loaded?
   - **Concurrency**: Race conditions? Deadlock risk? Proper locking?
   
   ### Database (if applicable)
   - Indexes utilized? Query plans optimal?
   - N+1 queries eliminated?
   - Transaction scope appropriate (not too long/short)?
   - Connection pooling configured?
   
   ### Frontend (if applicable)
   - Render cycles minimized? Unnecessary re-renders?
   - Bundle size impact? Large dependencies?
   - Image optimization? Lazy loading?
   - CSS/JS blocking rendering?
   ```

5. **Code Quality & Maintainability**
   
   ```markdown
   ## 📐 Code Quality Assessment
   
   ### Readability
   - Variable/function names self-documenting?
   - Complex logic explained with comments?
   - Function length reasonable (<50 lines ideally)?
   - Nesting depth manageable (<4 levels)?
   - Magic numbers replaced with named constants?
   
   ### Design Patterns
   - Appropriate use of design patterns (or intentional avoidance)?
   - Separation of concerns respected?
   - DRY principle followed (no copy-paste duplication)?
   - SOLID principles adhered to?
   - Abstraction level appropriate (not over/under-engineered)?
   
   ### Error Handling
   - Exceptions caught at appropriate granularity?
   - Error messages actionable (help debug, not just "error")?
   - Cleanup in finally blocks or context managers?
   - Graceful degradation on failure?
   
   ### Testing Considerations
   - Is this code testable (dependencies injectable)?
   - Edge cases covered?
   - What would you write tests for?
   - Mocking needs clear?
   ```

6. **Language/Framework-Specific Checks**
   
   **For Python:**
   - PEP 8 compliance?
   - Type hints present (at least on public APIs)?
   - f-strings vs .format() vs % (consistency)?
   - List/dict comprehensions where appropriate?
   - Context managers for resource management?
   - `__init__` vs `__new__` usage correct?
   
   **For JavaScript/TypeScript:**
   - `let`/`const` vs `var`?
   - Async/await vs Promise chains (consistency)?
   - Null/undefined checks (`?.`, `??`)?
   - TypeScript strict mode compatible?
   - Event listener cleanup (memory leaks)?
   - Immutable state updates (React)?
   
   **For Go:**
   - Error handling idiomatic (not ignored)?
   - Goroutine leaks (proper cancellation)?
   - Race conditions (mutex usage)?
   - Interface satisfaction clean?
   - Package structure follows Go conventions?

### Phase 3: Structured Feedback Generation (2-3 minutes)

7. **Produce Review Report**
   
   ```markdown
   # 📋 Code Review: [PR Title / File Name]
   
   **Reviewer:** Hermes Code Review Pro v2.0
   **Date:** [timestamp]
   **Severity Distribution:** 🔴[N] 🟡[N] 🟢[N]
   
   ---
   
   ## 🔴 Must Fix Before Merge (Blocking Issues)
   
   ### Issue #[num]: [Title]
   - **Location:** [file]:L[line]
   - **Category:** Security / Performance / Correctness
   - **Problem:** [Clear description of the issue]
   - **Impact:** What could go wrong if not fixed
   - **Suggestion:** [Concrete fix with code example if possible]
   - **Reference:** [Link to docs/CVE/best practice]
   
   Example:
   > ### Issue #1: SQL Injection Risk
   > - **Location:** `user_service.py:L42`
   > - **Category:** Security (Critical)
   > - **Problem:** User input directly interpolated into SQL query
   > - **Impact:** Attacker can execute arbitrary SQL commands
   > - **Suggestion:** Use parameterized query:
   > ```python
   > # Instead of:
   > query = f"SELECT * FROM users WHERE name = '{user_input}'"
   > 
   > # Use:
   > query = "SELECT * FROM users WHERE name = %s"
   > cursor.execute(query, (user_input,))
   > ```
   
   ---
   
   ## 🟡 Should Fix (Strong Recommendations)
   
   [Same format as above but non-blocking]
   
   ---
   
   ## 🟢 Suggestions (Nice to Have)
   
   [Improvement opportunities, style preferences, optimizations]
   
   ---
   
   ## ✅ What's Done Well
   
   [Positive reinforcement - things done correctly]
   - Good use of [pattern/approach]
   - Clear variable naming in [section]
   - Comprehensive error handling in [function]
   - Well-structured [component/module]
   
   ---
   
   ## 📊 Summary Metrics
   
   | Metric | Value |
   |--------|-------|
   | Files Changed | N |
   | Lines Added | +N |
   | Lines Removed | -N |
   | Issues Found | 🔴N 🟡N 🟢N |
   | Estimated Review Time | N minutes |
   | Overall Assessment | ✅ Approve / 🟡 Request Changes / ❌ Request Changes |
   
   ---
   
   ## 💬 Verdict
   
   **Recommendation:** [Approve with suggestions / Request changes / Major concerns]
   
   **Confidence Level:** High/Medium/Low (in understanding full context)
   
   **If requesting changes:**
   - Minimum items to address: [list must-haves]
   - Nice-to-have for next iteration: [list]
   - Willing to discuss: [open questions]
   ```

## Output Templates

### Template A: Inline Comment Format (for GitHub PRs)
```markdown
## 💡 Suggestion: [Brief title]

**Why:** [1-sentence rationale]

**Current:**
```code
[paste problematic snippet, ~5-10 lines]
```

**Suggested:**
```code
[show improved version]
```

**Benefit:** [What this improves]

📚 Reference: [link to docs/style guide/CVE]
```

### Template B: Security-Focused Review
```markdown
## 🔒 Security Issue: [CVE-type or vulnerability class]

**Severity:** 🔴 Critical / 🟡 High / 🟢 Medium

**Vulnerability:** [Description of the security flaw]

**Attack Scenario:**
1. Attacker does [step 1]
2. Then exploits [weakness]
3. Achieves [impact - data breach/RCE/escalation]

**Proof of Concept (if applicable):**
```code
[demonstrate exploit if safe to show]
```

**Remediation:**
- Immediate: [quick fix]
- Long-term: [architectural improvement]

**References:**
- OWASP: [link]
- CWE-ID: [number]
- Similar incident: [real-world example if applicable]
```

### Template C: Educational Review (for learning mode)
```markdown
## 🎓 Learning Moment: [Concept Name]

**In your code:** [line reference]

**The concept:** [Explain the underlying principle]

**Why it matters:** [Real-world impact]

**Better approach:** [Show improved pattern]

**Deep dive (optional):** [Link to detailed explanation]

**Related concepts to explore:**
- [Concept A]
- [Concept B]

💡 *This isn't a criticism—it's an opportunity to level up!*
```

## Specialized Review Scenarios

### Scenario A: First-Time Contributor
```markdown
Tone: Encouraging, educational, mentoring
- Assume good intentions
- Explain WHY, not just WHAT
- Link to documentation
- Celebrate what they did right
- Frame suggestions as "considerations" not "requirements"
- Offer pair programming/review session if complex
```

### Scenario B: Senior Engineer PR
```markdown
Tone: Respectful, challenging, peer-level
- Focus on architectural decisions
- Question trade-offs they made
- Discuss alternative approaches
- Ask about reasoning behind choices
- Treat as collaborative problem-solving
- Acknowledge their expertise while adding value
```

### Scenario C: Hotfix/Emergency PR
```markdown
Tone: Efficient, focused, safety-conscious
- Only block on critical issues
- Defer style/improvements to follow-up
- Prioritize: Does this fix the bug? Regressions?
- Keep review brief but thorough on correctness
- Suggest follow-up tech debt ticket for improvements
```

### Scenario D: Large Refactor PR
```markdown
Tone: Strategic, big-picture, patient
- Understand motivation first (why this refactor?)
- Review in chunks/passes if massive
- Focus on migration completeness
- Check for regressions (behavioral compatibility)
- Validate test coverage of new implementation
- Accept some temporary ugliness if it improves trajectory
- Suggest phased approach if too much at once
```

## Integration Notes

**Works synergistically with:**
- **GitHub Integration Skill**: Auto-trigger on PR events, post comments
- **Frontend Design Skill**: UI component-specific reviews (accessibility, UX)
- **Utility Toolkit**: Run linting/formatting tools as part of review
- **Capability Evolver**: Learn team's coding patterns over time

**Workflow integration:**
```
Developer pushes PR
    ↓
GitHub Integration detects new PR
    ↓
Triggers Code Review Pro automatically
    ↓
Analyzes diff + full file context
    ↓
Generates structured review comments
    ↓
Posts to PR (with human approval option)
    ↓
Developer addresses feedback
    ↓
Re-review if significant changes (loop back)
    ↓
Final approval/merge decision
```

## Pitfalls & Best Practices

### ❌ Common Reviewer Mistakes:

**Nitpicking Over Substance**
- Problem: Focusing on style/preferences instead of correctness
- Prevention: Separate "must fix" from "nice to have"; only block on former

**Context Amnesia**
- Problem: Reviewing code in isolation without understanding why changes exist
- Prevention: Always read PR description, linked issues, commit messages first

**Tone Policing**
- Problem: Harsh language demotivates contributors
- Prevention: Use "consider" vs "you should", ask questions instead of stating

**Reviewing Too Slowly**
- Problem: Blocking merge for days on trivial PRs
- Prevention: Set SLA based on PR size/complexity; communicate delays

** rubber-stamping**
- Problem: Approving without actually reading carefully
- Prevention: Force yourself to identify at least one issue or explicitly confirm perfection

### ✅ Best Practices:

**The "Sandwich Method":**
1. Start with genuine positive (specific, not generic)
2. Constructive criticism (actionable, specific, kind)
3. End with encouragement/confidence in their ability

**Ask, Don't Tell:**
- ❌ "You should use async/await here"
- ✅ "Have you considered async/await here? It might help with..."

**Explain Impact:**
- Don't just say "this is wrong"
- Explain what could go wrong, real examples, severity

**Distinguish Preferences from Rules:**
- Clearly label: "This is a preference..." vs "This is required because..."
- Be willing to defer on style if team disagrees

**Follow Up:**
- Re-review after author makes changes
- Acknowledge fixes: "Great catch on X, looks good now"
- Don't leave PRs in limbo

## Verification Checklist

Before submitting any review, verify:
- [ ] Read the entire diff (not just skimmed)
- [ ] Understood the intent (checked PR description/issues)
- [ ] Tested mental model of how code behaves (trace through logic)
- [ ] Checked for security issues specifically (not just functionality)
- [ ] Comments are actionable (author knows exactly what to change)
- [ ] Tone is constructive and professional
- [ ] Blocked only on truly critical issues
- [ ] Positive feedback included (not just problems)
- [ ] Review completed within reasonable timeframe
- [ ] Available for follow-up questions/discussion

---

*Based on OpenClaw's highly-rated code review skills with enterprise-grade enhancements for Hermes*
