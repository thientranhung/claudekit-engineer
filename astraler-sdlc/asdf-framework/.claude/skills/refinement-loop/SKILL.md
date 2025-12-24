# Refinement Loop Skill

Manages iterative human-AI feedback loops for spec refinement until explicit confirmation.

## When to Use

Activate this skill when:
- Creating or updating specifications (`/asdf:init`, `/asdf:spec`)
- Any document generation requiring human approval
- Iterative refinement of technical designs

## Core Protocol

### Feedback Loop States

```
DRAFT → REVIEW → [FEEDBACK | REFERENCE | CONFIRM]
                      ↓           ↓          ↓
                  UPDATE →    COLLECT →   FINALIZE
                      ↓           ↓
                   REVIEW ←───────┘
```

### User Response Types

| Response | Trigger | Action |
|----------|---------|--------|
| `[feedback]` | Free text input | Incorporate changes, increment version, re-present |
| `reference` | User types "reference" | Collect additional source documents, update draft |
| `confirm` | User types "confirm" | Finalize spec, save to file system |

---

## Refinement Prompt Template

After generating ANY draft, ALWAYS present:

```markdown
**Draft [Document Name] v[X.Y.Z] ready for review.**

Please choose:
- **Feedback** — Type your changes/concerns
- **Reference** — Type `reference` to add source documents
- **Confirm** — Type `confirm` to finalize

Location: `[file-path]`
```

---

## Version Management

### Increment Rules

| Change Type | Version Bump | Example |
|-------------|--------------|---------|
| Initial draft | 1.0.0 | First creation |
| Minor feedback | X.Y.+1 | Wording changes, clarifications |
| New reference added | X.+1.0 | Structure changes from new info |
| Major restructure | +1.0.0 | Fundamental approach change |

### Version in File

Every spec file MUST include version header:

```markdown
> **Version:** 1.0.0
> **Status:** Draft | Review | Approved
> **Last Updated:** YYMMDD
```

---

## Feedback Incorporation

### On Receiving Feedback

1. Parse feedback for specific changes requested
2. Update relevant sections
3. Increment version (minor bump)
4. Summarize changes made
5. Re-present with refinement prompt

### Change Summary Format

```markdown
**Updated to v[X.Y.Z]**

Changes made:
- [Section N]: [What changed]
- [Section M]: [What changed]

[Refinement Prompt]
```

---

## Reference Collection

Load: `references/reference-collection.md`

When user requests reference collection, execute the reference gathering protocol before updating the draft.

---

## Confirmation Protocol

### On "confirm"

1. Update version status to "Approved"
2. Update `Last Updated` date
3. Save to file system
4. Report finalization

### Finalization Report

```markdown
**Spec Finalized**

- **Name:** [feature-name]
- **Version:** [X.Y.Z]
- **Status:** Approved
- **Location:** [file-path]

**Next steps:**
- Review spec one more time
- Run `/asdf:code [path]` to implement
```

---

## Rules

- **Never skip confirmation** — Always wait for explicit "confirm"
- **Track all iterations** — Version numbers tell the story
- **Summarize changes** — User should know what changed
- **Be patient** — Loop as many times as needed
- **Preserve context** — Each iteration builds on previous
