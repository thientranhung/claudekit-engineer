# Spec Validation Rules

Rules for validating ASDF specifications. **v2.0** — Includes version management, mermaid diagrams, and open questions requirements.

---

## Universal Requirements (ALL Specs)

### 1. Version Header (CRITICAL)

Every spec MUST have version header at top:
```markdown
> **Version:** 1.0.0
> **Status:** Draft | Review | Approved
> **Last Updated:** YYMMDD
```

**Validation:**
- Version format: `X.Y.Z` (semantic versioning)
- Status: One of `Draft`, `Review`, `Approved`
- Date: `YYMMDD` format

### 2. Open Questions Section (CRITICAL)

Every spec MUST include:
```markdown
## Open Questions

| # | Question | Impact | Status |
|---|----------|--------|--------|
| 1 | [Question text] | [Impact description] | Open/Resolved |
```

**Validation:**
- Section must exist (can be empty table)
- Status: `Open` or `Resolved`

### 3. Changelog Section (CRITICAL)

Every spec MUST include:
```markdown
## Changelog

### YYMMDD - v1.0.0 - Initial Draft
- Created spec
- [Additional items]
```

**Validation:**
- At least one changelog entry
- Version matches header version
- Date in `YYMMDD` format

---

## Mermaid Diagram Requirements (CRITICAL)

### Required Diagrams by Document Type

| Document | Diagram Type | Mermaid Syntax |
|----------|--------------|----------------|
| master-map.md | System architecture | `flowchart TB` |
| data-architecture.md | ERD | `erDiagram` |
| infrastructure.md | Deployment topology | `flowchart TB` |
| Domain specs | Entity relationships | `erDiagram` |
| Feature specs | User flow | `flowchart TD` |

**Validation:**
- Document type must have corresponding mermaid block
- Diagram must be meaningful (not placeholder)

### Diagram Validation Checks

```javascript
// Required diagram patterns
const diagramRequirements = {
  'master-map.md': /```mermaid\s*flowchart/,
  'data-architecture.md': /```mermaid\s*erDiagram/,
  'infrastructure.md': /```mermaid\s*flowchart/,
  'domain.md': /```mermaid\s*erDiagram/,
  'spec.md': /```mermaid\s*flowchart/
};
```

---

## Feature Spec Validation

### Required Sections (MUST have)
- [ ] Version header with version, status, last updated
- [ ] Overview with business value
- [ ] **User Flow Diagram (mermaid flowchart)** ← NEW v2
- [ ] At least one Functional Requirement (FR-XXX)
- [ ] At least one Acceptance Criterion (AC-XXX)
- [ ] **Open Questions section** ← NEW v2
- [ ] **Changelog section** ← NEW v2
- [ ] Implementation Progress section

### Conditional Sections
- [ ] UI/UX section — Required if feature has user interface
- [ ] API Contract section — Required if feature has API endpoints
- [ ] Technical Design — Required for features touching >2 files
- [ ] Data Flow Diagram — Required for complex features

### ID Formats
| ID Type | Format | Example |
|---------|--------|---------|
| Feature ID | YYMMDD-kebab-case | 251224-user-auth |
| Functional Req | FR-XXX | FR-001 |
| Non-Functional | NFR-XXX | NFR-001 |
| Acceptance | AC-XXX | AC-001 |

### Quality Checks
- [ ] No orphan requirements (every FR has related AC)
- [ ] Acceptance criteria are testable (Given/When/Then format)
- [ ] Out of Scope explicitly defined
- [ ] Status reflects actual state
- [ ] NFRs have measurable targets

---

## Domain Spec Validation

### Required Sections (MUST have)
- [ ] Version header with version, status, last updated
- [ ] Overview with responsibilities and boundaries
- [ ] **ERD Diagram (mermaid erDiagram)** ← NEW v2
- [ ] At least one Entity definition with field types
- [ ] At least one Business Rule with ID
- [ ] Domain Events section
- [ ] API Contracts section
- [ ] Error Codes section
- [ ] **Open Questions section** ← NEW v2
- [ ] **Changelog section** ← NEW v2

### ID Formats
| ID Type | Format | Example |
|---------|--------|---------|
| Business Rule | [DOMAIN]-XXX | AUTH-001 |
| Error Code | [DOMAIN]_XXX | AUTH_001 |

### ERD Requirements
The ERD must show:
- [ ] All entities in domain
- [ ] Primary keys marked (PK)
- [ ] Foreign keys marked (FK)
- [ ] Relationship cardinality
- [ ] Key field types

### Quality Checks
- [ ] Business rules have enforcement description
- [ ] Entities have complete field definitions
- [ ] Integration points document inbound AND outbound
- [ ] Related features linked
- [ ] State machine for stateful entities

---

## System-Core Validation

### Required Diagrams

| File | Diagram | What to Show |
|------|---------|--------------|
| master-map.md | System architecture | All services, communication flows |
| data-architecture.md | Full ERD | All core entities, relationships |
| infrastructure.md | Deployment topology | Compute, storage, networking |

### Required Depth
- [ ] Not placeholder content
- [ ] Technical details present
- [ ] Rules & constraints documented
- [ ] Dependencies linked
- [ ] Rationale for decisions

---

## Valid Statuses

| Spec Type | Valid Statuses |
|-----------|----------------|
| All specs | `Draft`, `Review`, `Approved` |
| Feature (legacy) | `Planned`, `In Progress (X%)`, `Implemented`, `Deprecated` |
| Domain (legacy) | `Active`, `Planned`, `Deprecated` |

---

## Validation Error Levels

### CRITICAL (Block finalization)
- Missing version header
- Missing required mermaid diagram
- Missing Open Questions section
- Missing Changelog section
- Missing required sections
- Invalid ID format
- No acceptance criteria

### WARNING (Flag but allow)
- Missing optional sections
- Incomplete changelog
- Missing cross-references
- NFRs without measurable targets

### INFO (Suggestions)
- Long sections that could be split
- Missing diagrams for complex flows
- Empty Open Questions table

---

## Automated Validation

```javascript
function validateSpec(content, type) {
  const errors = [];
  const warnings = [];

  // CRITICAL: Version header
  if (!content.match(/>\s*\*\*Version:\*\*\s*\d+\.\d+\.\d+/)) {
    errors.push('CRITICAL: Missing or invalid Version header');
  }

  // CRITICAL: Status
  if (!content.match(/>\s*\*\*Status:\*\*\s*(Draft|Review|Approved)/)) {
    errors.push('CRITICAL: Missing or invalid Status');
  }

  // CRITICAL: Open Questions
  if (!content.includes('## Open Questions') &&
      !content.includes('## 9. Open Questions') &&
      !content.includes('## 10. Open Questions')) {
    errors.push('CRITICAL: Missing Open Questions section');
  }

  // CRITICAL: Changelog
  if (!content.includes('## Changelog') &&
      !content.includes('## 10. Changelog') &&
      !content.includes('## 12. Changelog')) {
    errors.push('CRITICAL: Missing Changelog section');
  }

  // Type-specific: Mermaid diagrams
  if (type === 'feature' && !content.includes('```mermaid')) {
    errors.push('CRITICAL: Missing user flow diagram');
  }
  if (type === 'domain' && !content.includes('erDiagram')) {
    errors.push('CRITICAL: Missing ERD diagram');
  }

  return { errors, warnings };
}
```

---

## Cross-Reference Rules

- Domain specs MUST link to related features
- Feature specs MUST link to parent domain
- System-core docs MUST link to each other where relevant
- Use relative paths: `../02-domains/auth/auth-domain.md`
