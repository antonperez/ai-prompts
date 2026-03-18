---
name: update-docs
description: Keep project documentation current by researching latest patterns, fetching sources, and updating docs with findings
version: 1.0.0
---

# /update-docs — Documentation Updater

Keeps your Claude Code configuration documentation current with the latest Claude API changes, best practices, and framework updates. Searches the web, compares findings to existing docs, and applies targeted updates.

Especially useful for keeping `~/.claude/` system prompts, skill files, and project memories accurate after Claude API updates.

---

## Usage

```
/update-docs [action] [target]
```

### Quick Examples

```
/update-docs                           # Interactive mode — ask what to update
/update-docs research "prompt caching" # Search web for latest info
/update-docs collect https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching
/update-docs analyze                   # Compare findings to existing docs
/update-docs update global-optimization.md
/update-docs update --scope global     # Update all global docs
/update-docs validate                  # Check docs for outdated content
```

---

## Actions

### `research [topic]` — Search web for latest patterns

Searches for current documentation, changelog entries, and community best practices on the given topic.

```
/update-docs research "Claude prompt caching"
/update-docs research "Serena MCP latest features"
/update-docs research "token efficient tools beta"
/update-docs research "Claude Code skills format"
```

**Process**:
1. Web search for official docs, Anthropic changelog, GitHub issues
2. Identify version-specific changes and deprecations
3. Extract actionable guidance
4. Store findings as a research summary

**Output**:
```
## Research: Claude Prompt Caching

### Sources Found
- https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching (official, current)
- Anthropic changelog: cache TTL extended to 1 hour in API v2025-01 (new)
- GitHub discussion: min_tokens threshold reduced to 512 in some regions (unverified)

### Key Changes Since Last Doc Update
1. Cache TTL now 1 hour (was 10 min) with explicit cache_control
2. New cache_read_input_tokens metric in API response
3. System prompt caching now available in all Claude models (was Sonnet+ only)

### Outdated Content in Your Docs
- prompt-caching.json: ttl_minutes set to 10 (should be 60)
- global-optimization.md: mentions "10 minute TTL" (now 60 min)

Run /update-docs analyze to see full comparison.
```

---

### `collect [url]` — Fetch content from a specific URL

Fetches and extracts relevant content from a specific documentation URL.

```
/update-docs collect https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching
/update-docs collect https://github.com/oraios/serena/blob/main/README.md
```

**Process**:
1. Fetch URL content
2. Extract key technical information
3. Summarize relevant sections
4. Add to research findings for `analyze` step

---

### `analyze` — Compare findings to existing docs

Compares research findings or collected content to your existing documentation and reports discrepancies.

```
/update-docs analyze
/update-docs analyze --target ~/.claude/system-prompts/global-optimization.md
```

**Output**:
```
## Documentation Analysis

### Comparing against: ~/.claude/system-prompts/global-optimization.md

OUTDATED sections:
- Line 45: "Cache TTL: 10 minutes" → should be "60 minutes"
- Line 112: Lists claude-sonnet-4-5 as latest → claude-sonnet-4-6 released
- Line 203: token-efficient-tools beta header — verify if still in beta

MISSING sections:
- No mention of interleaved thinking (added in API v2025-05)
- cache_read_input_tokens metric not documented

UP TO DATE:
- Prompt caching setup instructions ✅
- Haiku/Sonnet/Opus model selection ✅
- Symbol-first protocol ✅

Recommendation: 3 sections need updates. Run /update-docs update to apply.
```

---

### `update [target]` — Apply updates to documentation

Updates a specific file or set of files based on research findings.

```
/update-docs update global-optimization.md
/update-docs update prompt-caching.json
/update-docs update --scope global         # Update all ~/.claude/ docs
/update-docs update --scope memories       # Update Serena memories
/update-docs update --dry-run              # Preview changes without applying
```

**Scope options**:
| Scope | Files updated |
|-------|--------------|
| `global` | All `~/.claude/` documentation and skills |
| `memories` | All Serena memories in `.serena/memories/` |
| `settings` | JSON config files in `~/.claude/settings/` |
| `project` | Current project's `.claude/` documentation |

**Process**:
1. Load the target file
2. Apply only the changes identified in `analyze`
3. Preserve existing structure and formatting
4. Show diff of what changed

---

### `validate` — Check documentation for outdated content

Scans documentation for common staleness indicators without requiring prior research.

```
/update-docs validate
/update-docs validate --path ~/.claude/
/update-docs validate --path .serena/memories/
```

**Checks performed**:
- Model IDs that no longer exist (e.g., old Haiku/Sonnet versions)
- API beta headers that may have graduated to stable
- URLs that return 404 or redirect
- Date-stamped content older than 90 days
- Deprecated patterns (e.g., old tool formats)

**Output**:
```
## Documentation Validation Report

Files scanned: 12
Issues found: 4

WARNINGS:
⚠️ global-optimization.md:45 — model ID "claude-haiku-4-5" may be outdated
⚠️ beta-features.json:12 — beta header "token-efficient-tools-2025-02-19" may have graduated
⚠️ architecture.md — last updated 47 days ago, consider refreshing

ERRORS:
❌ system-prompts/symbol-first-protocol.md:83 — URL returns 404
   → https://docs.anthropic.com/old-tools-format (moved)

Run /update-docs research [topic] to investigate specific warnings.
```

---

## Workflow: Full Documentation Refresh

Recommended monthly workflow:

```bash
# Step 1: Research recent changes
/update-docs research "Claude API changelog 2026"
/update-docs research "Serena MCP updates"

# Step 2: Validate existing docs
/update-docs validate --path ~/.claude/

# Step 3: Analyze specific files
/update-docs analyze --target ~/.claude/system-prompts/global-optimization.md

# Step 4: Apply updates
/update-docs update --scope global --dry-run   # Preview first
/update-docs update --scope global              # Apply

# Step 5: Update Serena memories
/update-docs update --scope memories
```

---

## Token Optimization

This skill is designed to be efficient:
- Uses web search sparingly (only when needed)
- Caches research findings within the session
- Applies targeted edits rather than rewriting entire files
- Skips files that are already up to date

**Typical token usage**:
- `research [topic]`: ~3-8K tokens (includes web search)
- `analyze`: ~2-4K tokens
- `update [file]`: ~2-5K tokens per file
- `validate`: ~1-3K tokens

---

## Troubleshooting

### "No changes needed"

Your docs are up to date. Run `/update-docs validate` monthly to stay current.

### "URL fetch failed"

Some documentation sites block automated fetches. Try:
```
/update-docs collect [alternative-url]
```
Or search for the content manually and paste into the conversation.

### "Changes look wrong"

Use `--dry-run` to preview before applying:
```
/update-docs update --dry-run global-optimization.md
```

---

## See Also

- [`/init-project`](../init-project/SKILL.md) — Initial docs setup for a new project
- [`/context refresh`](../context/SKILL.md) — Refresh Serena memories specifically
- [Research Integration Guide](../../../04-research-integration/guide.md)
