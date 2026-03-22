# Token Optimization Checklist

**Quick verification that all optimizations are active**

Use this checklist at the start of a new project or when savings seem low.

---

## Foundation (One-Time Global Setup)

Run these once after completing `01-global-optimization`:

- [ ] `~/.claude/CLAUDE.md` exists with optimization rules
- [ ] `~/.claude/skills/optimize/SKILL.md` installed (`/optimize` works)
- [ ] `~/.claude/skills/context/SKILL.md` installed (`/context` works)
- [ ] `~/.claude/skills/cache-inspector/SKILL.md` installed (`/cache-inspector` works)
- [ ] `~/.claude/settings/token-optimization.json` present (reference doc)
- [ ] `~/.claude/settings/model-strategy.json` present (reference doc)
- [ ] Serena MCP configured in Claude Code

**Verify**: `/cache-inspector status` returns cache data (not "no data available")

---

## Per-Project Setup

Run these once per project after completing `02-project-activation`:

- [ ] Serena activated: `mcp__serena__get_current_config` shows current project
- [ ] Core memories exist:
  - [ ] `architecture.md` (≥1024 tokens for cache eligibility)
  - [ ] `codebase-conventions.md` (≥1024 tokens)
  - [ ] At least one domain-specific memory (module-structure, testing-strategy, etc.)
- [ ] `/context load` runs without errors at session start

**Verify**: `mcp__serena__list_memories` returns 3+ memories

---

## Per-Session Checklist

Run at the start of each session:

- [ ] Load context: `/context load`
- [ ] Check cache: `/cache-inspector status`
- [ ] Cache hit rate ≥ 70% after first 3 messages? (If not, memories may be too small)

---

## Code Exploration Habits

Check these during work:

- [ ] Using `mcp__serena__get_symbols_overview` before `Read` on code files
- [ ] Using `mcp__serena__find_symbol` to locate specific methods (not full-file reads)
- [ ] Only using `Read` for: config files, small files (<50 lines), non-code files
- [ ] Using `mcp__serena__find_referencing_symbols` before renaming/changing APIs

---

## Planning Discipline

Match planning depth to task complexity:

| Task Size | Strategy | Skip |
|-----------|----------|------|
| 1-2 subtasks | Unified (direct) | Requirements, design phases |
| 3-7 subtasks | Intent-Planning | Formal requirements doc |
| 8+ subtasks | Full Planning | Nothing — invest upfront |

- [ ] Not running full planning workflow on simple bug fixes
- [ ] Not skipping planning on 8+ subtask features

---

## Model Selection

- [ ] Using Haiku for: task decomposition, formatting, simple queries
- [ ] Using Sonnet for: implementation, design, testing
- [ ] Only using Opus for: security reviews, architecture decisions, judge evaluation
- [ ] NOT defaulting to Opus for "complex" tasks that are just large, not critical

---

## Warning Signs

If you see these, optimization is degrading:

- Cache hit rate below 50% → run `/context load`, check memory sizes
- Frequently reading 500+ line files in full → switch to symbol tools
- Using Opus for implementation tasks → switch to Sonnet
- Sessions taking 2× longer than similar past sessions → review tool call patterns
- `/cache-inspector optimize` showing many recommendations → act on them

---

## Quick Diagnostic

Run this sequence when something feels off:

```
/cache-inspector status          # Check cache health
mcp__serena__list_memories       # Confirm memories exist
mcp__serena__get_current_config  # Confirm Serena is on right project
/cache-inspector optimize        # Get specific recommendations
```

---

## Targets

| Metric | Minimum | Good | Excellent |
|--------|---------|------|-----------|
| Cache hit rate | 60% | 75% | 90%+ |
| Token reduction vs baseline | 40% | 60% | 75%+ |
| Memories loaded | 2 | 3-4 | 5+ |
| Haiku task % | 20% | 35% | 45%+ |

---

**See also**: [guide.md](guide.md) for technique details · [metrics.md](metrics.md) for measurement · [optimization-agent.md](optimization-agent.md) for automated analysis
