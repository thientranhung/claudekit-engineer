---
description: Initialize ASDF structure - supports new projects, existing code, or requirements docs
argument-hint: (interactive)
---

# Initialize ASDF

Single entry point for all project starting scenarios with iterative refinement.

---

## Skills Required

- **Activate:** `refinement-loop` (for iterative feedback)
- **Activate:** `spec-governance` (for templates and validation)
- **Activate:** `context-loading` (for proper hierarchy)

---

## Step 0: Incomplete Detection Scan (CRITICAL)

Before anything else, check if ASDF structure already exists:

```bash
# Check if astraler-docs/ exists
ls astraler-docs/ 2>/dev/null
```

**If not exists:** Proceed to Step 1.

**If exists:** Run comprehensive scan:

### Scan All Expected Components

```markdown
**ASDF structure exists. Scanning completeness...**

┌─────────────────────────────────────┬──────────┬─────────┐
│ Component                           │ Status   │ % Done  │
├─────────────────────────────────────┼──────────┼─────────┤
│ 01-system-core/01-architecture/     │ [STATUS] │ [XX]%   │
│   ├── master-map.md                 │ [STATUS] │         │
│   ├── tech-stack.md                 │ [STATUS] │         │
│   ├── data-architecture.md          │ [STATUS] │         │
│   └── infrastructure.md             │ [STATUS] │         │
├─────────────────────────────────────┼──────────┼─────────┤
│ 01-system-core/02-standards/        │ [STATUS] │ [XX]%   │
│   ├── coding-standards.md           │ [STATUS] │         │
│   ├── api-standards.md              │ [STATUS] │         │
│   ├── testing-strategy.md           │ [STATUS] │         │
│   └── performance-slas.md           │ [STATUS] │         │
├─────────────────────────────────────┼──────────┼─────────┤
│ 01-system-core/03-design/           │ [STATUS] │ [XX]%   │
│   ├── ui-ux-design-system.md        │ [STATUS] │         │
│   └── component-library.md          │ [STATUS] │         │
├─────────────────────────────────────┼──────────┼─────────┤
│ 01-system-core/04-governance/       │ [STATUS] │ [XX]%   │
│   ├── security-policy.md            │ [STATUS] │         │
│   ├── decision-log.md               │ [STATUS] │         │
│   └── glossary.md                   │ [STATUS] │         │
├─────────────────────────────────────┼──────────┼─────────┤
│ 01-system-core/project-status.md    │ [STATUS] │         │
├─────────────────────────────────────┼──────────┼─────────┤
│ 02-domains/                         │ [STATUS] │ [N] domains │
├─────────────────────────────────────┼──────────┼─────────┤
│ 03-features/                        │ [STATUS] │ [M] specs   │
├─────────────────────────────────────┼──────────┼─────────┤
│ 04-operations/                      │ [STATUS] │ [XX]%   │
│   ├── implementation-active.md      │ [STATUS] │         │
│   ├── session-handoff.md            │ [STATUS] │         │
│   └── roadmap.md                    │ [STATUS] │         │
└─────────────────────────────────────┴──────────┴─────────┘

**Overall Completion:** [XX]%
```

### Status Definitions

| Status | Meaning |
|--------|---------|
| Done | File exists with substantive content (>100 chars, has sections) |
| Partial | File exists but only template/placeholder content |
| Empty | File exists but <50 chars or blank |
| Missing | File does not exist |

### Calculate Completion

```python
# Completion scoring
REQUIRED_FILES = 14  # 13 system-core + project-status.md
populated = count(files with status == Done)
partial = count(files with status == Partial)
completion = (populated + partial * 0.5) / REQUIRED_FILES * 100
```

### Present Scan Results

**If 100% complete:**
```markdown
**ASDF structure is COMPLETE.**

- Created: [date from earliest file]
- Last updated: [date from most recent file]
- Domains: [N] defined
- Features: [M] specs

Please choose:
- **[update]** Update existing docs → Run `/asdf:update`
- **[spec]** Add new feature → Run `/asdf:spec`
- **[cancel]** Abort
```

**If incomplete (<100%):**
```markdown
**ASDF structure exists but INCOMPLETE.**

[Show scan table above]

**Missing/Incomplete:**
- [List specific files with Missing/Empty/Partial status]

Please choose:
- **[continue]** Resume setup, fill gaps only (recommended)
- **[override]** Start fresh, replace existing (WARNING: destructive)
- **[cancel]** Abort
```

