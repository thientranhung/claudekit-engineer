# Reverse Sync Protocol

Step-by-step process for synchronizing specs with implementation reality.

---

## When to Trigger

### Automatic Triggers
- Implementation deviates from spec during `/asdf:code`
- Bug fix reveals spec was incorrect
- Refactoring changes implementation approach

### Manual Triggers
- `/asdf:sync` command
- Before `/asdf:handoff`
- During code review when discrepancy found

---

## Sync Process

### Step 1: Identify Scope

Determine what needs syncing:
- Single feature? → Sync that feature's spec
- Multiple features? → Sync each separately
- Domain-level change? → May need domain spec update too

### Step 2: Compare Code vs Spec

For each section of the spec, check:

| Spec Section | What to Compare |
|--------------|-----------------|
| Requirements | Are all FR-XXX implemented? Any extras? |
| Technical Design | Does architecture match? Key files accurate? |
| API Contract | Endpoints, request/response match reality? |
| Acceptance Criteria | Can all AC-XXX be verified in code? |

### Step 3: Document Deviations

For each deviation found:

```markdown
### [Section Name]

<!-- Original:
[Copy exact text from original spec]
-->

[Write what was actually implemented]

**Reason:** [Explain why deviation occurred]
- Better performance discovered
- Requirement changed mid-implementation
- Original approach had unforeseen issues
- Bug fix required different approach

[Reverse Synced: YYMMDD]
```

### Step 4: Update Spec

Apply changes to `spec.md`:
- Keep original in HTML comment (audit trail)
- Write new reality below comment
- Add reason and date annotation
- Update status if needed (e.g., progress %)

### Step 5: Update Changelog

Add entry to `changelog.md`:

```markdown
## YYMMDD - Reverse Sync

### Changed
- [Section]: [What changed]
- [Section]: [What changed]

### Reason
[Brief explanation of why sync was needed]

### Impact
- [Any downstream effects]
- [Related specs that may need review]
```

### Step 6: Update Operations

Update `implementation-active.md`:
```markdown
## Sync Log
- [YYMMDD] Synced [feature-name]: [brief description]
```

### Step 7: Verify

Re-read updated spec and confirm:
- [ ] Spec accurately describes current implementation
- [ ] No orphaned requirements (spec says X, code doesn't have X)
- [ ] No undocumented features (code has Y, spec doesn't mention Y)
- [ ] All changes have reasons documented

---

## Examples

### Example 1: API Endpoint Changed

**Original spec:**
```markdown
#### POST /api/users
Response: 201 Created
```

**After sync:**
```markdown
#### POST /api/users

<!-- Original: Response: 201 Created -->
Response: 200 OK with created user object

**Reason:** Frontend needed user data immediately; 201 with empty body required extra fetch.
[Reverse Synced: 241223]
```

### Example 2: Feature Simplified

**Original spec:**
```markdown
### FR-003: Multi-currency Support
Users can select from 10 supported currencies.
```

**After sync:**
```markdown
### FR-003: Currency Support

<!-- Original: Users can select from 10 supported currencies. -->
Users can select from USD, EUR, or VND (3 currencies).

**Reason:** MVP scope reduction; additional currencies deferred to v2.
[Reverse Synced: 241223]
```

### Example 3: Technical Approach Changed

**Original spec:**
```markdown
### Architecture
Using REST API with PostgreSQL database.
```

**After sync:**
```markdown
### Architecture

<!-- Original: Using REST API with PostgreSQL database. -->
Using GraphQL API with PostgreSQL database.

**Reason:** Team expertise in GraphQL; better fit for complex nested queries in this domain.
[Reverse Synced: 241223]
```

---

## Anti-Patterns

### DON'T:
- Delete original text (lose audit trail)
- Sync without documenting reason
- Batch multiple features in one sync entry
- Skip changelog update
- Leave spec in aspirational state ("we'll fix the code later")

### DO:
- Preserve original in comments
- Explain every change
- Sync feature by feature
- Update all related files
- Accept reality, document it
