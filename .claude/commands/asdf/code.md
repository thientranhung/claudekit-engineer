---
description: EXECUTE MODE - Implement code from specification
argument-hint: [spec-path]
---

# EXECUTE MODE: Implement from Spec

**Spec Path:** $ARGUMENTS

---

## Workflow

1. **Activate skill:** `context-loading`

2. **Read Spec Completely:**
   - Load the spec at provided path (or find by feature name in `astraler-docs/03-features/`)
   - Understand ALL requirements before writing any code
   - Note acceptance criteria — these define "done"

3. **Load Supporting Context:**
   - System standards from `astraler-docs/01-system-core/02-standards/`
   - Relevant domain logic from `astraler-docs/02-domains/`
   - Check `astraler-docs/04-operations/session-handoff.md` for prior work

4. **Implement:**
   - Follow spec precisely — no more, no less
   - Use patterns from existing codebase
   - YAGNI: Don't add features not in spec

5. **Handle Deviations:**
   If implementation requires change from spec:
   - **STOP** — Do not silently deviate
   - Explain: "Spec says X, but I need to do Y because Z"
   - Options:
     a) Update spec now (minor clarification)
     b) Continue and run `/asdf:sync` after (tracked deviation)
     c) Wait for human decision (blocking issue)

6. **Update Progress:**
   - Update `astraler-docs/04-operations/implementation-active.md`:
     - Current task
     - Progress percentage
     - Blockers (if any)
     - Files changed

7. **Verify Completion:**
   - Check each acceptance criterion
   - Run relevant tests
   - Report: "Implementation complete. AC [list] verified."

---

## Rules

- **Spec is law** — Deviation requires explicit acknowledgment
- **Read before write** — No coding until spec is understood
- **Track everything** — Update implementation-active.md as you go
- **YAGNI** — If it's not in spec, don't build it
- **Honest reporting** — Flag blockers immediately, don't hide problems
