---
description: DESIGN MODE - Brainstorm and create feature specification
argument-hint: [feature-name]
---

# DESIGN MODE: Create Feature Spec

**Feature:** $ARGUMENTS

---

## Workflow

1. **Activate skill:** `spec-governance` (for templates and validation rules)
2. **Activate skill:** `context-loading` (for proper hierarchy)

3. **Load Context:**
   - Read `astraler-docs/01-system-core/` for global standards
   - Identify relevant domain from `astraler-docs/02-domains/`
   - Check existing features in `astraler-docs/03-features/` for patterns

4. **Draft Spec:**
   - Create spec following template from `spec-governance` skill
   - Include: Overview, Requirements (FR/NFR), Technical Design, Acceptance Criteria
   - Be concrete — include API contracts, data structures, UI wireframes if applicable

5. **Present to Human:**
   - Show complete draft
   - Highlight key decisions and trade-offs
   - Ask: "Ready to finalize, or adjustments needed?"

6. **Iterate:**
   - Incorporate feedback
   - Re-present until human approves

7. **Save:**
   - Get today's date: `bash -c 'date +%y%m%d'`
   - Create folder: `astraler-docs/03-features/YYMMDD-[feature-name]/`
   - Save `spec.md` and `changelog.md`

---

## Output Location

```
astraler-docs/03-features/YYMMDD-[feature-name]/
├── spec.md        # The specification
└── changelog.md   # Version history
```

---

## Rules

- **Draft first, adjust later** — Don't ask too many questions upfront
- **Be thorough** — Anticipate edge cases, dependencies, blockers
- **Link domains** — Reference relevant domain specs
- **No implementation** — This is design only, code comes via `/asdf:code`
