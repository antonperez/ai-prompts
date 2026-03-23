# Codebase Conventions

## Overview

Documentation-only repository. All conventions govern Markdown authoring, file naming, directory structure, and content formatting — not code style.

## File Naming

- Primary entry point per directory: `guide.md`
- Executable agent prompts: `*-agent.md` (e.g., `setup-agent.md`, `activation-agent.md`)
- Skill files: `SKILL.md` (uppercase, inside a named subdirectory)
- Templates use descriptive names: `architecture-template.md`, `conventions-template.md`
- Settings/config files: kebab-case JSON (e.g., `prompt-caching.json`, `beta-features.json`)

## Directory Naming

- Zero-padded numbers + kebab-case topic: `01-global-optimization`, `13-security-hardening`
- Never renumber existing directories
- Subdirectories follow topic naming: `memory-templates/`, `skills/`, `settings/`, `system-prompts/`

## Document Structure

### Standard guide.md heading hierarchy
```markdown
# Title
## Section
### Subsection
```
- Maintain existing heading hierarchy when editing
- Do not skip heading levels

### YAML Frontmatter (skill/agent files)
```yaml
---
field: value
---
```
- All skill and subagent definition files use YAML frontmatter
- Subagent definitions include: `model`, `tools`, `mcpServers` fields
- Preserve existing frontmatter fields; do not add/remove without intent

### Template Placeholders
- Format: `[Description]`, `[Step 1]`, `[Your Value Here]`
- Always use square brackets for user-fillable placeholders

## Content Conventions

- **Token savings estimates** must be included in guides (e.g., "Token savings: 60-70%")
- **Time estimates** for setup steps included where applicable (e.g., "Time: 10-15 minutes")
- **Cross-references**: link to related guides when adding new content
- **Version compatibility** noted where relevant (e.g., "Compatible with Claude Opus 4.6, Sonnet 4.6, Haiku 4.5")
- **Agent Tools notes**: agent capability additions like "(v2.1.32+)" noted inline

## Version Tracking

- Root `README.md` is the canonical version history file
- Update version header and changelog section on significant additions
- Current version format: `v1.X.Y` (semver-like)
- `CHANGELOG.md` at root for detailed change history

## Anti-Patterns

- Do not create new numbered directories unless adding a genuinely new topic
- Do not add " 2" suffix duplicates (these are legacy artifacts to be cleaned up)
- Do not embed executable code — this is a documentation library
- Do not use emoji in file content unless already present in that file's style
- Do not change bracket placeholder format to other styles (e.g., `{{}}` or `<>`)

## Last Updated

2026-03-23
