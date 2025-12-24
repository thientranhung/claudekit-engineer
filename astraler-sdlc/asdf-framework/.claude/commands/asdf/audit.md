---
description: AUDIT MODE - Scan for spec health issues
argument-hint: (no args)
---

# AUDIT MODE: Spec Health Audit

Detect outdated, missing, and orphaned specs.

---

## Skills Required

- **Activate:** `maintenance` (for audit patterns)
- **Activate:** `context-loading` (for codebase scan)

---

## Workflow

### Step 1: Scan for Outdated Specs

**Definition:** Spec exists, code has changed since spec was updated.

1. For each spec in `02-domains/` and `03-features/`:
   - Get spec last-updated date (from header)
   - Find corresponding code directory
   - Check git log for changes after spec date
   - If code changed after spec → OUTDATED

```markdown
## Outdated Specs (code changed, spec not updated)

| Spec | Last Update | Code Changes |
|------|-------------|--------------|
| authentication.md | 30 days ago | 15 files changed |
| payments-v1.md | 45 days ago | 8 files changed |
```

---

### Step 2: Scan for Missing Specs

**Definition:** Code directory exists with no corresponding spec.

1. Scan `src/` or source directories
2. For each module/feature directory:
   - Check if matching spec exists in `02-domains/` or `03-features/`
   - If no spec → MISSING

```markdown
## Missing Specs (code exists, no spec)

| Code Location | Type | Suggestion |
|---------------|------|------------|
| src/services/notification/ | Domain | Create 02-domains/notification/ |
| src/utils/analytics/ | Utility | Consider documentation |
```

---

### Step 3: Scan for Orphaned Specs

**Definition:** Spec exists, but corresponding code has been deleted.

1. For each spec:
   - Check if corresponding code directory exists
   - If code deleted → ORPHANED

```markdown
## Orphaned Specs (spec exists, code deleted)

| Spec | Code Path | Status |
|------|-----------|--------|
| 03-features/241201-legacy-auth/ | src/auth-legacy/ | Deleted |
| 02-domains/old-payments/ | src/payments-v1/ | Replaced |
```

---

### Step 4: Present Audit Report

```markdown
**Spec Audit Report**

**Summary:**
| Category | Count | Action |
|----------|-------|--------|
| Outdated | [N] | Run /asdf:sync |
| Missing | [M] | Run /asdf:spec |
| Orphaned | [O] | Run /asdf:cleanup |

---

## Outdated Specs
[Table from Step 1]

## Missing Specs
[Table from Step 2]

## Orphaned Specs
[Table from Step 3]

---

**Recommendations:**
1. Run `/asdf:sync` for [N] outdated specs
2. Run `/asdf:spec` for [M] missing specs
3. Run `/asdf:cleanup` for [O] orphaned specs

---

Options:
- **[fix-all]** Address all issues automatically
- **[fix-outdated]** Sync outdated specs only
- **[fix-missing]** Create missing specs only
- **[details]** Show detailed analysis
- **[cancel]** Exit
```

---

### Step 5: Handle Options

**On fix-all:**
1. For each outdated: Run `/asdf:sync [spec]`
2. For each missing: Run `/asdf:spec [name]`
3. For each orphaned: Queue for `/asdf:cleanup`
4. Present summary when done

**On fix-outdated:**
1. For each outdated spec:
   - Run sync workflow
   - Wait for confirmation
2. Skip missing and orphaned

**On fix-missing:**
1. For each missing spec:
   - Run spec creation workflow
   - Collect references if available
2. Skip outdated and orphaned

**On details:**
```markdown
**Detailed Analysis: [spec-name]**

Last spec update: [date]
Code changes since:
- [file1.ts] modified [date]
- [file2.ts] modified [date]
- [file3.ts] added [date]

Key differences:
- API endpoint added: POST /api/new
- Schema changed: User.email now unique
- New service: NotificationService

Recommendation: Run `/asdf:sync [spec-name]`
```

---

## Staleness Thresholds

| Age | Status | Action |
|-----|--------|--------|
| <7 days | Fresh | No action |
| 7-30 days | Aging | Review recommended |
| >30 days | Stale | Sync required |

---

## Rules

- **Non-destructive** — Audit only reports, doesn't change
- **Comprehensive** — Scan all spec types
- **Actionable** — Provide specific fix commands
- **Conservative** — Mark uncertain cases for review

---

## Help

**Usage:** `/asdf:audit`

**Arguments:** None

**Behavior:**
1. Scan for outdated specs
2. Scan for missing specs
3. Scan for orphaned specs
4. Present categorized report
5. Offer fix options

**Examples:**
- `/asdf:audit` — Run full spec health audit

**Related:**
- `/asdf:sync` — Fix outdated specs
- `/asdf:spec` — Create missing specs
- `/asdf:cleanup` — Remove orphaned specs
- `/asdf:report all` — Project health report
