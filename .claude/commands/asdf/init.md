---
description: Initialize ASDF structure - supports new projects, existing code, or requirements docs
argument-hint: (interactive)
---

# Initialize ASDF

Single entry point for all project starting scenarios.

---

## Step 1: Determine Starting Point

Ask the user:

> **What's your starting point?**
> - **A) New project** — Fresh start, I'll describe the vision
> - **B) Existing codebase** — I have code, need specs generated
> - **C) Requirements docs** — I have BRD/PRD/specs to parse
> - **D) Mixed** — I have both code and docs

---

## Step 2: Execute Based on Answer

### A) New Project (Greenfield)

1. **Ask vision questions:**
   - What problem does this solve?
   - Who are the users?
   - What's the core functionality?
   - Any tech stack preferences?

2. **Draft system-core docs:**
   - Architecture files from vision answers
   - Standards files with sensible defaults
   - Design placeholders

3. **Identify initial domains:**
   - Based on core functionality, suggest domain breakdown
   - Create domain folders with starter specs

4. **Suggest first feature:**
   - Recommend starting with MVP feature
   - Guide to run `/asdf:spec [feature]`

---

### B) Existing Codebase (Brownfield)

1. **Ask for codebase path** (or use current directory)

2. **Scan and analyze:**
   - Detect tech stack from package.json, requirements.txt, go.mod, etc.
   - Identify project structure patterns
   - Find key modules/components
   - Analyze existing tests for testing patterns
   - Detect API patterns from routes/endpoints
   - Identify UI frameworks and component patterns
   - Look for existing documentation

3. **Generate system-core (inferred from code):**

   **01-architecture/**
   - `master-map.md` — From directory structure, entry points, module dependencies
   - `tech-stack.md` — From detected dependencies and runtime
   - `data-architecture.md` — From database schemas, models, ORMs detected
   - `infrastructure.md` — From Dockerfile, docker-compose, CI/CD configs, cloud configs

   **02-standards/**
   - `coding-standards.md` — From linter configs, code patterns observed
   - `api-standards.md` — From API routes, response patterns, error handling
   - `testing-strategy.md` — From test files structure, frameworks used
   - `performance-slas.md` — From any monitoring configs, or placeholder if none

   **03-design/**
   - `ui-ux-design-system.md` — From CSS/style files, design tokens, theme configs
   - `component-library.md` — From UI component inventory

   **04-governance/**
   - `security-policy.md` — From auth patterns, env handling, security headers
   - `decision-log.md` — Placeholder (no history to infer)
   - `glossary.md` — From domain terms found in code/comments

4. **Generate domain specs:**
   - Group related files into logical domains
   - Create domain spec for each with inferred business rules

5. **Generate feature inventory:**
   - List detected features as potential specs
   - Ask user to confirm/adjust

6. **Create feature specs:**
   - For each confirmed feature, create `spec.md`
   - Mark as "Implemented" status
   - Document actual behavior (not aspirational)

---

### C) Requirements Docs

1. **Ask for docs path** (PRD, BRD, specs, etc.)

2. **Parse and extract:**
   - Identify requirements (functional/non-functional)
   - Extract user stories
   - Find acceptance criteria
   - Detect domain concepts

3. **Generate system-core:**
   - Architecture from system requirements
   - Standards from NFRs
   - Design from UI requirements
   - Governance from compliance/security requirements

4. **Generate domain specs:**
   - Group requirements by domain
   - Create domain spec for each

5. **Generate feature specs:**
   - Convert user stories/requirements into feature specs
   - Map acceptance criteria
   - Mark as "Planned" status

6. **Identify gaps:**
   - List areas where requirements are unclear
   - Suggest clarification questions

---

### D) Mixed (Code + Docs)

1. **Execute B) first** — Scan existing code
2. **Execute C) second** — Parse requirements docs
3. **Reconcile:**
   - Compare code reality vs documented requirements
   - Identify:
     - Implemented but undocumented features
     - Documented but unimplemented requirements
     - Mismatches between docs and code
4. **Generate unified specs** reflecting actual state
5. **Create gap analysis** for human review

---

## Step 3: Create Complete ASDF Structure

Create full structure with all files:

```
astraler-docs/
├── 01-system-core/
│   ├── 01-architecture/
│   │   ├── master-map.md           # System overview, component relationships
│   │   ├── tech-stack.md           # Languages, frameworks, tools
│   │   ├── data-architecture.md    # Database design, data flow
│   │   └── infrastructure.md       # Deployment, hosting, DevOps
│   ├── 02-standards/
│   │   ├── coding-standards.md     # Code style, patterns, conventions
│   │   ├── api-standards.md        # API design, versioning, errors
│   │   ├── testing-strategy.md     # Test types, coverage, tools
│   │   └── performance-slas.md     # Response times, availability targets
│   ├── 03-design/
│   │   ├── ui-ux-design-system.md  # Colors, typography, spacing
│   │   └── component-library.md    # Reusable UI components
│   ├── 04-governance/
│   │   ├── security-policy.md      # Auth, data protection, compliance
│   │   ├── decision-log.md         # ADRs, key decisions with rationale
│   │   └── glossary.md             # Domain terms, abbreviations
│   └── project-status.md           # Version, status, active work summary
├── 02-domains/
│   └── [domain-name]/
│       └── [domain]-domain.md      # Business rules, entities, APIs
├── 03-features/
│   └── YYMMDD-[feature-name]/
│       ├── spec.md                 # Feature specification
│       └── changelog.md            # Feature version history
└── 04-operations/
    ├── implementation-active.md    # Current task, progress, blockers
    ├── session-handoff.md          # Last session state, next steps
    └── changelog/
        └── YYMMDD-[event].md       # Project-level changes
```

**File Population Rules:**
- **Greenfield (A):** Placeholders with section headers, filled via Q&A
- **Brownfield (B):** AI infers and generates content from code analysis
- **Requirements (C):** AI extracts and maps from source docs
- **Mixed (D):** AI reconciles both sources, flags conflicts

---

## Step 4: Report & Next Steps

Show:
- Created structure summary
- Generated specs count (by status: Planned/Implemented)
- Files that are placeholders vs populated
- Gaps/questions identified

Suggest next actions:
- Review generated system-core for accuracy
- Fill placeholder files with actual decisions
- Run `/asdf:spec [feature]` for new features
- Run `/asdf:code [path]` when ready to implement

---

## Rules

- **Complete structure always** — All folders and files created, even if placeholders
- **Ask, don't assume** — Let user choose their path
- **Show, then adjust** — Generate drafts, iterate on feedback
- **Truth over aspiration** — Document what IS (brownfield), what SHOULD BE (greenfield)
- **Single entry point** — User shouldn't need multiple init commands
