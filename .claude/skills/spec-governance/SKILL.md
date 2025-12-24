---
name: spec-governance
description: Validate specs, enforce standards, provide templates for ASDF specifications.
---

# Spec Governance

Ensure all specifications follow ASDF standards and templates.

## When to Use

- Creating new feature specs (`/asdf:spec`)
- Creating domain specs (`/asdf:init`)
- Validating existing specs
- Reviewing spec quality

## Templates

### Feature Spec Template
Load: `references/feature-template.md`

### Domain Spec Template
Load: `references/domain-template.md`

## Validation Rules
Load: `references/validation-rules.md`

## Quick Reference

### Feature Spec Required Sections
1. Overview + Business Value
2. Requirements (FR/NFR/Out of Scope)
3. Technical Design
4. UI/UX (if applicable)
5. API Contract (if applicable)
6. Acceptance Criteria
7. Implementation Progress

### Domain Spec Required Sections
1. Domain Purpose
2. Business Rules (with IDs)
3. Entities (TypeScript interfaces)
4. State Machines (if applicable)
5. Integration Points
6. API Contracts
7. Error Codes

### Naming Conventions
- Feature folders: `YYMMDD-feature-name` (kebab-case)
- Domain folders: `domain-name` (kebab-case)
- Requirement IDs: `FR-001`, `NFR-001`, `AC-001`
- Business Rule IDs: `[DOMAIN]-001` (e.g., `AUTH-001`)

## Quality Checklist

Before finalizing any spec:
- [ ] All required sections present
- [ ] Requirements have unique IDs
- [ ] Acceptance criteria are testable
- [ ] Technical design matches requirements
- [ ] Dependencies clearly stated
- [ ] Out of scope explicitly defined