### On User Choice

**On continue:**
1. Track which files need work (Missing, Empty, Partial)
2. Skip to Step 2 (Reference Collection)
3. In Step 3, ONLY generate content for incomplete files
4. Preserve existing content in Done files

**On override:**
1. Show second warning:
   ```markdown
   **WARNING: This will DELETE all existing ASDF content.**

   - [N] system-core files will be replaced
   - [M] domain specs will be deleted
   - [P] feature specs will be deleted

   Type "CONFIRM" to proceed, or any other key to cancel.
   ```
2. If confirmed → Delete astraler-docs/, proceed to Step 1
3. If not → Return to choice prompt

**On cancel:** Abort command

---

## Step 1: Determine Starting Point

Ask the user:

> **What's your starting point?**
> - **A) New project** — Fresh start, I'll describe the vision
> - **B) Existing codebase** — I have code, need specs generated
> - **C) Requirements docs** — I have BRD/PRD/specs to parse
> - **D) Mixed** — I have both code and docs

---

## Step 2: Reference Collection (All Paths)

Before generating anything, collect references:

```markdown
**Do you have existing documents to reference?**

Categories:
- **Business** — PRD, BRD, requirements, user stories
- **Technical** — API specs, architecture docs, schemas
- **Data** — ERD, database schemas, data dictionaries
- **Design** — Wireframes, mockups, design system
- **External** — Third-party API docs, competitor analysis

Provide file path(s) or type "no" to continue.
```

For each document provided:
1. Parse and extract relevant information
2. Report what was extracted
3. Ask for additional references
4. Repeat until user says "no more"

---

## Step 3: Execute Based on Starting Point

### A) New Project (Greenfield)

1. **Ask vision questions:**
   - What problem does this solve?
   - Who are the users?
   - What's the core functionality?
   - Any tech stack preferences?

2. **Draft system-core docs** (v1.0.0) from vision + references

3. **Identify initial domains** from core functionality

4. **Present for refinement:**
   ```markdown
   **Draft system-core v1.0.0 ready for review.**

   Created:
   - [N] architecture files
   - [M] standards files
   - [P] design files
   - [Q] governance files
   - [R] domains identified

   Please choose:
   - **Feedback** — Type your changes
   - **Reference** — Type `reference` to add source documents
   - **Confirm** — Type `confirm` to finalize
   ```

5. **Loop until confirmed**

---

### B) Existing Codebase (Brownfield)

1. **Ask for codebase path** (or use current directory)

2. **Scan and analyze:**
   - Detect tech stack from package.json, requirements.txt, etc.
   - Identify project structure patterns
   - Find key modules/components
   - Analyze existing tests, API patterns, UI frameworks

