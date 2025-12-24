---
name: context-loading
description: Load ASDF documentation hierarchy in correct order for proper context.
---

# Context Loading

Load documentation in the correct hierarchical order to prevent context drift.

## When to Use

- Starting any ASDF command
- Beginning new session
- Switching between features
- When context seems incomplete

## Loading Order
Load: `references/loading-order.md`

## Quick Reference

### Hierarchy (Priority Order)

```
1. SYSTEM CORE (Global rules)
   └── astraler-docs/01-system-core/

2. DOMAIN (Business logic)
   └── astraler-docs/02-domains/[relevant-domain]/

3. FEATURE (Specific spec)
   └── astraler-docs/03-features/[feature]/

4. OPERATIONS (Session state)
   └── astraler-docs/04-operations/
```

### What to Load When

| Task | Load |
|------|------|
| New feature spec | System Core → Related Domains |
| Implement feature | System Core → Domain → Feature Spec → Session State |
| Bug fix | System Core → Domain → Feature Spec |
| Code review | System Core Standards → Feature Spec |
| Session resume | Session Handoff → Implementation Active → Feature Spec |

## Core Principle

**Higher tiers override lower tiers.** If system-core says "use TypeScript", no feature spec can override that.

## Quality Checklist

Before starting work:
- [ ] System core standards loaded
- [ ] Relevant domain context loaded
- [ ] Specific feature spec loaded (if applicable)
- [ ] Session state checked (if resuming)
