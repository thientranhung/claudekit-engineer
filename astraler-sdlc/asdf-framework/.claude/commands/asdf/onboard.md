---
description: ONBOARD MODE - Quick 5-minute project tour
argument-hint: (no args)
---

# ONBOARD MODE: Guided Project Tour

Quick 5-minute introduction for new or returning developers.

---

## Skills Required

- **Activate:** `context-loading` (for project data)

---

## Workflow

Present 4 sections, each ~1-2 minutes:

### Section 1: Project Overview (~1 min)

Load from `01-system-core/`:
- Project name and purpose (from master-map.md)
- Tech stack summary (from tech-stack.md)

```markdown
## 1. Project Overview

**What is this?**
[Project description from master-map.md]

**Tech Stack:**
- Frontend: [framework + UI library]
- Backend: [framework + runtime]
- Database: [primary database]
- Testing: [test frameworks]

---
```

---

### Section 2: Current Status (~1 min)

Load from `04-operations/`:
- Phase from roadmap.md
- Feature counts from implementation-active.md
- Overall progress

```markdown
## 2. Current Status

**Phase:** [Phase name from roadmap]
**Progress:** [X]% complete

| Feature | Status |
|---------|--------|
| [feature1] | Done |
| [feature2] | Active ([X]%) |
| [feature3] | Active ([Y]%) |
| [feature4] | Planned |

---
```

---

### Section 3: Active Work (~1 min)

Load from `04-operations/`:
- Locked features from locks/
- Active blockers from implementation-active.md

```markdown
## 3. Active Work

**Currently being implemented:**
| Feature | Instance | Progress |
|---------|----------|----------|
| [feature] | [instance-id] | [X]% |

**Blockers:**
- [Blocker description] (affects: [feature])

**Available for work:**
- [feature-name] (P0 priority)
- [feature-name] (P1 priority)

---
```

---

### Section 4: How to Start (~2 min)

Provide actionable commands:

```markdown
## 4. How to Start

**Continue existing work:**
```bash
/asdf:status                    # See what's active
/asdf:code [feature-path]       # Implement a feature
```

**Start new work:**
```bash
/asdf:spec [feature-name]       # Create new feature spec
/asdf:roadmap                   # See project phases
```

**Get help:**
```bash
/asdf                           # All commands
/asdf:report all                # Project health
/asdf:audit                     # Check for issues
```

---

**Quick Reference:**
| Task | Command |
|------|---------|
| Check status | `/asdf:status` |
| Implement feature | `/asdf:code [path]` |
| Create spec | `/asdf:spec [name]` |
| Sync code→spec | `/asdf:sync` |
| Generate tests | `/asdf:test [feature]` |
| Create PR | `/asdf:pr [feature]` |

---
```

---

### Section 5: Next Steps

```markdown
Ready to start? Choose:
- **[status]** Show detailed current status
- **[roadmap]** Show project roadmap
- **[start]** I'm ready, exit onboard
```

---

## Adaptive Content

**If project is new (no features implemented):**
```markdown
**Getting Started:**
This project is just starting. Run these commands first:

1. `/asdf:init` — Set up project structure (if not done)
2. `/asdf:spec [first-feature]` — Create first feature spec
3. `/asdf:code [feature-path]` — Implement the feature
```

**If project has blockers:**
```markdown
**Attention Required:**
There are [N] active blockers. Consider addressing these first:
- [Blocker 1]
- [Blocker 2]
```

**If returning after long break (>7 days since handoff):**
```markdown
**Catching Up:**
Last session was [N] days ago. Here's what changed:
- [Recent change 1]
- [Recent change 2]

Read the handoff: `/asdf:status` or check `session-handoff.md`
```

---

## Timing

| Section | Target Duration |
|---------|-----------------|
| Overview | 1 minute |
| Status | 1 minute |
| Active Work | 1 minute |
| How to Start | 2 minutes |
| **Total** | **5 minutes** |

---

## Rules

- **Concise** — Max 5 minutes total read time
- **Actionable** — End with clear next steps
- **Adaptive** — Adjust content based on project state
- **Welcoming** — Friendly tone for new team members
- **Current** — Pull live data, not stale info

---

## Help

**Usage:** `/asdf:onboard`

**Arguments:** None

**Behavior:**
1. Load project overview
2. Show current status
3. Display active work
4. Provide getting started guide
5. Offer next step options

**Examples:**
- `/asdf:onboard` — Start guided tour

**Related:**
- `/asdf:status` — Detailed status
- `/asdf:roadmap` — Project phases
- `/asdf` — All commands
