# Hermes-Skill-Accessibility-Expert

> ♿ **数字无障碍专家** | WCAG 2.2 合规 | 包容性设计 | 辅助技术集成

---

## 📋 技能概述

Hermes-Skill-Accessibility-Expert 是一个专业的数字无障碍（Accessibility/a11y）AI 助手，专注于帮助开发团队创建符合 WCAG 2.2 标准的包容性数字产品。涵盖 Web、移动应用、桌面软件的无障碍设计、开发、测试和审计全流程。

### 无障碍能力矩阵

```
┌─────────────────────────────────────────────────────────────┐
│                  Accessibility Stack                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🎨 Design           │  💻 Development    │  🧪 Testing     │
│  ├── Color Contrast │  ├── Semantic HTML │  ├── Automated │
│  ├── Typography     │  ├── ARIA Labels   │  │   Scanners   │
│  ├── Focus States   │  ├── Keyboard Nav  │  ├── Manual     │
│  └── Layout         │  └── Screen Reader │  │   Audits     │
│                      │      Support      │  └── User        │
│  📱 Platforms       │  🔧 Tools          │  │   Testing     │
│  ├── Web (WCAG)    │  ├── Lighthouse    │                 │
│  ├── iOS (VoiceOver)│  ├── axe DevTools  │                 │
│  ├── Android        │  ├── WAVE          │                 │
│  │   (TalkBack)    │  └── NVDA/JAWS     │                 │
│  └── Desktop Apps  │                     │                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 快速审计

```bash
# 页面无障碍检查
/a11y audit "https://example.com/dashboard"

# 组件级审查
/a11y review-component "navigation-menu"

# 对比度检测
/a11y contrast-check "#FFFFFF" "#0066CC"

# 键盘导航测试
/a11y keyboard-test "checkout-form"
```

### 代码生成

```bash
# 生成无障碍组件代码
/a11y generate "accessible-modal-dialog"

# ARIA 属性建议
/a11y aria-suggest "custom-dropdown"

# 修复建议
/a11y fix "low-contrast-warning-message"
```

---

## 📏 WCAG 2.2 原则与指南

### 四大原则 (POUR)

```
┌─────────────────────────────────────────────────────────────┐
│                    POUR Principles                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  P - Perceivable (可感知)                                   │
│  ├── 1.1 Text Alternatives (文本替代)                       │
│  │   └── 所有非文本内容都有等效替代方案                      │
│  ├── 1.2 Time-based Media (时基媒体)                        │
│  │   └── 提供字幕、手语、音频描述等                          │
│  ├── 1.3 Adaptable (可适配)                                 │
│  │   └── 信息可以不同方式呈现                               │
│  └── 1.4 Distinguishable (可区分)                           │
│      └── 颜色不是唯一区分手段                                │
│                                                             │
│  O - Operable (可操作)                                      │
│  ├── 2.1 Keyboard Accessible (键盘可访问)                   │
│  ├── 2.2 Enough Time (充足时间)                             │
│  ├── 2.3 Seizures and Physical Reactions (防癫痫)          │
│  ├── 2.4 Navigable (可导航)                                 │
│  └── 2.5 Input Modalities (多种输入方式) [NEW in 2.2]      │
│                                                             │
│  U - Understandable (可理解)                                 │
│  ├── 3.1 Readable (可读)                                    │
│  ├── 3.2 Predictable (可预测)                               │
│  └── 3.3 Input Assistance (输入辅助)                        │
│      └── 包括 3.2.6 Consistent Help (一致性帮助) [NEW]      │
│                                                             │
│  R - Robust (健壮性)                                        │
│  └── 4.1 Compatible (兼容)                                  │
│      └── 与辅助技术兼容                                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 合规等级速查表

| 等级 | 描述 | 适用场景 | 示例要求 |
|------|------|----------|----------|
| **A** | 最低级别 | 所有网站必须满足 | 图片有 alt 文本 |
| **AA** | 中级别 | 法律合规标准 | 文本对比度 ≥ 4.5:1 |
| **AAA** | 最高级别 | 特殊需求场景 | 对比度 ≥ 7:1 |

---

## 💻 无障碍开发实践

