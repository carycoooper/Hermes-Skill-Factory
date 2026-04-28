---
name: i18n-manager
description: "Hermes-Skill-I18N-Manager - 国际化(i18n)与本地化(l10n)管理器，支持多语言文本提取、翻译工作流、RTL布局、文化适配、复数规则和时区处理。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [i18n, l10n, internationalization, localization, translation, rtl, multilingual, culture, globalization]
    related_skills: [content-pipeline, api-doc-generator, frontend-design-assistant, content-calendar]
    requires_toolsets: [web, terminal]
    config:
      - key: source_language
        default: en-US
      - key: target_languages
        default: "zh-CN,ja,ko,de,fr,es,ar,pt-BR"
---

# Hermes-Skill-I18N-Manager (国际化与本地化管理器)

## 概述

**Hermes-Skill-I18N-Manager** 是一个全面的国际化和本地化工程系统。它帮助开发者将产品从单一语言扩展到全球市场，涵盖文本提取、翻译管理、文化适配、技术实现和持续维护的完整流程。

### 核心能力

- **文本提取**: 自动扫描代码中的硬编码字符串 → 提取到资源文件
- **翻译工作流**: 翻译记忆库 / 术语表 / 上下文感知翻译
- **文化适配**: 日期/数字/货币/地址/姓名格式本地化
- **RTL 支持**: 阿拉伯语/希伯来语/波斯语从右到左布局
- **复数与性别**: ICU MessageFormat 复杂语法支持
- **质量保证**: 翻译完整性检查 / 截图对比 / 假数据检测

---

## i18n 架构设计

### 推荐架构

```
┌─────────────────────────────────────────────┐
│              Source Code                    │
│  t('key.name') / i18n.t('key') / <Trans>   │
└──────────────────┬──────────────────────────┘
                   │ extract
                   ▼
┌─────────────────────────────────────────────┐
│           Locale Files (JSON/YAML)         │
│                                             │
│  en-US.json │ zh-CN.json │ ja.json │ ...     │
│  {                                          │
│    "common.save": "Save",                   │
│    "common.cancel": "Cancel",               │
│    "greeting": "Hello, {name}!",            │
│    "items_count": "{count, plural,          │
│      one {# item} other {# items}}"          │
│  }                                          │
└──────────────────┬──────────────────────────┘
                   │ compile
                   ▼
┌─────────────────────────────────────────────┐
│            Runtime Bundle                     │
│  (en-US + zh-CN merged, tree-shaken)       │
└──────────────────┬──────────────────────────┘
                   │ render
                   ▼
┌─────────────────────────────────────────────┐
│              User Interface                  │
│  🇺🇸 Hello, World! | 🇨🇳 你好，世界！        │
│  $1,234.56        | ¥8,500.00                │
│  04/27/2026       | 2026年4月27日             │
└─────────────────────────────────────────────┘
```

### 文件组织结构

```
src/
├── i18n/
│   ├── locales/
│   │   ├── en-US/           # 英语 (美国)
│   │   │   ├── common.json
│   │   │   ├── home.json
│   │   │   └── errors.json
│   │   │
│   │   ├── zh-CN/           # 简体中文
│   │   │   ├── common.json
│   │   │   ├── home.json
│   │   │   └── errors.json
│   │   │
│   │   ├── ja/              # 日本語
│   │   ├── ko/              # 한국어
│   │   ├── ar/              # العربية (RTL)
│   │   └── de/              # Deutsch
│   │
│   ├── index.ts            # i18n 初始化配置
│   ├── types.ts            # 类型定义
│   └── utils.ts            # 工具函数 (日期格式化等)
│
├── components/
│   └── LanguageSwitcher.tsx  # 语言切换组件
│
└── config/
    └── supported-languages.ts  # 支持的语言列表
```

---

## 文本提取与翻译

### 自动提取规则

```bash
# 扫描代码中的硬编码字符串
/i18n extract --src ./src --output ./extracted_strings.json

# 检测遗漏的未提取文本
/i18n audit --check hardcoded-strings,missing-keys,inconsistent-plurals

# 对比两个语言的覆盖率
/i18n compare --source en-US --target zh-CN --show missing-only
```

### JSON 格式规范

```json
{
  "app_name": "Hermes Skills",
  
  "common": {
    "save": "Save",
    "cancel": "Cancel",
    "delete": "Delete",
    "confirm": "Confirm",
    "loading": "Loading...",
    "error_generic": "Something went wrong. Please try again.",
    "retry": "Retry"
  },
  
  "home": {
    "title": "Welcome to Hermes Skills",
    "subtitle": "Discover AI-powered agent skills",
    "cta_button": "Explore Skills",
    "stats": {
      "skills_available": "{count, plural, one {# skill available} other {# skills available}}",
      "users_trusted": "{number, plural, one {+1 user trusts us} other {+, number} users trust us}}"
    }
  },
  
  "auth": {
    "login_title": "Sign In",
    "login_subtitle": "Access your account to continue",
    "email_placeholder": "Enter your email",
    "password_placeholder": "Enter your password",
    "forgot_password": "Forgot password?",
    "no_account": "Don't have an account?",
    "sign_up_link": "Create one",
    "or_divider": "Or continue with",
    "terms_agree": "I agree to the {terms} and {privacy}",
    "terms": "Terms of Service",
    "privacy": "Privacy Policy"
  },
  
  "validation": {
    "email_required": "Email is required",
    "email_invalid": "Please enter a valid email address",
    "password_min_length": "Password must be at least {minLength} characters",
    "password_mismatch": "Passwords do not match"
  },
  
  "date_formats": {
    "full": "MMMM d, yyyy",       // April 27, 2026
    "short": "MMM d",             // Apr 27
    "time": "h:mm a"             // 3:30 PM
  },
  
  "number_formats": {
    "currency": "${amount,.2f}",  // $1,234.56
    "decimal": "#,##0.00",        // 1,234.56
    "integer": "#,##0"            // 1,234
  }
}
```

