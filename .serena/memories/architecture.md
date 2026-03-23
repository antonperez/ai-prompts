# Architecture

## Overview

Documentation-only library of reusable prompts, guides, and agent definitions for optimizing Claude Code workflows. No application code, no build system, no tests — all content is Markdown files organized in numbered directories forming a sequential learning path.

## Directory Structure

```
01-global-optimization/     One-time ~/.claude/ setup (CLAUDE.md, agents, skills)
02-project-activation/      Per-project Serena MCP activation and memory creation
03-custom-skills/           Skill authoring guide + template
04-research-integration/    Keeping docs current with Claude API changes
05-token-optimization/      Token reduction techniques and measurement
06-advanced-patterns/       Multi-agent coordination, constitutions, observability
07-custom-commands/         Ready-to-use slash commands (debug, i18n, qa, etc.)
08-ui-ux-development/       UI/UX workflow with browser testing via Chrome MCP
09-laravel-mcp-integration/ Laravel Boost + LaraPlugins.io MCP setup
10-subagents/               Custom subagent creation (YAML frontmatter format)
11-mobile-development/      iOS, Android, React Native, Flutter guides
12-desktop-development/     macOS, Tauri, Electron guides
13-security-hardening/      MCP CVEs, vetting checklists, prompt injection defense
14-webmcp/                  WebMCP (W3C Draft) integration guide
```

Note: Some directories have duplicates with " 2" suffix (e.g., "10-subagents 2") — likely stale copies to be cleaned up.

## Two Types of Actionable Files per Directory

1. **`guide.md`** — Human-readable walkthroughs, meant to be read
2. **`*-agent.md` / subagent examples** — Executable prompts meant to be pasted into Claude Code conversations

## Key File Patterns

- **YAML frontmatter** (`---` delimited) for metadata in skill files and subagent definitions
- **Skill files**: `~/.claude/skills/<name>/SKILL.md` — YAML frontmatter + Markdown body
- **Subagent definitions**: YAML frontmatter with `model`, `tools`, `mcpServers` fields
- **Memory templates**: stored in `02-project-activation/memory-templates/`
- **Skills bundled in repo**: `01-global-optimization/skills/` — 5 global skills (optimize, context, cache-inspector, update-docs, init-project)
- **Settings templates**: `01-global-optimization/settings/` — JSON config references

## Version Tracking

- Version history tracked in root `README.md`
- Current: v1.11.0 (as of 2026-03-23)
- Compatible with: Claude Opus 4.6, Sonnet 4.6, Haiku 4.5
- CHANGELOG.md exists at root

## Constraints

- Never renumber existing directories
- Maintain Markdown heading hierarchy and section structure
- Preserve YAML frontmatter fields and format
- Update `README.md` version history when adding new sections or making significant changes
- Cross-reference related guides when adding new content
- Template placeholders use bracket format: `[Description]`, `[Step 1]`
- Token savings estimates should be included in guides where applicable

## Last Updated

2026-03-23