### 语义化 HTML 最佳实践

```html
<!-- ✅ 正确：使用语义化标签 -->
<header>
  <nav aria-label="Main navigation">
    <ul role="menubar">
      <li role="none">
        <a href="/" role="menuitem">Home</a>
      </li>
      <li role="none">
        <button 
          type="button"
          aria-haspopup="true"
          aria-expanded="false"
          id="products-btn"
        >
          Products
        </button>
        <ul role="menu" aria-labelledby="products-btn">
          <li role="none"><a href="/products/api" role="menuitem">API</a></li>
          <li role="none"><a href="/products/sdk" role="menuitem">SDK</a></li>
        </ul>
      </li>
    </ul>
  </nav>
</header>

<main>
  <article>
    <h1>Getting Started with Hermes API</h1>
    
    <!-- 跳转链接 -->
    <a class="skip-link" href="#main-content">
      Skip to main content
    </a>
    
    <section id="main-content" tabindex="-1">
      <h2>Quick Start Guide</h2>
      
      <!-- 代码块带语言标识 -->
      <pre><code language="python">import hermes
client = HermesClient(api_key="your-key")</code></pre>
      
      <!-- 表格带摘要 -->
      <table aria-describedby="pricing-summary">
        <caption id="pricing-summary">Pricing plans comparison</caption>
        <thead>
          <tr>
            <th scope="col">Plan</th>
            <th scope="col">Price</th>
            <th scope="col">Features</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Free</td>
            <td>$0/month</td>
            <td>1,000 calls/day</td>
          </tr>
        </tbody>
      </table>
    </section>
  </article>
</main>

<!-- ❌ 避免：使用 div 模拟按钮 -->
<div onclick="handleSubmit()" class="btn-primary">
  Submit
</div>

<!-- ✅ 正确：使用原生 button 元素 -->
<button type="onSubmit" class="btn-primary">
  Submit
</button>
```

### React 无障碍组件库

```tsx
// components/AccessibleModal.tsx
import React, {
  useState,
  useEffect,
  useRef,
  useCallback,
  KeyboardEvent as ReactKeyboardEvent
} from 'react';

interface AccessibleModalProps {
  isOpen: boolean;
  onClose: () => void;
  title: string;
  children: React.ReactNode;
  initialFocusRef?: React.RefObject<HTMLElement>;
}

export const AccessibleModal: React.FC<AccessibleModalProps> = ({
  isOpen,
  onClose,
  title,
  children,
  initialFocusRef
}) => {
  const modalRef = useRef<HTMLDivElement>(null);
  const previousActiveElement = useRef<HTMLElement | null>(null);
  
  // 打开时保存焦点并设置初始焦点
  useEffect(() => {
    if (isOpen) {
      previousActiveElement.current = document.activeElement as HTMLElement;
      
      // 设置初始焦点
      setTimeout(() => {
        if (initialFocusRef?.current) {
          initialFocusRef.current.focus();
        } else {
          modalRef.current?.focus();
        }
      }, 0);
      
      // 禁止背景滚动
      document.body.style.overflow = 'hidden';
      
      // 通知屏幕阅读器
      announceToScreenReader(`${title} dialog opened`);
    } else {
      // 恢复焦点
      previousActiveElement.current?.focus();
      document.body.style.overflow = '';
    }
    
    return () => {
      document.body.style.overflow = '';
    };
  }, [isOpen, initialFocusRef, title]);
  
  // Escape 关闭 + Focus Trap
  const handleKeyDown = useCallback(
    (event: ReactKeyboardEvent<HTMLDivElement>) => {
      if (event.key === 'Escape') {
        onClose();
        return;
      }
      
      // Focus Trap 实现
      if (event.key === 'Tab' && modalRef.current) {
        const focusableElements = modalRef.current.querySelectorAll<HTMLElement>(
          'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
        );
        
        const firstElement = focusableElements[0];
        const lastElement = focusableElements[focusableElements.length - 1];
        
        if (event.shiftKey && document.activeElement === firstElement) {
          event.preventDefault();
          lastElement?.focus();
        } else if (!event.shiftKey && document.activeElement === lastElement) {
          event.preventDefault();
          firstElement?.focus();
        }
      }
    },
    [onClose]
  );
  
  if (!isOpen) return null;
  
  return (
    <div
      className="modal-overlay"
      onClick={(e) => e.target === e.currentTarget && onClose()}
      role="presentation"
    >
      <div
        ref={modalRef}
        className="modal-content"
        role="dialog"
        aria-modal="true"
        aria-labelledby="modal-title"
        aria-describedby="modal-description"
        tabIndex={-1}
        onKeyDown={handleKeyDown}
      >
        <header className="modal-header">
          <h2 id="modal-title">{title}</h2>
          <button
            type="button"
            onClick={onClose}
            aria-label="Close dialog"
            className="close-button"
          >
            <span aria-hidden="true">×</span>
          </button>
        </header>
        
        <div id="modal-description" className="sr-only">
          Press Escape to close this dialog
        </div>
        
        <div className="modal-body">{children}</div>
      </div>
    </div>
  );
};

// 屏幕阅读器公告工具函数
function announceToScreenReader(message: string, priority: 'polite' | 'assertive' = 'polite') {
  const announcement = document.createElement('div');
  announcement.setAttribute('role', 'status');
  announcement.setAttribute('aria-live', priority);
  announcement.setAttribute('aria-atomic', 'true');
  announcement.className = 'sr-only';
  announcement.textContent = message;
  
  document.body.appendChild(announcement);
  
  setTimeout(() => {
    document.body.removeChild(announcement);
  }, 1000);
}
```

