# Content Review Skill

Manually audit all user-facing content (documentation, UI copy, error messages, help text, translations) for technical accuracy, consistency, and quality.

## Usage

`/content-review [scope]` where scope is:

- (no args) - Full content audit (default)
- `docs` - Documentation files only
- `ui` - UI-facing content only (views, translations, components)
- `errors` - Error messages and validation text
- `api` - API documentation and responses

## What This Skill Does

Performs a comprehensive content audit:

1. **Discover Content** — find all user-facing text (docs, views, translations, error messages, config comments)
2. **Verify Technical Accuracy** — check terminology matches actual code (model names, endpoints, config keys)
3. **Check Consistency** — terminology, capitalization, naming conventions, voice/tone across all content
4. **Assess Quality** — clarity, completeness, grammar, outdated information, broken references
5. **Report Findings** — categorized as Critical/High/Medium with file:line references and suggested fixes

## Review Process

### Step 1: Discovery

Find all content files based on scope:

```bash
# Documentation
docs/**/*.md
README.md
CLAUDE.md
TODO*.md
*.md (root level)

# UI content
resources/views/**/*.blade.php
app/Livewire/**/*.php (look for strings)
resources/js/**/*.js (look for UI text)

# Translation files
lang/**/*.php

# Error messages
app/Http/Requests/**/*Request.php (validation messages)
app/Exceptions/**/*.php (exception messages)

# API responses
app/Http/Controllers/Api/**/*.php (response messages)
routes/api.php (route comments)
```

### Step 2: Technical Accuracy Check

For each content piece:

1. **Extract technical terms**: Model names, endpoints, commands, config keys
2. **Verify against codebase**:
   - Model name mentioned? Check `app/Models/`
   - Endpoint documented? Check `routes/api.php` or `routes/web.php`
   - Command shown? Check `app/Console/Commands/`
   - Config key mentioned? Check `config/`
3. **Flag mismatches**: Document vs Reality

### Step 3: Consistency Audit

Track terminology usage across files:

```
Terms Found:
- "Organization" (42 uses)
- "Org" (5 uses) ← Inconsistent!

Capitalization:
- "PostGIS" (correct, 12 uses)
- "Postgis" (incorrect, 3 uses) ← Fix
```

For multilingual projects, also check:
- **Translation completeness**: All keys present in every language
- **Duplicate keys**: PHP arrays silently overwrite — find and remove duplicates
- **Grammar conventions**: Sentence case vs Title Case per language rules
- **Slang/informal language**: Flag colloquial terms that should be formal

### Step 4: Quality Assessment

- **Clarity**: Is it easy to understand?
- **Completeness**: Are all steps/labels included?
- **Accuracy**: Does it match the current codebase?
- **Grammar**: Correct spelling, punctuation, sentence structure
- **Examples**: Are code examples correct and runnable?
- **Links**: Do internal references work?

### Step 5: Generate Report

## Output Format

```markdown
# Content Review Report

## Scope
- Files reviewed: N
- Documentation: N files
- UI content: N files
- Translation files: N files
- Error messages: N files

## Critical Issues (Incorrect Information)

### file:line
**Issue**: Description
**Current**: quoted text
**Fix**: Suggested correction

## High Priority (Inconsistencies)

### Category: description
**Files affected**: N
**Standard**: What it should be
**Recommendation**: How to fix

## Medium Priority (Polish)

### Description
**File**: path
**Recommendation**: suggestion

## Summary Statistics
- Critical issues: N
- High priority: N
- Medium priority: N
- Total issues: N
```

## Language-Specific Checks

### Bulgarian (bg/)
- Sentence case for UI labels (not Title Case — "Нов абонамент", not "Нов Абонамент")
- Comma before "за да" (purpose clauses)
- No Latin characters in Cyrillic text (common typo: Latin "c" instead of Cyrillic "с")
- Formal register (avoid slang like "логнати" — use "влезли в системата")

### English (en/)
- Consistent Oxford comma usage
- Action verbs for buttons ("Save changes", not "Changes saved")
- Sentence case for labels (unless brand names)

## Quality Checklist

### Technical Accuracy
- [ ] Model names match `app/Models/`
- [ ] Endpoint routes match `routes/api.php`
- [ ] Commands match `app/Console/Commands/`
- [ ] Config keys match `config/`
- [ ] Examples use correct syntax
- [ ] Database schema matches migrations

### Consistency
- [ ] Terminology standardized (check for variants)
- [ ] Capitalization follows conventions
- [ ] Code style consistent in examples
- [ ] Voice and tone uniform
- [ ] Translation keys match across all languages

### Quality
- [ ] Clear and concise
- [ ] Complete (no missing steps/keys)
- [ ] Well-organized
- [ ] Current (not outdated)
- [ ] Links work (internal references)
- [ ] No duplicate PHP array keys

### Content Types

**UI Labels**: Clear, actionable, sentence case
**Error Messages**: Helpful (explain how to fix), friendly, non-technical
**Placeholder Text**: Good examples, not lorem ipsum
**Button Text**: Action verbs ("Save", "Create", "Delete")
**Documentation**: Complete steps, correct examples, current syntax
**API Docs**: All endpoints listed, request/response examples, auth requirements
