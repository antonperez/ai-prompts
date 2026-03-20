# Changelog

All notable changes to this library are documented here.

---

## [1.8.0] — 2026-03-20

### Fixed

**Critical: ~/.claude/ setup audit** — identified and fixed files Claude Code never reads

- `~/.claude/system-prompts/` is NOT loaded by Claude Code — migrated global optimization rules and symbol-first protocol to `~/.claude/CLAUDE.md` (the only file Claude Code reads as global instructions)
- `~/.claude/settings/*.json` (prompt-caching.json, beta-features.json, model-strategy.json, token-optimization.json) are reference documentation only — they do not configure Claude Code behavior
- Removed 13 duplicate SKILL.md copies from `~/.claude/settings/` (canonical copies already in `~/.claude/skills/`)
- Removed 15 misplaced .md files from `~/.claude/settings/` (agent/command copies that don't belong there)
- Removed dead docs from `~/.claude/` root: README.md, QUICK-REFERENCE.md, INSTALLATION-COMPLETE.md (never loaded by Claude Code)
- Updated all model IDs to current: `claude-opus-4-6`, `claude-sonnet-4-6`, `claude-haiku-4-5-20251001`

**01-global-optimization/guide.md** — added compatibility notice table explaining what Claude Code actually reads from `~/.claude/`

### Updated

- `CLAUDE.md` — corrected architecture description (system prompts → CLAUDE.md), updated model compatibility to Sonnet 4.6
- `README.md` — bumped to v1.8.0, documented audit findings

---

## [1.7.1] — 2026-03-20

### Added

**06-advanced-patterns/parallel-agents-guide.md** — Git isolation with Worktrunk

- New section: "Git Isolation with Worktrunk" for running parallel Claude agents in isolated git worktrees
- [Worktrunk](https://worktrunk.dev/) CLI — manages worktrees so each agent operates on a separate branch/directory
- Installation instructions (Homebrew, Cargo, shell integration)
- Core commands reference (`wt switch`, `wt list`, `wt merge`, `wt remove`)
- Claude Code parallel workflow example with 3 concurrent agents
- Hooks integration (`.worktrunk.toml` — `on-create`, `post-merge`)
- Decision table: when to use worktrees vs in-process parallelism

### Updated

- `README.md` — added Worktrunk to Related Tools, updated parallel-agents-guide description, bumped to v1.7.1

---

## [1.7.0] — 2026-03-18

### Fixed

**01-global-optimization/** — resolved issue #1 (skills with no content, side notes breaking structure)

- Created all 5 missing `SKILL.md` files for global skills:
  - `skills/optimize/SKILL.md` — task complexity analysis, planning strategy selection, model routing, session metrics
  - `skills/context/SKILL.md` — full memory lifecycle (load, save, list, refresh, inspect, clear), token savings documentation
  - `skills/cache-inspector/SKILL.md` — cache hit rate monitoring, cost breakdown, optimization recommendations, full report action
  - `skills/update-docs/SKILL.md` — web research + doc comparison + targeted update workflow with `--scope` flag
  - `skills/init-project/SKILL.md` — stack detection for 8+ frameworks, constitution generation, memory creation, per-framework templates
- Created 2 missing system prompt files (previously referenced as placeholders):
  - `system-prompts/global-optimization.md` — all MANDATORY/AUTOMATIC optimization rules, ready to copy to `~/.claude/`
  - `system-prompts/symbol-first-protocol.md` — complete protocol with 5 usage patterns, token savings table, fallback strategy
- Removed circular reference in `setup-agent.md` — no longer references `*[Copy from your machine]*`; now points to actual files in repo with copy commands
- Updated `guide.md` Steps 4–6 to replace placeholder text with real links and descriptions

**README.md** — fixed section ordering (12-desktop-development now appears before 13-security-hardening)

### Added

**03-custom-skills/guide.md** — added `pwa.md` and global skills as examples in the Examples section

**04-research-integration/guide.md** — added cross-reference to `sources.md`

### Fixed

**01-global-optimization/skills/init-project/SKILL.md** — fixed broken iOS link (`ios/guide.md` → `ios/ios-guide.md`)

---

## [1.6.0] — 2026-03-09

### Added

**13-security-hardening/** — new section

- MCP CVE table (2025-2026): 11 vulnerabilities with affected versions, CVSS scores, and patches
- 5-minute MCP audit checklist + community-vetted safe/unsafe list
- Prompt injection defense hooks (PreToolUse scanner + PostToolUse output monitor)
- 6 production safety rules with concrete `settings.json` deny rules and bash hook implementations:
  - Port Stability, Database Safety, Feature Completeness, Infrastructure Lock, Dependency Safety, Pattern Conformance
- `permissions.deny` hardening templates (global `~/.claude/` and per-project `.claude/`)
- Agent Skills supply chain risk data (36.8% of public skills have security flaws per Snyk 2026 scan)
- Incident response playbook (detect, contain, rotate, patch)

**06-advanced-patterns/agent-teams-guide.md**

- Native Agent Teams experimental feature (Claude Code v2.1.32+, Opus 4.6, `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`)
- Architecture overview: peer-to-peer messaging, git-based task claiming, isolated 1M-token contexts per agent
- Decision matrix: Agent Teams vs Task tool (Parallel Agents) vs Dual-Instance vs Multi-Instance
- Setup guide with verification steps
- 4 copy-paste prompt patterns: pre-release review, security PR review, multi-file doc update, parallel refactor
- Limitations and anti-patterns

**06-advanced-patterns/observability-guide.md**

- `cs` bash alias for fast session search (keyword + project filter, ~15ms)
- Community tools comparison (session-search.sh vs claude-conversation-extractor vs ran CLI)
- Per-session and weekly cost tracking scripts (Python, no external deps)
- Pricing reference table (Opus 4.6, Sonnet 4.5, Haiku 4.5)
- Most-read files analysis (candidates for Serena memory or prompt caching)
- Tool usage distribution analysis
- Cross-folder session migration guide
- Team aggregate usage reporting

### Updated

- `README.md` — bumped to v1.6.0, documented all new guides, added claude-code-ultimate-guide to community resources
- `CLAUDE.md` — updated architecture section with sections 06 and 13 descriptions

---

## [1.5.0] — 2026-02-15

### Added

**03-custom-skills/examples/module-mcp/** — `/module:mcp` skill
- Full MCP server implementation for any Laravel project
- Domain analysis → tool scaffolding → implementation workflow
- Read/Write/Destructive tool patterns with proper annotations
- Dual transport (HTTP/SSE + stdio), auth bootstrap trait, global scope fix
- `analyze`, `add <domain>`, `sync` subcommands

**03-custom-skills/examples/module-assistant/** — `/module:assistant` skill
- AI assistant chat panel (Livewire, PrismPHP, resizable sidebar)
- Three provider strategies: cloud (native tools), Claude Code (`<tool_call>` loop), Codex (MCP native)
- Role-based tool registry (read/write/destructive tiers)

---

## [1.4.0] — 2026-02-14

### Added

**12-desktop-development/** — desktop development section
- macOS native (SwiftUI/AppKit) with XcodeBuildMCP + xclaude-plugin
- Tauri v2 (Rust + web) with tauri-plugin-mcp
- Electron (Node.js + Chromium) with electron-mcp-server
- Platform-specific subagents and CLAUDE.md templates for all three platforms
- Cross-platform `/desktop-build` and `/desktop-test` skills

---

## [1.3.0] — 2026-02-14

### Added

**11-mobile-development/** — mobile development section
- iOS (Swift/SwiftUI) with XcodeBuildMCP (59 tools) and xclaude-plugin (87% token savings)
- Android (Kotlin/Compose) with JetBrains MCP and android-mcp-server (ADB)
- React Native with Expo MCP (EAS Build/Update)
- Flutter with Dart/Flutter MCP, Flutter MCP, DCM MCP (450+ rules)
- Platform-specific subagents and CLAUDE.md templates for all four platforms
- Cross-platform `/mobile-build` and `/mobile-test` skills

---

## [1.2.0] — 2026-02-09

### Added

**09-laravel-mcp-integration/** — Laravel MCP ecosystem
- Laravel Boost MCP setup guide (official Laravel MCP server)
- LaraPlugins.io MCP configuration for package discovery
- CLAUDE.md template for Laravel projects (Livewire, Inertia, Filament variants)
- Laravel-specialized subagent with Boost MCP integration

**10-subagents/** — custom subagent system
- Complete subagent creation guide with YAML frontmatter format
- Tool access patterns (read-only, research, code writer, full access)
- Model selection guide (haiku/sonnet/opus by task type)
- Four example subagents: code-reviewer, laravel-specialist, debugger, test-generator

**06-advanced-patterns/** — selective deep plan analysis
- Tiered agent selection system (Tier 1/2/3) to prevent context overflow
- Context budget calculator formula
- Agent priority matrix by project type
- Compact-between-agents and background agent patterns

### Updated

- Model compatibility to Claude 4.6 (Opus 4.6, Sonnet 4.5, Haiku 4.5)

---

## [1.1.0] — 2026-01-22

### Added

**08-ui-ux-development/** — UI/UX development section
- UI/UX Pro Max skill: 50+ styles, 21 color palettes, 50 font pairings
- Dashboard workflow guide with real API integration patterns
- Browser testing guide with Chrome DevTools automation (Claude in Chrome MCP)
- Security best practices: XSS prevention, CSRF protection, empty state patterns

---

## [1.0.0] — 2026-01-04

### Added

Initial release:
- **01-global-optimization** — `~/.claude/` setup: PM Orchestrator, settings, skills, system prompts
- **02-project-activation** — Serena MCP activation and memory templates
- **03-custom-skills** — Skill authoring guide, template, and examples
- **04-research-integration** — Research agent for keeping docs current
- **05-token-optimization** — Token reduction techniques and measurement
- **06-advanced-patterns** — Parallel agents, checkpoint system, constitution framework
- **07-custom-commands** — debug, i18n, qa, seo, deploy, refactor, perf skills