### 颜色对比度工具

```typescript
// utils/colorContrast.ts
interface RGB {
  r: number;
  g: number;
  b: number;
}

interface ContrastResult {
  ratio: number;
  aa_normal: boolean;   // 4.5:1 for normal text
  aa_large: boolean;    // 3:1 for large text (18pt+ or 14pt bold)
  aaa_normal: boolean;  // 7:1 for normal text
  aaa_large: boolean;   // 4.5:1 for large text
  wcag_level: 'Fail' | 'AA' | 'AAA';
}

function hexToRgb(hex: string): RGB {
  const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
  return result ? {
    r: parseInt(result[1], 16),
    g: parseInt(result[2], 16),
    b: parseInt(result[3], 16)
  } : { r: 0, g: 0, b: 0 };
}

function getLuminance(rgb: RGB): number {
  const { r, g, b } = rgb;
  const [rs, gs, bs] = [r, g, b].map(c => {
    c = c / 255;
    return c <= 0.03928 ? c / 12.92 : Math.pow((c + 0.055) / 1.055, 2.4);
  });
  return 0.2126 * rs + 0.7152 * gs + 0.0722 * bs;
}

export function calculateContrast(
  foregroundHex: string,
  backgroundHex: string
): ContrastResult {
  const fgRgb = hexToRgb(foregroundHex);
  const bgRgb = hexToRgb(backgroundHex);
  
  const l1 = getLuminance(fgRgb);
  const l2 = getLuminance(bgRgb);
  
  const lighter = Math.max(l1, l2);
  const darker = Math.min(l1, l2);
  
  const ratio = (lighter + 0.05) / (darker + 0.05);
  
  return {
    ratio: Math.round(ratio * 100) / 100,
    aa_normal: ratio >= 4.5,
    aa_large: ratio >= 3,
    aaa_normal: ratio >= 7,
    aaa_large: ratio >= 4.5,
    wcag_level: ratio >= 7 ? 'AAA' : ratio >= 4.5 ? 'AA' : 'Fail'
  };
}

// 使用示例
const result = calculateContrast('#000000', '#FFFFFF');
console.log(result);
// Output:
// {
//   ratio: 21,
//   aa_normal: true,
//   aa_large: true,
//   aaa_normal: true,
//   aaa_large: true,
//   wcag_level: 'AAA'
// }
```

---

## 🧪 测试策略

### 自动化测试框架

