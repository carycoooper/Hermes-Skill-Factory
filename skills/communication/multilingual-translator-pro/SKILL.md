---
name: multilingual-translator-pro
description: Professional-grade translation with context awareness, terminology management, cultural adaptation, domain expertise, and batch processing for 50+ languages including technical, legal, medical, and literary translation.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [translation, localization, multilingual, i18n, communication, language]
    category: communication
    related_skills: [email-management, summarize, deep-research]
    config:
      - key: source_language
        description: "Default source language"
        default: "auto"
        prompt: "What is the primary source language?"
      - key: target_languages
        description: "Preferred target languages"
        default: "[english, chinese, spanish, japanese]"
        prompt: "Translate into which languages?"
---

# Multilingual Translator Pro Skill

Enterprise-level translation engine that bridges language barriers with cultural intelligence.

## When to Use

- **Document Translation**: "Translate this document to Spanish"
- **Website Localization**: "Localize this site for Japanese market"
- **Communication**: "Help me write an email in German"
- **Technical Translation**: "Translate this API documentation"
- **Learning Aid**: "Explain this French text in English"

## Quick Reference

| Command | Description |
|---------|-------------|
| `/translate [text] to [language]` | Direct translation |
| `/translate file [path] --target [lang]` | Document translation |
| `/translate localize [website] --region [country]` | Cultural adaptation |
| `/translate glossary create [domain]` | Build terminology database |

## Procedure

### Phase 1: Context Analysis
1. Detect source language automatically
2. Identify domain (technical/legal/medical/casual/literary)
3. Assess formality level required
4. Note cultural references, idioms, humor

### Phase 2: Translation Execution
5. Core meaning preservation (not word-for-word)
6. Natural phrasing in target language
7. Terminology consistency (use glossary)
8. Format preservation (markdown, code blocks, tables)

### Phase 3: Quality Assurance
9. Back-translation check (translate back, compare)
10. Cultural appropriateness validation
11. Technical accuracy verification (if specialized)
12. Readability scoring (Flesch-Kincaid equivalent)

## Specialized Domains

### Technical Translation
- Preserve variable names, code snippets exactly
- Maintain formatting (API docs, error messages)
- Handle terminology: API, endpoint, payload, authentication

### Legal Translation
- Exact terminology (shall/must vs should/may)
- Jurisdiction-aware (US vs UK vs EU law)
- Date/number format localization
- Disclaimer preservation with legal validity

### Marketing/Creative
- Transcreation (adapt message, not just words)
- Cultural sensitivity (colors, symbols, humor)
- Call-to-action optimization per culture
- SEO keyword adaptation for target market

## Output Templates

### Template A: Standard Translation
```markdown
## 🌐 Translation Result

**Source:** [original text]  
**Source Language:** [detected]  
**Target Language:** [requested]  
**Domain:** [identified]  
**Confidence:** High/Medium/Low  

---  

**[Translated Text]**  

---  

**Notes:**  
- [Cultural adaptation made here]
- [Term choice explanation if ambiguous]
- [Alternative phrasing option]
```

### Template B: Comparison View
```markdown
## 📊 Side-by-Side Comparison

| Original ([source]) | Translation ([target]) |
|---------------------|----------------------|
| [sentence 1] | [translation 1] |
| [sentence 2] | [translation 2] |

**Key Differences Explained:**
- [Why certain choices were made]
```

### Template C: Glossary Entry
```markdown
## 📚 Term: [English Term]

**Translations:**
- 🇪🇸 Spanish: [term-es]
- 🇫🇷 French: [term-fr]
- 🇩🇪 German: [term-de]
- 🇯🇵 Japanese: [term-ja]
- 🇨🇳 Chinese: [term-zh]

**Context:** [when to use this term]  
**Related Terms:** [synonyms, antonyms]  
**Usage Example:** [sentence showing usage in context]
```

## Pitfalls & Best Practices

❌ Avoid:
- Literal translation of idioms ("it's raining cats and dogs" → ❌ not literal)
- Ignoring formality levels (tu/vous, desu/da, informal/formal you)
- Date format assumptions (MM/DD vs DD/MM)
- Number formatting (1,000.00 vs 1.000,00 vs 1'000.00)
- Color symbolism differences (white = purity vs death vs mourning)

✅ Best Practices:
- Always ask about target audience (age, region, profession)
- Provide alternatives for ambiguous terms
- Flag culturally sensitive content
- Maintain brand voice consistency across languages
- Use locale-aware libraries for dates/numbers/currency

## Integration Notes
Works with: Email Management (translate correspondence), Deep Research (multilingual sources), Summarize (foreign language docs)

---

*Breaking language barriers with cultural intelligence*
