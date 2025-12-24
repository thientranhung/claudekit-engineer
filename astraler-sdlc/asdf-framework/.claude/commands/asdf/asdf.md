---
description: ASDF Toolkit command reference
argument-hint: (no args)
---

# ASDF Toolkit - Command Reference

Show all available ASDF commands grouped by category.

---

## Output Format

```markdown
ASDF Toolkit v4.0 - Command Reference

## Spec Creation
| Command | Purpose |
|---------|---------|
| /asdf:init | Initialize project structure |
| /asdf:spec [name] | Create/edit feature spec |
| /asdf:update [component] | Update system-core component |

## Implementation
| Command | Purpose |
|---------|---------|
| /asdf:code [path] | Implement from spec |
| /asdf:sync | Reverse sync code → spec |
| /asdf:test [feature] | Generate test cases |

## Review & Quality
| Command | Purpose |
|---------|---------|
| /asdf:pr [feature] | Create PR package |
| /asdf:review [path] | AI review (use new instance) |
| /asdf:audit | Check spec health |

## Project Management
| Command | Purpose |
|---------|---------|
| /asdf:status | Current project status |
| /asdf:roadmap | Manage phases & priorities |
| /asdf:report [feature|all] | Progress report |

## Maintenance
| Command | Purpose |
|---------|---------|
| /asdf:cleanup | Remove unused specs |

## Session & Team
| Command | Purpose |
|---------|---------|
| /asdf:handoff | Create session handoff |
| /asdf:onboard | Quick project tour (5 min) |

---

Type '/asdf:[command] --help' for details on any command.
```

---

## Per-Command Help

When user types `/asdf:[command] --help`, show:

```markdown
/asdf:[command] - [Short Description]

## Usage
/asdf:[command] [arguments]

## Arguments
- arg1: Description
- arg2: Description (optional)

## Behavior
1. Step 1
2. Step 2
3. Step 3

## Examples
/asdf:[command] example1
/asdf:[command] example2

## Related
- /asdf:[related1]
- /asdf:[related2]
```

---

## Help

**Usage:** `/asdf`

**Arguments:** None

**Behavior:**
1. Display grouped command reference
2. Show version info
3. Provide hint for per-command help

**Examples:**
- `/asdf` — Show all commands
- `/asdf:spec --help` — Show spec command help

**Related:**
- All ASDF commands
