---
description: EXECUTE MODE - Implement code from specification with deviation handling
argument-hint: [spec-path]
---

# EXECUTE MODE: Implement from Spec

**Spec Path:** $ARGUMENTS

---

## Skills Required

- **Activate:** `context-loading` (for proper hierarchy)
- **Activate:** `refinement-loop` (for deviation handling)

---

## Workflow

### Step 1: Read Spec Completely

1. Load the spec at provided path (or find by feature name in `astraler-docs/03-features/`)
2. Understand ALL requirements before writing any code
3. Note acceptance criteria — these define "done"
4. Verify spec status is "Approved" (warn if still "Draft" or "Review")

---

### Step 2: Load Supporting Context

1. System standards from `astraler-docs/01-system-core/02-standards/`
2. Relevant domain logic from `astraler-docs/02-domains/`
3. Check `astraler-docs/04-operations/session-handoff.md` for prior work

---

### Step 3: Implement

1. Follow spec precisely — no more, no less
2. Use patterns from existing codebase
3. YAGNI: Don't add features not in spec

---

### Step 4: Handle Deviations

If implementation requires change from spec:

1. **STOP** — Do not silently deviate
2. **Explain** the deviation:

```markdown
**Deviation Detected**

- **Spec says:** [original requirement]
- **Implementation needs:** [what's actually required]
- **Reason:** [why the change is necessary]

Options:
- **A) Update spec now** — Minor clarification, proceed with change
- **B) Continue + sync later** — Tracked deviation, run `/asdf:sync` after
- **C) Wait for decision** — Blocking issue, need human input

Which option? (A/B/C)
```

3. **Track choice:**
   - **A**: Update spec inline, note in changelog, continue
   - **B**: Continue, mark deviation in `implementation-active.md`, sync later
   - **C**: Wait for human response before proceeding

---

### Step 5: Update Progress

Update `astraler-docs/04-operations/implementation-active.md`:

```markdown
# Implementation Active

> **Last Updated:** YYMMDD
> **Session:** [Feature being implemented]

## Current Task

- **Feature:** [feature-name]
- **Spec:** [path to spec]
- **Status:** In Progress
- **Progress:** [X]%

## Files Modified

- `path/to/file.ts` — [What changed]
- `path/to/file.tsx` — [What changed]

## Deviations

| Spec Section | Deviation | Reason | Resolution |
|--------------|-----------|--------|------------|
| [Section] | [What changed] | [Why] | A/B/C |

## Blockers

- [Any blockers]

## Next Actions

- [What's next]
```

---

### Step 6: Verify Completion

1. Check each acceptance criterion from spec
2. Run relevant tests
3. Present completion report:

```markdown
**Implementation Complete**

- **Feature:** [feature-name]
- **Spec Version:** [X.Y.Z]
- **Files Modified:** [N]

**Acceptance Criteria:**
- [x] AC-001: [criteria] — Verified
- [x] AC-002: [criteria] — Verified
- [ ] AC-003: [criteria] — Blocked by [reason]

**Deviations:** [N] (resolved via A/B/C)

**Next steps:**
- Run `/asdf:sync` if deviations exist
- Update `session-handoff.md` before ending session
```

---

## Deviation Handling Rules

| Deviation Type | Action | Example |
|----------------|--------|---------|
| Minor clarification | Option A | "Field is VARCHAR(50) not VARCHAR(255)" |
| Better approach | Option B | "Using bulk insert instead of loop" |
| Requirement conflict | Option C | "Spec says X but dependency requires Y" |
| Missing info | Option C | "Spec doesn't specify timeout behavior" |

---

## Rules

- **Spec is law** — Deviation requires explicit acknowledgment
- **Read before write** — No coding until spec is understood
- **Track everything** — Update implementation-active.md as you go
- **YAGNI** — If it's not in spec, don't build it
- **Honest reporting** — Flag blockers immediately, don't hide problems
- **Version awareness** — Note which spec version you're implementing against
- **Deviation choices** — Always give user A/B/C options for deviations