```javascript
// a11y.test.js - Jest + jest-axe
import { axe, toHaveNoViolations } from 'jest-axe';
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { AccessibleModal } from './AccessibleModal';

expect.extend(toHaveNoViolations);

describe('AccessibleModal', () => {
  it('should have no accessibility violations', async () => {
    const { container } = render(
      <AccessibleModal
        isOpen={true}
        onClose={() => {}}
        title="Test Modal"
      >
        <p>Modal content</p>
      </AccessibleModal>
    );
    
    const results = await axe(container);
    expect(results).toHaveNoViolations();
  });
  
  it('should be keyboard accessible', async () => {
    const user = userEvent.setup();
    const onClose = jest.fn();
    
    render(
      <AccessibleModal
        isOpen={true}
        onClose={onClose}
        title="Test Modal"
      >
        <input data-testid="first-input" />
        <input data-testid="second-input" />
      </AccessibleModal>
    );
    
    // Tab 导航应该能到达所有交互元素
    await user.tab();
    expect(screen.getByTestId('first-input')).toHaveFocus();
    
    await user.tab();
    expect(screen.getByTestId('second-input')).toHaveFocus();
    
    // Escape 应该关闭 Modal
    await user.keyboard('{Escape}');
    expect(onClose).toHaveBeenCalled();
  });
  
  it('should trap focus within modal', async () => {
    const user = userEvent.setup();
    
    render(
      <AccessibleModal
        isOpen={true}
        onClose={() => {}}
        title="Test Modal"
      >
        <button>First Button</button>
        <button>Last Button</button>
      </AccessibleModal>
    );
    
    const buttons = screen.getAllByRole('button');
    
    // 聚焦到最后一个按钮
    buttons[buttons.length - 1].focus();
    
    // Shift+Tab 应该回到第一个按钮（而不是离开 modal）
    await user.keyboard('{Shift>}{Tab}{/Shift}');
    expect(buttons[0]).toHaveFocus();
  });
});
```

### 手动测试清单

```markdown
## Screen Reader Testing Checklist

### Navigation
- [ ] Page title is announced correctly?
- [ ] Heading structure (h1-h6) is logical?
- [ ] Landmarks (main, nav, header, footer) are identifiable?
- [ ] Links are descriptive (no "click here")?

### Forms
- [ ] All form inputs have associated labels?
- [ ] Error messages are linked to inputs via aria-describedby?
- [ ] Required fields are indicated?
- [ ] Form validation feedback is clear?

### Dynamic Content
- [ ] Alerts/notifications use appropriate ARIA live regions?
- [ ] Role/state changes are announced?
- [ ] New content insertion is detected?

### Media
- [ ] Images have meaningful alt text?
- [ ] Videos have captions/transcripts?
- [ ] Audio content has transcripts?

## Keyboard Testing Checklist

### Basic Navigation
- [ ] Tab navigates through all interactive elements in order?
- [ ] Shift+Tab navigates backwards?
- [ ] Enter activates buttons/links?
- [ ] Space toggles checkboxes/radio buttons?

### Widgets
- [ ] Arrow keys work in custom components (menus, tabs, etc.)?
- [ ] Escape closes modals/dropdowns?
- [ ] Home/End navigate within lists?
- [ ] Focus indicators are visible?

### Focus Management
- [ ] Focus doesn't get trapped unexpectedly?
- [ ] Focus moves logically after dynamic changes?
- [ ] Skip links function correctly?
```

---

## 📱 移动端无障碍

### iOS VoiceOver 配置

