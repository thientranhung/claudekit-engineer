---
description: REPORT MODE - Generate progress reports for features or project
argument-hint: [feature|all]
---

# REPORT MODE: Progress Report

**Scope:** $ARGUMENTS

---

## Skills Required

- **Activate:** `context-loading` (for loading feature data)

---

## Workflow

### Determine Report Type

- If $ARGUMENTS is a feature name → Feature Report
- If $ARGUMENTS is "all" or blank → Project Report

---

## Feature Report

### Step 1: Load Feature Data

1. Find spec in `astraler-docs/03-features/*-$ARGUMENTS/`
2. Load execution file from `04-operations/active/$ARGUMENTS.md`
3. Load test results (if available)
4. Load changelog

---

### Step 2: Calculate Metrics

- **Progress:** Based on implementation status table
- **Test Coverage:** From testing section or test results
- **Time:** Started date to now, estimate completion

---

### Step 3: Generate Feature Report

```markdown
**Feature Report: [Feature Name]**

## Status
- **Spec:** v[X.Y.Z] ([Draft|Approved|Implemented])
- **Implementation:** [X]% complete
- **Tests:** [N]/[M] passing

## Progress
| Task | Status | Assignee |
|------|--------|----------|
| [Task 1] | Done | [Instance] |
| [Task 2] | Active | [Instance] |
| [Task 3] | Pending | — |

## Test Coverage
- Unit: [X]%
- Integration: [Y]%
- E2E: [Z]%

## Blockers
- [Blocker description]

## Time Tracking
- Started: [YYMMDD]
- Estimated completion: [YYMMDD]
- Days elapsed: [N]

## Deviations
| Section | Status |
|---------|--------|
| [section] | [Synced|Pending] |

## Files Modified
- [N] files total
- Key files: [list top 5]
```

---

## Project Report

### Step 1: Aggregate Data

1. Scan all features in `03-features/`
2. Load `04-operations/implementation-active.md`
3. Load `04-operations/tech-debt.md` (if exists)
4. Load `04-operations/roadmap.md`
5. Count specs by status

---

### Step 2: Calculate Health Indicators

- **Spec freshness:** Last updated within 7 days = Fresh
- **Tech debt:** Count by priority
- **Blocker count:** From active features
- **Test coverage:** Average across features

---

### Step 3: Generate Project Report

```markdown
**Project Health Report**

## Overview
- **Features:** [N] total ([Done], [Active], [Planned])
- **Specs:** [M] files
- **Test Coverage:** [X]% average
- **Phase:** [Current phase from roadmap]

## Feature Status
| Feature | Status | Progress | Assignee |
|---------|--------|----------|----------|
| [feature1] | Done | 100% | — |
| [feature2] | Active | 75% | [Instance] |
| [feature3] | Active | 40% | [Instance] |
| [feature4] | Planned | 0% | — |

## Health Indicators
| Indicator | Value | Status |
|-----------|-------|--------|
| Specs up-to-date | [N]/[M] | [Green/Yellow/Red] |
| Tech debt items | [N] | [Green/Yellow/Red] |
| Active blockers | [N] | [Green/Yellow/Red] |
| Test coverage | [X]% | [Green/Yellow/Red] |

## Tech Debt Summary
- High priority: [N] items ([X]h estimated)
- Medium priority: [M] items
- Low priority: [O] items

## Blockers
| Feature | Blocker | Since |
|---------|---------|-------|
| [feature] | [description] | [YYMMDD] |

## Recommendations
1. [Actionable recommendation 1]
2. [Actionable recommendation 2]
3. [Actionable recommendation 3]

## Next Priorities
1. [From roadmap P0]
2. [From roadmap P1]
3. [From roadmap P1]
```

---

## Health Indicator Thresholds

| Indicator | Green | Yellow | Red |
|-----------|-------|--------|-----|
| Specs up-to-date | >80% | 50-80% | <50% |
| Tech debt | <5 items | 5-15 items | >15 items |
| Blockers | 0 | 1-2 | >2 |
| Test coverage | >70% | 40-70% | <40% |

---

## Rules

- **Data-driven** — Only report what's in the docs
- **Actionable** — Include recommendations
- **Visual** — Use tables for scannability
- **Honest** — Flag problems, don't hide them

---

## Help

**Usage:** `/asdf:report [feature|all]`

**Arguments:**
- feature: Name of specific feature to report on
- all: Generate project-wide report (default if blank)

**Behavior:**
1. Determine report scope
2. Load relevant data
3. Calculate metrics
4. Generate formatted report

**Examples:**
- `/asdf:report checkout` — Report on checkout feature
- `/asdf:report all` — Full project health report
- `/asdf:report` — Same as 'all'

**Related:**
- `/asdf:status` — Quick status snapshot
- `/asdf:audit` — Spec health check
- `/asdf:roadmap` — Phase planning
