---
description: DESIGN MODE - Brainstorm and create feature specification with iterative refinement
argument-hint: [feature-name]
---

# DESIGN MODE: Create Feature Spec

**Feature:** $ARGUMENTS

---

## Skills Required

- **Activate:** `refinement-loop` (for iterative feedback)
- **Activate:** `spec-governance` (for templates and validation)
- **Activate:** `context-loading` (for proper hierarchy)

---

## Workflow

### Step 0: Duplicate Detection (CRITICAL)

Before drafting, check if feature already exists:

```bash
# Check for existing feature spec
ls astraler-docs/03-features/*-$ARGUMENTS/ 2>/dev/null
```

**If exists, present:**

```markdown
**Feature '$ARGUMENTS' already exists.**

Current state:
- Location: astraler-docs/03-features/[YYMMDD]-$ARGUMENTS/
- Version: v[X.Y.Z]
- Status: [Draft | Review | Approved | Implemented]
- Last updated: [YYMMDD]

Please choose:
- **[continue]** Resume editing current spec (opens refinement loop)
- **[new-version]** Create major version (v+1.0.0) for significant changes
- **[view]** Just show the current spec
- **[cancel]** Abort

Type your choice:
```

**On user choice:**
- `continue` → Load existing spec, present with refinement prompt
- `new-version` → Bump to next major version, start refinement loop
- `view` → Display current spec, no changes
- `cancel` → Abort command

**If not exists:** Proceed to Step 0.5.

---

### Step 0.5: Acquire Spec Lock (Multi-Instance)

Before editing (new or existing spec), acquire lock:

1. **Generate instance ID** (if not set):
   ```bash
   # Format: YYMMDD-HHMM-random
   echo "$(date +%y%m%d-%H%M)-$(openssl rand -hex 2)"
   ```

2. **Check for existing lock:**
   ```bash
   cat astraler-docs/04-operations/spec-locks/[feature-name].lock 2>/dev/null
   ```

3. **If locked by another instance:**
   ```markdown
   **SPEC LOCKED**

   Feature: [feature-name]
   Locked by: [other-instance-id]
   Since: [timestamp]
   Last activity: [N minutes ago]

   [STOPPED] Cannot edit while locked.

   Options:
   - Wait for lock release
   - Contact editor to coordinate
   - Type `force` to override (DANGER: may cause conflicts)
   ```

4. **If not locked or owned by this instance:**
   Create/update lock file:
   ```markdown
   # Spec Lock: [feature-name]

   Instance: [instance-id]
   Acquired: [YYMMDD HH:MM]
   Last Activity: [YYMMDD HH:MM]
   Operation: DESIGN
   ```

5. **Proceed to Step 1**

---

### Step 1: Reference Collection

Before drafting, collect references:

```markdown
**Do you have existing documents to reference for this feature?**

Categories:
- **Business** — PRD sections, user stories, acceptance criteria
- **Technical** — API specs, existing service docs
- **Data** — Related schemas, entities
- **Design** — Wireframes, mockups for this feature
- **External** — Third-party docs if integrating

Provide file path(s) or type "no" to continue.
```

For each document:
1. Parse and extract feature-relevant information
2. Report what was extracted
3. Ask for additional references
4. Repeat until user says "no more"

---

### Step 2: Load Context

1. Read `astraler-docs/01-system-core/` for global standards
2. Identify relevant domain from `astraler-docs/02-domains/`
3. Check existing features in `astraler-docs/03-features/` for patterns
4. Check `astraler-docs/04-operations/session-handoff.md` for prior work

---

### Step 3: Draft Spec (v1.0.0)

Create spec following template from `spec-governance` skill:

**Required sections:**
1. Overview + Business Value
2. Requirements (FR/NFR/Out of Scope)
3. Technical Design + User Flow Diagram (mermaid)
4. UI/UX + Wireframes
5. API Contract
6. Acceptance Criteria
7. Testing
8. Open Questions (unresolved items)
9. Changelog

**Be concrete:**
- Include API contracts with request/response examples
- Include data structures
- Include mermaid user flow diagram
- Include ASCII wireframes or design references