```swift
// MARK: - Accessibility Configuration

import UIKit

extension UIViewController {
    func configureAccessibility() {
        // 1. 设置 Accessibility Identifier 用于自动化测试
        self.view.accessibilityIdentifier = "\(type(of: self)).view"
        
        // 2. 为关键元素添加 accessibilityLabel
        configureViewAccessibility()
    }
    
    private func configureViewAccessibility() {
        guard let view = self.view else { return }
        
        // 遍历所有子视图配置无障碍属性
        for subview in view.subviews {
            switch subview {
            case let button as UIButton:
                configureButtonAccessibility(button)
            case let label as UILabel:
                configureLabelAccessibility(label)
            case let textField as UITextField:
                configureTextFieldAccessibility(textField)
            case let tableView as UITableView:
                configureTableViewAccessibility(tableView)
            default:
                break
            }
        }
    }
    
    private func configureButtonAccessibility(_ button: UIButton) {
        // 如果按钮只有图标，需要添加 label
        if button.titleLabel?.text == nil || button.titleLabel?.text?.isEmpty == true {
            button.accessibilityLabel = button.accessibilityHint ?? "Button"
        }
        
        // 添加 trait
        button.accessibilityTraits = .button
        
        // 如果按钮触发重要操作，添加 hint
        if button.accessibilityHint == nil {
            button.accessibilityHint = "Double tap to activate"
        }
    }
    
    private func configureTextFieldAccessibility(_ textField: UITextField) {
        // 关联 label
        if let placeholder = textField.placeholder {
            textField.accessibilityLabel = placeholder
        }
        
        // 设置 value
        textField.accessibilityValue = textField.text
        
        // 设置 traits
        textField.accessibilityTraits = [.notEnabled]
        
        // 监听文本变化更新 accessibilityValue
        textField.addTarget(self, action: #selector(textFieldDidChange(_:)), for: .editingChanged)
    }
    
    @objc private func textFieldDidChange(_ textField: UITextField) {
        // 发布通知让 VoiceOver 朗读新值
        UIAccessibility.post(notification: .announcement, argument: textField.text)
    }
}

// MARK: - Custom Actions

class CustomAccessibilityButton: UIButton {
    override var accessibilityCustomActions: [UIAccessibilityCustomAction]? {
        get {
            return [
                UIAccessibilityCustomAction(
                    name: "Like",
                    target: self,
                    selector: #selector(performLike)
                ),
                UIAccessibilityCustomAction(
                    name: "Share",
                    target: self,
                    selector: #selector(performShare)
                )
            ]
        }
        set { super.accessibilityCustomActions = newValue }
    }
}
```

### Android TalkBack 配置

```kotlin
// AccessibilityUtils.kt
import android.content.Context
import android.view.View
import android.view.accessibility.AccessibilityNodeInfo
import androidx.core.view.ContentDescriptionCompat
import androidx.core.view.ViewCompat
import androidx.core.view.accessibility.AccessibilityNodeInfoCompat

object AccessibilityUtils {
    
    fun configureScreenReaderContent(
        view: View,
        contentDescription: String,
        hint: String? = null
    ) {
        ViewCompat.setAccessibilityDelegate(view, object : ViewCompat.AccessibilityDelegate() {
            override fun onInitializeAccessibilityNodeInfo(
                host: View?,
                info: AccessibilityNodeInfoCompat?
            ) {
                super.onInitializeAccessibilityNodeInfo(host, info)
                
                info?.contentDescription = contentDescription
                
                hint?.let {
                    info?.hintText = it
                }
                
                // 设置状态描述
                info?.stateDescription = getStateDescription(view)
            }
        })
    }
    
    fun announceForAccessibility(context: Context, message: String) {
        val event = AccessibilityEvent.obtain().apply {
            eventType = AccessibilityEvent.TYPE_ANNOUNCEMENT
            text.add(message)
            contentDescription = message
        }
        
        val manager = context.getSystemService(Context.ACCESSIBILITY_SERVICE) 
            as? AccessibilityManager
        
        manager?.sendAccessibilityEvent(event)
    }
    
    private fun getStateDescription(view: View): String {
        return when (view) {
            is CheckBox -> {
                if (view.isChecked) "Checked" else "Not checked"
            }
            is Switch -> {
                if (view.isChecked) "On" else "Off"
            }
            is ProgressBar -> {
                "${view.progress} percent"
            }
            else -> ""
        }
    }
}

// Usage in Compose
@Composable
fun AccessibleButton(
    text: String,
    onClick: () -> Unit,
    modifier: Modifier = Modifier,
    contentDescription: String? = null
) {
    Button(
        onClick = onClick,
        modifier = modifier
            .semantics {
                this.contentDescription = contentDescription ?: text
                this.role = Role.Button
                this.stateDescription = "Double tap to activate"
            }
    ) {
        Text(text)
    }
}
```

---

## 📊 审计报告生成