### ICU MessageFormat 复杂语法

```json
{
  "message_with_variables": "Hello {name}, you have {count, number} new messages",
  
  "gender_example": "{gender, select, male {He has} female {She has} other {They have}} {count} items",
  
  "plural_example": "You have {count, plural, =0 {no messages} one {# message} other {# messages}}",
  
  "nested_select_plural": 
    "{gender, select, male {{user_male, plural, one {He invited one friend} other {He invited # friends}}}" +
    " female {{user_female, plural, one {She invited one friend} other {She invited # friends}}}" +
    " other {{user_other, plural, one {They invited one friend} other {They invited # friends}}}",
    
  "ordinal": "You finished in {rank, ordinal}",
  // Output: "You finished in 1st" / "2nd" / "3rd"
}
```

---

## 文化适配指南

### 各地区格式差异

| 类别 | 🇺🇸 en-US | 🇨🇳 zh-CN | 🇯🇵 ja-JP | 🇩🇪 de-DE | 🇸🇦 ar-SA |
|------|----------|----------|----------|----------|----------|
| **日期** | 4/27/2026 | 2026年4月27日 | 2026年4月27日 | 27.04.2026 | ٢٧/٤٢/٢٠٢٢٦ |
| **时间** | 2:30 PM | 下午2:30 | 14:30 | 14:30 Uhr | ٢:٣٣ م |
| **数字** | 1,234.56 | 1,234.56 | 1,234.56 | 1.234,56 | ١٬٢٣٤٫.٥٦ |
| **货币** | $1,234.56 | ¥1,234.56 | ￥1,234.56 | 1.234,56 € | ١٬٢٣٤٫.٥٦ ر.س |
| **名姓顺序** | First Last | 姓 名 | 姓 名 | First Last | First Last |
| **地址** | Street, City | 省 市 区 街道 | 都道府県市区町字 | Straße PLZ Stadt | المدينة، الحي |
| **周起始日** | Sunday | Monday | Monday | Monday | Saturday |
| **引号风格** | "double" | 「」or "" | 「」 | „“ or »« | "« |
| **阅读方向** | LTR → | LTR → | LTR → | LTR → | RTL ← |

### RTL (从右到左) 实现要点

```css
/* RTL 支持核心 CSS */
[dir="rtl"] {
  direction: rtl;
  text-align: right;
  
  /* 物理属性镜像 */
  margin-left: auto;
  margin-right: 0;
  padding-left: inherit;
  padding-right: 0;
  border-radius: 0 var(--radius) var(--radius) 0; /* 左上圆角变右上 */
  
  /* Flexbox/Grid 反转 */
  .flex-row { flex-direction: row-reverse; }
  .grid-start { grid-column-start: -1; } /* 从右侧开始 */
}

/* 逻辑属性（自动适配 LTR/RTL） */
.element {
  margin-inline-start: 16px;  /* left in LTR, right in RTL */
  margin-inline-end: 16px;   /* right in LTR, left in RTL */
  padding-inline: 8px;
  inset-inline-start: 0;   /* left/right */
  text-align: start;        /* start = left(LTR) / right(RTL) */
}
```

```javascript
// JavaScript RTL 检测
const isRTL = document.documentElement.dir === 'rtl' || 
                 getComputedStyle(document.documentElement).direction === 'rtl';

// 图标翻转 (对于方向性图标)
if (isRTL) {
  arrowIcon.style.transform = 'scaleX(-1)';
}

// 滚动条位置
if (isRTL) {
  // RTL 中滚动条默认在左侧，可能需要调整
}
```

---

## 质量保证

### 翻译完整性检查

```bash
# 完整性报告
/i18n qa completeness \
  --source en-US \
  --targets zh-CN,ja,ko,de,ar \
  --check missing-keys,extra-keys,stale-values,placeholder-remaining

# 输出示例:
# ════════════════════════════════════════════
# i18n Completeness Report
# ════════════════════════════════════════════
#
# Source: en-US (1,247 keys)
#
# Target: zh-CN
# ├─ Missing keys: 12 ⚠️
# │   ├─ auth.terms_of_service_url (newly added)
# │   ├─ settings.advanced_ai_mode (not translated)
# │   └─ ...
# ├─ Extra keys: 3 🔶
# │   ├─ home.old_welcome_msg (removed from source)
# │   └─ ...
# └─ Coverage: 99.0% ✅
#
# Target: ar
# ├─ Missing keys: 45 ❌
# └─ Coverage: 96.4% ⚠️ (needs attention)
```

### 假数据 (Pseudo-localization)

```json
// pseudoloc.json - 用于测试 UI 是否正确处理长文本
{
  "save": "[!!!Śàvé Ñów !!!]",
  "welcome": "[Ĥéłçōmé ťø Ŵőřld! 👋🌍]",
  "long_text": "Ťhis ís á véry lôńg štrîñg that tésts hów ŷóûr ÚÍ háňdlés lôńg téxtś. " +
             "Ít shôûld bé móré thán 200 cháráctérs tõ téšt límítś.",
  "email": "ëxámplë@ťëst.lôcälízætïøn.cöm"
}
// 使用方式: 开发时选择 pseudoloc 语言包，快速发现布局问题
```

---

*基于 Unicode CLDR / ICU / W3C Internationalization Best Practices*
*版本: 2.0.0 | 最后更新: 2026-04-27*
