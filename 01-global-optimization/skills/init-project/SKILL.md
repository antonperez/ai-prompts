---
name: init-project
description: Initialize Claude Code optimization for a new project — detect stack, create memories, generate constitution, configure settings
version: 1.0.0
---

# /init-project — Project Initialization

Sets up Claude Code optimization for a new project in 10-15 minutes. Detects the tech stack, fetches best practices, creates Serena memories, generates a project constitution, and configures optimization settings.

Run once per project. After init, use `/optimize` and `/context load` for all subsequent work.

---

## Usage

```
/init-project [action] [options]
```

### Quick Examples

```
/init-project --full            # Complete initialization (recommended)
/init-project detect            # Just detect the stack
/init-project fetch rails       # Fetch best practices for a language/framework
/init-project constitution      # Generate constitution only
/init-project memories          # Initialize Serena memories only
/init-project optimize          # Configure optimization settings only
```

---

## Quick Start: Full Initialization

```
/init-project --full
```

This runs all steps in sequence and produces a fully configured project. Takes 10-15 minutes. After completion you'll have:

- `.serena/memories/architecture.md`
- `.serena/memories/codebase-conventions.md`
- `.serena/memories/testing-strategy.md`
- `.claude/settings/constitution.json`
- `.claude/settings/token-optimization.json` (project-level)
- `.claude/CLAUDE.md` (if it doesn't exist)

---

## Actions

### `detect` — Auto-detect project type and stack

Scans the project directory to identify the tech stack, frameworks, and conventions.

```
/init-project detect
```

**What it checks**:
- `package.json` → Node.js, framework (Next.js, Express, etc.), test runner
- `composer.json` → PHP/Laravel version and dependencies
- `Gemfile` → Ruby/Rails version
- `requirements.txt` / `pyproject.toml` → Python framework
- `Cargo.toml` → Rust
- `go.mod` → Go
- `*.xcodeproj` / `pubspec.yaml` → iOS/Flutter
- `Dockerfile` / `docker-compose.yml` → containerization
- `.github/workflows/` → CI/CD setup
- `README.md` → project description

**Output**:
```
## Project Detection Results

Stack detected:
- Language: Ruby 3.3
- Framework: Rails 8.0
- Database: PostgreSQL (ActiveRecord)
- Frontend: Hotwire (Turbo + Stimulus)
- Testing: RSpec + FactoryBot
- CI: GitHub Actions
- Docker: Yes (docker-compose.yml)
- Auth: Devise

Recommended memories to create:
- architecture.md ✓
- codebase-conventions.md ✓
- testing-strategy.md ✓
- docker-workflow.md ✓

Recommended constitution rules:
- Rails conventions (service objects, concerns, concerns)
- RSpec best practices
- Hotwire patterns

Run /init-project --full to complete setup.
```

---

### `fetch [framework]` — Fetch best practices for your stack

Fetches current best practices, conventions, and patterns for the detected or specified framework.

```
/init-project fetch rails
/init-project fetch nextjs
/init-project fetch laravel
/init-project fetch fastapi
/init-project fetch flutter
```

**Supported stacks**:

| Framework | What's fetched |
|-----------|---------------|
| `rails` | Rails 8 conventions, Hotwire patterns, service objects |
| `laravel` | Laravel 11 patterns, Livewire, Eloquent conventions |
| `nextjs` | App Router patterns, Server Components, data fetching |
| `fastapi` | Pydantic models, async patterns, dependency injection |
| `django` | Models, views, serializers, Django REST Framework |
| `express` | Middleware patterns, route organization, error handling |
| `flutter` | Widget patterns, state management, platform conventions |
| `ios` | SwiftUI, UIKit, async/await, MVVM patterns |

**Process**:
1. Search official documentation for current version
2. Find community-accepted conventions
3. Identify anti-patterns to avoid
4. Extract testing conventions
5. Summarize into memory-ready format

---

### `constitution` — Generate project constitution

Creates `.claude/settings/constitution.json` with architectural rules tailored to the detected stack.

```
/init-project constitution
/init-project constitution --framework rails
/init-project constitution --framework nextjs --strict
```

**What a constitution contains**:
```json
{
  "version": "1.0.0",
  "project": "my-rails-app",
  "framework": "Rails 8.0",
  "principles": [
    {
      "id": "fat-models-skinny-controllers",
      "rule": "Business logic belongs in models or service objects, not controllers",
      "enforcement": "mandatory",
      "examples": {
        "correct": "UserRegistrationService.call(params)",
        "incorrect": "def create; @user = User.new; if @user.save_with_team...; end"
      }
    },
    {
      "id": "no-raw-sql",
      "rule": "Use ActiveRecord query interface, not raw SQL strings",
      "enforcement": "mandatory",
      "exceptions": ["complex reports with CTEs are acceptable"]
    }
  ],
  "code_quality": {
    "max_method_lines": 15,
    "max_class_lines": 200,
    "test_coverage_minimum": "80%"
  },
  "security": {
    "no_user_input_in_sql": true,
    "validate_at_model_layer": true,
    "use_strong_parameters": true
  }
}
```

**Constitution rules by framework**:

**Rails**: fat models, service objects, FormRequest for validation, no N+1 queries, Hotwire over JavaScript
**Laravel**: repositories optional but consistent, FormRequest validation, eager loading, feature flags in config
**Next.js**: Server Components by default, Client Components only for interactivity, no prop drilling past 2 levels
**FastAPI**: Pydantic for all I/O, dependency injection for DB/auth, async everywhere

---

### `memories` — Initialize Serena memories

Creates the initial set of Serena memories by analyzing the codebase.

```
/init-project memories
```

**Creates these files** in `.serena/memories/`:

#### `architecture.md`
Analyzes the project structure and documents:
- Top-level directory layout and purpose
- Key classes/modules and their responsibilities
- Data flow between layers
- External dependencies and integrations
- Deployment topology

#### `codebase-conventions.md`
Documents coding standards by scanning the codebase:
- Naming conventions (files, classes, methods, variables)
- File organization patterns
- Preferred libraries and why
- Anti-patterns found in existing code (to be consistent with or avoid)
- Comment and documentation style

#### `testing-strategy.md`
Documents the testing approach:
- Test frameworks in use
- Directory structure for tests
- Factory/fixture patterns
- What's tested and how (unit vs integration vs e2e)
- How to run tests locally

**Time**: ~5 minutes per memory (reads relevant files symbolically)

---

### `optimize` — Configure project-level optimization settings

Creates `.claude/settings/token-optimization.json` with project-specific overrides.

```
/init-project optimize
```

Customizes the global token optimization settings for this specific project:
- Which memories to always load
- Constitution rules to enforce
- Model selection overrides (e.g., always use Haiku for migrations)
- Project-specific checkpoint locations

---

## CLAUDE.md Generation

If no `CLAUDE.md` exists, `--full` will create one:

```
/init-project --full
```

The generated `CLAUDE.md` includes:
- Project overview and stack summary
- Development workflow (setup, testing, deployment)
- Key conventions (based on constitution)
- Common commands
- Links to Serena memories for architecture details

---

## After Initialization

Once init is complete, start working:

```bash
# Load context at the start of every session
/context load

# Start work with full optimization
/optimize "your task description"

# Check everything is working
/optimize status
```

---

## Per-Framework CLAUDE.md Templates

See framework-specific guides for project-type CLAUDE.md templates:
- [Laravel](../../../09-laravel-mcp-integration/guide.md)
- [iOS](../../../11-mobile-development/ios/ios-guide.md)
- [macOS / Tauri / Electron](../../../12-desktop-development/)

---

## Troubleshooting

### "Stack not detected"

```
/init-project detect
# → Could not detect stack automatically
```

Specify manually:
```
/init-project fetch [framework] --force
```

### "Serena not available"

`memories` step requires Serena MCP. The other steps (`constitution`, `optimize`) work without it. Install Serena and run `/init-project memories` separately.

### "Constitution conflicts with existing code"

The generated constitution reflects best practices, not necessarily your current codebase. Either:
- Use `--lenient` flag to generate more permissive rules
- Edit `.claude/settings/constitution.json` manually after generation

---

## Time Estimate

| Action | Time |
|--------|------|
| `detect` | ~30 sec |
| `fetch [framework]` | ~2 min |
| `constitution` | ~2 min |
| `memories` (3 files) | ~8 min |
| `optimize` | ~1 min |
| `--full` (all steps) | 10-15 min |

---

## See Also

- [`/context`](../context/SKILL.md) — Load memories after init
- [`/optimize`](../optimize/SKILL.md) — Start optimized work sessions
- [Project Activation Guide](../../../02-project-activation/guide.md)
- [Custom Skills Guide](../../../03-custom-skills/guide.md)
