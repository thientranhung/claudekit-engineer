# Spec Validation Rules

Rules for validating ASDF specifications.

---

## Feature Spec Validation

### Required Sections (MUST have)
- [ ] Overview with business value
- [ ] At least one Functional Requirement (FR-XXX)
- [ ] At least one Acceptance Criterion (AC-XXX)
- [ ] Implementation Progress section

### Conditional Sections
- [ ] UI/UX section — Required if feature has user interface
- [ ] API Contract section — Required if feature has API endpoints
- [ ] Technical Design — Required for features touching >2 files

### ID Formats
- Feature ID: `YYMMDD-kebab-case-name`
- Functional Req: `FR-001`, `FR-002`, ...
- Non-Functional Req: `NFR-001`, `NFR-002`, ...
- Acceptance Criteria: `AC-001`, `AC-002`, ...

### Quality Checks
- [ ] No orphan requirements (every FR has related AC)
- [ ] Acceptance criteria are testable (Given/When/Then format preferred)
- [ ] Out of Scope explicitly defined
- [ ] Status reflects actual state (not aspirational)

---

## Domain Spec Validation

### Required Sections (MUST have)
- [ ] Domain Purpose with bounded context
- [ ] At least one Business Rule with ID
- [ ] At least one Entity definition
- [ ] Error Codes section

### ID Formats
- Business Rule ID: `[DOMAIN]-001` (e.g., `AUTH-001`, `PAY-001`)
- Error Code: `[DOMAIN]_001` (e.g., `AUTH_001`)

### Quality Checks
- [ ] Business rules have enforcement description
- [ ] Entities use TypeScript interface format
- [ ] Integration points document both inbound and outbound
- [ ] Related features linked

---

## Common Validation Rules

### Metadata Block
Every spec MUST have metadata block at top:
```markdown
> **[Field]:** Value
> **Status:** [Valid status]
> **Created:** YYMMDD
> **Last Updated:** YYMMDD
```

### Valid Statuses
- **Feature:** `Planned` | `In Progress (X%)` | `Implemented` | `Deprecated`
- **Domain:** `Active` | `Planned` | `Deprecated`

### Changelog
- Every spec MUST have changelog section
- Entries in reverse chronological order (newest first)
- Format: `### YYMMDD - [Title]`

### Cross-References
- Domain specs link to related features
- Feature specs link to parent domain
- Use relative paths: `../02-domains/auth/auth-domain.md`

---

## Validation Errors

### Critical (Block approval)
- Missing required sections
- Invalid ID format
- No acceptance criteria
- Status mismatch with reality

### Warning (Flag but allow)
- Missing optional sections
- Incomplete changelog
- Missing cross-references

### Info (Suggestions)
- Long sections that could be split
- Missing diagrams for complex flows
- NFRs without measurable targets

---

## Automated Checks

When validating specs, verify:

```javascript
// Pseudo-code for validation logic
function validateFeatureSpec(content) {
  const errors = [];

  // Check metadata
  if (!content.includes('> **Feature ID:**')) {
    errors.push('CRITICAL: Missing Feature ID');
  }

  // Check required sections
  const requiredSections = [
    '## 1. Overview',
    '## 2. Requirements',
    '## 6. Acceptance Criteria'
  ];

  for (const section of requiredSections) {
    if (!content.includes(section)) {
      errors.push(`CRITICAL: Missing ${section}`);
    }
  }

  // Check ID formats
  const frPattern = /FR-\d{3}/;
  if (!frPattern.test(content)) {
    errors.push('CRITICAL: No functional requirements with proper ID');
  }

  return errors;
}
```
