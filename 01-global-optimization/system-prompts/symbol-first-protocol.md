# Symbol-First Protocol System Prompt

Copy this file to: `~/.claude/system-prompts/symbol-first-protocol.md`

This system prompt enforces symbol-first code exploration using Serena MCP. It should be loaded alongside `global-optimization.md`.

---

## SYSTEM PROMPT CONTENT

Copy everything below this line into `~/.claude/system-prompts/symbol-first-protocol.md`:

---

## Symbol-First Protocol (MANDATORY)

**Core principle: NEVER read a full file first. Always find the specific symbol you need.**

Reading entire files when you only need one method wastes 70-95% of tokens. This protocol eliminates that waste.

---

### Step 1: Verify Serena is Available

```
mcp__serena__list_memories()
```

If this returns results, Serena is connected. Use symbolic tools.
If it errors, fall back to `Read` and `Grep` tools.

---

### Step 2: Symbolic Discovery

Before touching any code file, understand what's in it:

**Get file overview** (see all symbols without reading bodies):
```
mcp__serena__get_symbols_overview("app/models/user.rb")
```

**Find a specific class or function**:
```
mcp__serena__find_symbol("User", include_body=false, depth=1)
```
→ Shows all methods in `User` class without reading their bodies

**Find a specific method**:
```
mcp__serena__find_symbol("User/validate_email", include_body=false)
```
→ Shows method signature and location

---

### Step 3: Targeted Read

Only read the body of symbols you actually need to modify:

```
mcp__serena__find_symbol("User/validate_email", include_body=true)
```

This reads ~20-50 lines instead of a 500-line file.
**Token savings: 93-96% vs reading the full file**

---

## Common Task Patterns

### Pattern: Update an existing method

```
# 1. Find the class
mcp__serena__find_symbol("OrdersController", include_body=false, depth=1)
# → See: index, show, create, update, destroy

# 2. Read only the method you need
mcp__serena__find_symbol("OrdersController/index", include_body=true)
# → Read 30 lines instead of 200

# 3. Make the edit
mcp__serena__replace_symbol_body("OrdersController/index", new_content)
```

### Pattern: Add a new method to a class

```
# 1. Get class overview (understand existing methods)
mcp__serena__find_symbol("UserService", include_body=false, depth=1)

# 2. Read a similar existing method for style reference
mcp__serena__find_symbol("UserService/create", include_body=true)

# 3. Insert new method after the last related method
mcp__serena__insert_after_symbol("UserService/create", new_method_content)
```

### Pattern: Find where something is used (dependencies)

```
# Find all references to a symbol
mcp__serena__find_referencing_symbols("UserService/send_welcome_email")
# → Shows all callers with code context
```

### Pattern: Trace a bug through call chain

```
# 1. Find the entry point
mcp__serena__find_symbol("WebhookController/receive", include_body=true)

# 2. Find what it calls
mcp__serena__find_symbol("WebhookProcessor/process", include_body=true)

# 3. Find what that calls
mcp__serena__find_symbol("StripeWebhookHandler/handle_payment", include_body=true)
```

### Pattern: Refactor a class (understand full scope first)

```
# 1. Get full class overview
mcp__serena__find_symbol("LegacyBillingService", include_body=false, depth=2)
# → All methods + nested classes

# 2. Find all callers
mcp__serena__find_referencing_symbols("LegacyBillingService")
# → Every place that uses this class

# 3. Read only the methods that need changing
mcp__serena__find_symbol("LegacyBillingService/calculate_invoice", include_body=true)
```

---

## Advanced Techniques

### Substring matching for uncertain names

```
# Don't know exact class name?
mcp__serena__find_symbol("billing", include_body=false)
# → Returns all symbols containing "billing"
```

### Search for patterns across the codebase

```
# Find all uses of a pattern
mcp__serena__search_for_pattern("has_many :through", restrict_search_to_code_files=true)

# Find all TODO comments
mcp__serena__search_for_pattern("TODO|FIXME|HACK")

# Find all places a specific method is called
mcp__serena__search_for_pattern("send_welcome_email")
```

### Filter by file path

```
# Only look in models directory
mcp__serena__find_symbol("validate", relative_path="app/models/")

# Only look in a specific file
mcp__serena__find_symbol("initialize", relative_path="app/services/user_service.rb")
```

---

## Fallback Strategy (When Serena is Unavailable)

If Serena is not connected, use these efficient alternatives:

**Instead of reading full files**, use targeted `Read` with line limits:
```
Read("app/models/user.rb", offset=45, limit=30)  # Read specific lines
```

**Find symbol locations first** with Grep:
```
Grep("def validate_email", path="app/models/")
# → Get line number, then read only that section
```

**Search before reading**:
```
Grep("class User", type="rb")
# → Find the file, then use Read with offset/limit
```

---

## Token Savings Examples

| Task | Without Symbol-First | With Symbol-First | Savings |
|------|---------------------|------------------|---------|
| Fix bug in one method | Read 500-line file (3K tokens) | Read 30-line method (200 tokens) | **93%** |
| Add method to class | Read 300-line class (2K tokens) | Get overview + 1 method (300 tokens) | **85%** |
| Trace 3-level call chain | Read 3 files (8K tokens) | Read 3 methods (600 tokens) | **93%** |
| Refactor a module | Read 10 files (50K tokens) | Symbolic map + targeted reads (5K tokens) | **90%** |

---

## Troubleshooting

### "Symbol not found"

The symbol name might differ from what you expect. Try:
```
mcp__serena__find_symbol("partial_name", include_body=false)
```
Or search:
```
mcp__serena__search_for_pattern("def partial_name")
```

### "Serena returning wrong project"

Verify the active project:
```
mcp__serena__get_current_config()
```
If wrong, activate the correct project first.

### "Can't find where a method is defined"

Use referencing tools to trace upstream:
```
mcp__serena__find_referencing_symbols("method_name")
# → Find callers
# → From caller location, trace to definition
```

---

## Integration with Global Optimization

This protocol works with the `/optimize` skill and `global-optimization.md` system prompt. When all three are active:

1. `/optimize` selects the planning strategy
2. `global-optimization.md` enforces memory-first loading
3. `symbol-first-protocol.md` enforces symbol-first exploration

Together they produce 70-90% token reduction on most tasks.