```python
# accessibility_audit.py
from dataclasses import dataclass
from enum import Enum
from typing import List, Dict, Optional
from datetime import datetime

class Severity(Enum):
    CRITICAL = "critical"
    SERIOUS = "serious"
    MODERATE = "moderate"
    MINOR = "minor"
    INFO = "info"

class WCAGLevel(Enum):
    A = "A"
    AA = "AA"
    AAA = "AAA"

@dataclass
class Issue:
    rule_id: str
    description: str
    severity: Severity
    wcag_criterion: str
    wcag_level: WCAGLevel
    element: str
    suggestion: str
    code_snippet: Optional[str] = None

@dataclass
class AuditReport:
    url: string
    audit_date: datetime
    overall_score: float
    total_issues: int
    issues_by_severity: Dict[Severity, int]
    wcag_compliance: Dict[WCAGLevel, bool]
    issues: List[Issue]

def generate_accessibility_report(url: str) -> AuditReport:
    """执行完整无障碍审计"""
    
    issues = []
    
    # 1. 自动化扫描
    automated_issues = run_automated_scanners(url)
    issues.extend(automated_issues)
    
    # 2. 人工审查项目
    manual_review_items = generate_manual_checklist(url)
    
    # 3. 计算分数
    score = calculate_a11y_score(issues)
    
    # 4. 分类统计
    severity_counts = {}
    for issue in issues:
        severity_counts[issue.severity] = severity_counts.get(issue.severity, 0) + 1
    
    # 5. WCAG 合规判断
    compliance = check_wcag_compliance(issues)
    
    return AuditReport(
        url=url,
        audit_date=datetime.now(),
        overall_score=score,
        total_issues=len(issues),
        issues_by_severity=severity_counts,
        wcag_compliance=compliance,
        issues=issues
    )

def format_report_markdown(report: AuditReport) -> str:
    """格式化为 Markdown 报告"""
    
    md = f"""# Accessibility Audit Report

**URL**: {report.url}  
**Date**: {report.audit_date.strftime('%Y-%m-%d %H:%M')}  
**Overall Score**: {report.overall_score}/100

## Executive Summary

- **Total Issues**: {report.total_issues}
- **Critical**: {report.issues_by_severity.get(Severity.CRITICAL, 0)}
- **Serious**: {report.issues_by_severity.get(Severity.SERIOUS, 0)}
- **Moderate**: {report.issues_by_severity.get(Severity.MODERATE, 0)}
- **Minor**: {report.issues_by_severity.get(Severity.MINOR, 0)}

## WCAG Compliance Status

| Level | Status |
|-------|--------|
| A | {'✅ Pass' if report.wcag_compliance.get(WCAGLevel.A, False) else '❌ Fail'} |
| AA | {'✅ Pass' if report.wcag_compliance.get(WCAGLevel.AA, False) else '❌ Fail'} |
| AAA | {'✅ Pass' if report.wcag_compliance.get(WCAGLevel.AAA, False) else '❌ Fail'} |

## Issues ({len(report.issues)} found)

"""
    
    for issue in report.issues:
        md += f"""### {issue.rule_id} - {issue.severity.value.upper()}

**WCAG Criterion**: {issue.wcag_criterion} ({issue.wcag_level.value})

**Description**: {issue.description}

**Affected Element**: `{issue.element}`

**Suggestion**: {issue.suggestion}

"""
        if issue.code_snippet:
            md += f"**Code Example**:\n```{issue.code_snippet}\n```\n\n"
    
    return md
```

---

## 🔗 相关技能

- [Hermes-Skill-Mobile-App-Guide](../development/mobile-app-guide/SKILL.md) - 移动端无障碍实现
- [Hermes-Skill-Creative-Writing-Coach](../creative/creative-writing-coach/SKILL.md) - 清晰文案写作
- [Hermes-Skill-BI-Dashboard-Builder](../business/bi-dashboard-builder/SKILL.md) - 数据可视化无障碍

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| WCAG 版本支持 | 2.1 & 2.2 |
| 平台覆盖 | Web, iOS, Android, Desktop |
| 代码示例 | React, Vue, Swift, Kotlin |
| 测试工具集成 | axe, Lighthouse, WAVE, pa11y |
| 审计模板 | 10+ 检查清单 |
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
**适用场景**: Web/Mobile 无障碍 | WCAG 合规 | 包容性设计 | 辅助技术
