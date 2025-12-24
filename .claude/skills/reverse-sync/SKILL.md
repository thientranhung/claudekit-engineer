---
name: reverse-sync
description: Detect code-spec deviations and update specs to match implementation reality.
---

# Reverse Sync

Update specifications to match actual implementation when deviations occur.

## When to Use

- After implementation deviates from spec
- When better approach discovered during coding
- After bug fixes reveal spec inaccuracies
- Before session handoff (ensure docs current)
- When `/asdf:sync` is called

## Core Principle

**Truth over aspiration.** Specs must reflect what IS, not what was planned.

## Sync Protocol
Load: `references/sync-protocol.md`

## Quick Reference

### Deviation Types

| Type | Description | Action |
|------|-------------|--------|
| **Addition** | Code has feature not in spec | Add to spec |
| **Change** | Implementation differs from spec | Update spec |
| **Removal** | Spec feature not implemented | Remove or mark deferred |

### Annotation Format

```markdown
<!-- Original: [what spec said] -->
[What was actually implemented]

**Reason:** [Why the change was necessary]
[Reverse Synced: YYMMDD]
```

### Files to Update

1. `spec.md` — The specification itself
2. `changelog.md` — Add sync entry
3. `implementation-active.md` — Note sync occurred

## Quality Checklist

Before completing sync:
- [ ] All deviations identified
- [ ] Original text preserved in comments
- [ ] Reason documented for each change
- [ ] Date annotation added
- [ ] Changelog updated
- [ ] Spec accurately reflects code