3. **Draft system-core** (v1.0.0) inferred from code:

   **01-architecture/**
   - `master-map.md` — From directory structure, entry points
   - `tech-stack.md` — From dependencies and runtime
   - `data-architecture.md` — From database schemas, models
   - `infrastructure.md` — From Docker, CI/CD configs

   **02-standards/**
   - `coding-standards.md` — From linter configs, patterns
   - `api-standards.md` — From API routes, error handling
   - `testing-strategy.md` — From test files structure
   - `performance-slas.md` — From monitoring configs

   **03-design/**
   - `ui-ux-design-system.md` — From CSS/style files
   - `component-library.md` — From UI components

   **04-governance/**
   - `security-policy.md` — From auth patterns
   - `decision-log.md` — Placeholder
   - `glossary.md` — From domain terms

4. **Draft domain specs** from code analysis

5. **Present for refinement** (same prompt as A)

6. **Loop until confirmed**

---

### C) Requirements Docs

1. **Ask for docs path** (PRD, BRD, specs, etc.)

2. **Parse and extract:**
   - Requirements (functional/non-functional)
   - User stories
   - Acceptance criteria
   - Domain concepts
   - Tech stack decisions

3. **Draft system-core** (v1.0.0) from requirements:
   - Architecture from system requirements
   - Standards from NFRs
   - Design from UI requirements
   - Governance from compliance/security

4. **Draft domain specs** from grouped requirements

5. **Draft feature specs** from user stories/FRs

6. **Present for refinement:**
   ```markdown
   **Draft ASDF structure v1.0.0 ready for review.**

   Extracted:
   - [N] functional requirements
   - [M] non-functional requirements
   - [P] domains identified
   - [Q] feature specs created

   Gaps identified:
   - [List unclear areas]

   Please choose:
   - **Feedback** — Type your changes
   - **Reference** — Type `reference` to add source documents
   - **Confirm** — Type `confirm` to finalize
   ```

7. **Loop until confirmed**

---

### D) Mixed (Code + Docs)

1. **Execute B)** — Scan existing code
2. **Execute C)** — Parse requirements docs
3. **Reconcile:**
   - Compare code reality vs documented requirements
   - Identify mismatches, gaps, undocumented features
4. **Draft unified specs** reflecting actual state
5. **Present reconciliation for refinement:**
   ```markdown
   **Draft reconciled ASDF structure v1.0.0 ready for review.**

   Reconciliation:
   - [N] features in code AND docs (aligned)
   - [M] features in code only (undocumented)
   - [P] features in docs only (not implemented)
   - [Q] mismatches found

   Please choose:
   - **Feedback** — Type your changes
   - **Reference** — Type `reference` to add source documents
   - **Confirm** — Type `confirm` to finalize
   ```

6. **Loop until confirmed**

---

## Step 4: Create Complete ASDF Structure (After Confirmation)

Create full structure with all files:

```
astraler-docs/
├── 01-system-core/
│   ├── 01-architecture/
│   │   ├── master-map.md           # System overview, mermaid diagrams
│   │   ├── tech-stack.md           # Languages, frameworks, tools
│   │   ├── data-architecture.md    # ERD diagram, database design
│   │   └── infrastructure.md       # Deployment topology diagram
│   ├── 02-standards/
│   │   ├── coding-standards.md     # Code style, patterns
│   │   ├── api-standards.md        # API design, versioning
│   │   ├── testing-strategy.md     # Test types, coverage
│   │   └── performance-slas.md     # Response times, availability
│   ├── 03-design/
│   │   ├── ui-ux-design-system.md  # Colors, typography
│   │   └── component-library.md    # Reusable components
│   ├── 04-governance/
│   │   ├── security-policy.md      # Auth, data protection
│   │   ├── decision-log.md         # ADRs
│   │   └── glossary.md             # Domain terms
│   └── project-status.md           # Version, status
├── 02-domains/
│   └── [domain-name]/
│       └── [domain]-domain.md      # ERD diagram, business rules
├── 03-features/
│   └── YYMMDD-[feature-name]/
│       ├── spec.md                 # User flow diagram
│       └── changelog.md
└── 04-operations/
    ├── implementation-active.md
    ├── session-handoff.md
    └── changelog/
        └── YYMMDD-[event].md
```

**All files MUST include:**
- Version header (v1.0.0)
- Status (Draft | Review | Approved)
- Last Updated date
- Mermaid diagrams where required (see spec-governance)

---

## Step 5: Finalization Report

After user confirms:

```markdown
**ASDF Structure Finalized**

- **Version:** [X.Y.Z]
- **Location:** astraler-docs/
- **Status:** Approved

**Created:**
- [N] system-core files
- [M] domain specs
- [P] feature specs
- [Q] operations files

**Next steps:**
- Review generated specs for accuracy
- Run `/asdf:spec [feature]` for new features
- Run `/asdf:code [path]` when ready to implement
```

---

## Rules

- **Always collect references first** — Before generating any content
- **Always use refinement loop** — Never finalize without explicit confirm
- **Version everything** — Start at v1.0.0, increment on changes
- **Include diagrams** — Mermaid diagrams are mandatory in system-core
- **Ask, don't assume** — Let user choose their path
- **Show, then adjust** — Draft first, iterate on feedback

---

## Help

**Usage:** `/asdf:init`

**Arguments:** None (interactive)

**Behavior:**
1. Scan for existing ASDF structure
2. If exists: Show completeness scan, offer continue/override/cancel
3. If new: Ask starting point (A/B/C/D)
4. Collect reference documents
5. Generate structure based on path chosen
6. Present for refinement loop
7. Finalize on confirm

**Examples:**
- `/asdf:init` — Start interactive init
- Choose A for new project, B for existing code, C for requirements, D for mixed

**Related:**
- `/asdf:spec` — Create individual feature spec
- `/asdf:update` — Update existing component
- `/asdf` — All commands