---

### Step 4: Present for Review

```markdown
**Draft [Feature Name] spec v1.0.0 ready for review.**

Sections:
- Overview: [summary]
- Requirements: [N] functional, [M] non-functional
- Technical Design: [key components]
- API Endpoints: [N] endpoints defined
- Acceptance Criteria: [N] criteria

Open Questions:
- [List any unresolved items]

Please choose:
- **Feedback** — Type your changes/concerns
- **Reference** — Type `reference` to add source documents
- **Confirm** — Type `confirm` to finalize
```

---

### Step 5: Refinement Loop

On **Feedback**:
1. Parse feedback for specific changes
2. Update relevant sections
3. Increment version (minor: X.Y.+1)
4. Summarize changes made
5. Re-present with refinement prompt

```markdown
**Updated to v[X.Y.Z]**

Changes made:
- [Section]: [What changed]
- [Section]: [What changed]

Please choose:
- **Feedback** — Type your changes/concerns
- **Reference** — Type `reference` to add source documents
- **Confirm** — Type `confirm` to finalize
```

On **Reference**:
1. Collect additional documents
2. Parse and extract relevant info
3. Increment version (minor: X.+1.0)
4. Update spec with new information
5. Re-present with refinement prompt

Loop continues until user types **confirm**.

---

### Step 6: Finalization

On **Confirm**:

1. Get today's date: `bash -c 'date +%y%m%d'`
2. Update version status to "Approved"
3. Create folder: `astraler-docs/03-features/YYMMDD-[feature-name]/`
4. Save `spec.md` with final version
5. Create `changelog.md` with creation entry
6. **Release spec lock:**
   ```bash
   rm astraler-docs/04-operations/spec-locks/[feature-name].lock
   ```

```markdown
**Spec Finalized**

- **Name:** [feature-name]
- **Version:** [X.Y.Z]
- **Status:** Approved
- **Location:** astraler-docs/03-features/YYMMDD-[feature-name]/spec.md
- **Lock:** Released

**Next steps:**
- Review spec one more time
- Run `/asdf:code astraler-docs/03-features/YYMMDD-[feature-name]/` to implement
```

---

## Output Location

```
astraler-docs/03-features/YYMMDD-[feature-name]/
├── spec.md        # The specification (versioned)
└── changelog.md   # Version history
```

---

## Version Format in File

```markdown
# [Feature Name]

> **Version:** 1.0.0
> **Status:** Draft | Review | Approved
> **Feature ID:** YYMMDD-feature-name
> **Domain:** [Link to domain spec]
> **Created:** YYMMDD
> **Last Updated:** YYMMDD
```

---

## Rules

- **Always collect references first** — Even if user says "no"
- **Always use refinement loop** — Never save without explicit confirm
- **Draft first, adjust later** — Don't ask too many questions upfront
- **Version track iterations** — Every change increments version
- **Be thorough** — Anticipate edge cases, dependencies, blockers
- **Include diagrams** — User flow mermaid diagram is mandatory
- **Link domains** — Reference relevant domain specs
- **List open questions** — Unresolved items at end of spec
- **No implementation** — Design only, code comes via `/asdf:code`
- **Lock before edit** — Acquire spec lock in multi-instance scenarios

---

## Help

**Usage:** `/asdf:spec [feature-name]`

**Arguments:**
- feature-name: Name of the feature to spec (e.g., "checkout", "user-auth")

**Behavior:**
1. Check for duplicate spec
2. Acquire spec lock (multi-instance)
3. Collect reference documents
4. Load context from system-core/domains
5. Draft spec with all required sections
6. Refinement loop until confirmed
7. Save to `03-features/YYMMDD-[name]/`
8. Release spec lock

**Examples:**
- `/asdf:spec checkout` — Create checkout feature spec
- `/asdf:spec user-authentication` — Create auth feature spec

**Related:**
- `/asdf:code` — Implement from spec
- `/asdf:update` — Update existing spec
- `/asdf:sync` — Sync spec after code changes
- `/asdf` — All commands
