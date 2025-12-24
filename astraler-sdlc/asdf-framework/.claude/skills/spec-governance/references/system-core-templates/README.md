# System-Core Templates

Templates for all system-core documentation files with required depth and diagram specifications.

## Template Index (13 Files)

| # | File | Template | Required Diagram |
|---|------|----------|------------------|
| 1 | master-map.md | `master-map-template.md` | System architecture (flowchart) |
| 2 | tech-stack.md | `tech-stack-template.md` | Stack diagram (flowchart) |
| 3 | data-architecture.md | `data-architecture-template.md` | ERD (erDiagram) |
| 4 | infrastructure.md | `infrastructure-template.md` | Deployment topology (flowchart) |
| 5 | coding-standards.md | `coding-standards-template.md` | None |
| 6 | api-standards.md | `api-standards-template.md` | None |
| 7 | testing-strategy.md | `testing-strategy-template.md` | Testing pyramid (flowchart) |
| 8 | performance-slas.md | `performance-slas-template.md` | Cache tiers (flowchart) |
| 9 | ui-ux-design-system.md | `ui-ux-design-system-template.md` | Color palette (flowchart) |
| 10 | component-library.md | `component-library-template.md` | Component hierarchy (flowchart) |
| 11 | security-policy.md | `security-policy-template.md` | Security architecture (flowchart) |
| 12 | decision-log.md | `decision-log-template.md` | Decision process (flowchart) |
| 13 | glossary.md | `glossary-template.md` | None |

---

## Diagram Requirements Summary

| Document | Diagram Type | Required? |
|----------|--------------|-----------|
| master-map.md | System architecture (flowchart) | **YES** |
| data-architecture.md | ERD (erDiagram) | **YES** |
| infrastructure.md | Deployment topology (flowchart) | **YES** |
| Domain specs | ERD (erDiagram) | **YES** |
| Feature specs | User flow (flowchart) | **YES** |

---

## Standard Template Structure

All system-core files MUST include these sections:

```markdown
# [Document Name]

> **Version:** 1.0.0
> **Status:** Draft | Review | Approved
> **Last Updated:** YYMMDD

## 1. Overview
[Brief description]

## 2. Visual Diagram (if required)
[Mermaid diagram]

## 3. Detailed Specifications
[Deep technical content]

## 4. Rules & Constraints
[Business rules, technical constraints]

## 5. Dependencies
[Links to related docs]

## 6. Open Questions
[Items needing clarification]

## 7. Changelog
| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | YYMMDD | Initial draft |
```

---

## Usage

Each template file contains:
1. Template metadata and usage instructions
2. Complete markdown template with all required sections
3. Validation rules checklist

To use a template:
1. Copy the markdown code block from the template file
2. Replace placeholder values with project-specific content
3. Run through validation checklist before finalizing

---

## Validation Checklist

Before finalizing any system-core file:

- [ ] Version header present (v1.0.0 format)
- [ ] Status field present (Draft/Review/Approved)
- [ ] Last Updated date set
- [ ] Required mermaid diagram included (if applicable)
- [ ] All sections populated with meaningful content
- [ ] Rules & constraints documented
- [ ] Dependencies linked
- [ ] Open questions listed
- [ ] Changelog has initial entry
