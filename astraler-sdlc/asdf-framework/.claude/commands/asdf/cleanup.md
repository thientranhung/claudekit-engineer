---
description: CLEANUP MODE - Remove unused or orphaned specs
argument-hint: (no args)
---

# CLEANUP MODE: Remove Unused Specs

Identify and remove orphaned, deprecated, and empty specs.

---

## Skills Required

- **Activate:** `maintenance` (for cleanup patterns)

---

## Workflow

### Step 1: Identify Cleanup Candidates

**Category 1: Orphaned Specs**
- Spec exists but corresponding code deleted
- No references from other specs

**Category 2: Deprecated Specs**
- Marked with `> **Status:** Deprecated` in header
- Has deprecation date in changelog

**Category 3: Empty Files**
- File exists but contains only template/placeholder
- No substantive content added

---

### Step 2: Scan and Categorize

```markdown
**Cleanup Analysis**

## Orphaned Specs (safe to delete)
| Spec | Reason | Last Modified |
|------|--------|---------------|
| 03-features/241201-legacy-auth/ | Code deleted | 241215 |
| 02-domains/old-payments/ | Replaced by payments-v2 | 241210 |

## Deprecated Specs (marked for removal)
| Spec | Deprecated On | Replacement |
|------|---------------|-------------|
| 03-features/241205-temp-fix/ | 241220 | 241220-permanent-fix |

## Empty Files (never populated)
| File | Created | Content |
|------|---------|---------|
| 01-system-core/02-standards/performance-slas.md | 241201 | Template only |

---

**Total items:** [N] specs identified for cleanup
```

---

### Step 3: Present Options

```markdown
Options:
- **[delete-orphaned]** Remove orphaned specs only
- **[delete-deprecated]** Remove deprecated specs only
- **[delete-empty]** Remove empty files only
- **[delete-all]** Remove all identified items
- **[review]** Review each item individually
- **[cancel]** Exit without changes
```

---

### Step 4: Handle Options

**On delete-orphaned:**
1. For each orphaned spec:
   - Move to `04-operations/archived/` (not delete)
   - Log action in cleanup-log.md
2. Report count deleted

**On delete-deprecated:**
1. For each deprecated spec:
   - Verify deprecation date >30 days ago
   - Move to `04-operations/archived/`
   - Log action
2. Report count deleted

**On delete-empty:**
1. For each empty file:
   - Confirm truly empty (not WIP)
   - Delete file
   - Log action
2. Report count deleted

**On delete-all:**
1. Process all categories
2. Report total cleanup

**On review:**
```markdown
**Reviewing: [spec-path]**

**Contents:**
- spec.md (v1.0.0, created [date])
- changelog.md

**Related code:** [DELETED | REPLACED | MOVED]
**Last referenced:** [date] from [file]
**Dependencies:** [None | List]

Delete this spec?
- **[yes]** Move to archive
- **[no]** Keep this spec
- **[skip]** Skip and continue
- **[cancel]** Stop review
```

---

### Step 5: Archive vs Delete

**Archive (default for orphaned/deprecated):**
1. Create `04-operations/archived/[YYMMDD]-[spec-name]/`
2. Move all spec files
3. Add tombstone file with reason

**Delete (for empty files only):**
1. Remove file completely
2. Log in cleanup-log.md

---

### Step 6: Report Results

```markdown
**Cleanup Complete**

**Archived:**
- [N] orphaned specs → 04-operations/archived/
- [M] deprecated specs → 04-operations/archived/

**Deleted:**
- [O] empty files removed

**Log:** 04-operations/cleanup-log.md

**Rollback:**
To restore archived specs, move from:
`04-operations/archived/[spec-name]/` → original location
```

---

## Cleanup Log Format

```markdown
# Cleanup Log

## 251224 - Cleanup Session

| Action | Spec | Reason | Archived To |
|--------|------|--------|-------------|
| Archived | 241201-legacy-auth | Code deleted | archived/241201-legacy-auth |
| Archived | 241205-temp-fix | Deprecated | archived/241205-temp-fix |
| Deleted | performance-slas.md | Empty file | — |

---
```

---

## Safety Rules

- **Archive, don't delete** — Orphaned and deprecated specs are archived, not deleted
- **Confirm on review** — Individual confirmation for each item in review mode
- **Skip uncertain** — If status unclear, skip and note for manual review
- **Log everything** — All actions logged for audit trail
- **Rollback possible** — Archived specs can be restored

---

## Help

**Usage:** `/asdf:cleanup`

**Arguments:** None

**Behavior:**
1. Scan for orphaned specs
2. Scan for deprecated specs
3. Scan for empty files
4. Present categorized list
5. Offer delete options
6. Archive/delete and log

**Examples:**
- `/asdf:cleanup` — Run cleanup analysis

**Related:**
- `/asdf:audit` — Identify issues first
- `/asdf:sync` — Update outdated specs
- `/asdf:status` — Check project state
