---
description: Create session handoff notes for continuity
argument-hint: (no args)
---

# Session Handoff

Capture session state for seamless continuation in next session.

---

## Workflow

### Step 0: Release Feature Lock

**If a feature is currently locked by this session:**

1. Check `04-operations/locks/` for locks owned by this session
2. For each lock:
   - Delete the lock file
   - Update execution file status in `04-operations/active/[feature].md` to "Paused"
   - Update `implementation-active.md` Feature Execution Status (remove lock indicator)

```markdown
**Locks Released:**

| Feature | Previous Status | Now |
|---------|-----------------|-----|
| [feature-name] | In Progress | Paused (Available) |

Next session can continue with: `/asdf:code [feature]`
```

---

### Step 1: Gather Session Context

1. **Gather session context:**
   - What was the goal this session?
   - What was accomplished?
   - What's in progress (incomplete)?
   - What blockers were encountered?
   - What decisions were made?

### Step 2: Update implementation-active.md
   ```markdown
   # Implementation Active

   > **Last Updated:** YYMMDD HH:MM
   > **Session:** [Brief description]

   ## Current Task
   - **Feature:** [feature path]
   - **Status:** [In Progress / Blocked / Paused]
   - **Progress:** [X]%

   ## Files Changed This Session
   - [file1.ts] — [what changed]
   - [file2.ts] — [what changed]

   ## Blockers
   - [List or "None"]

   ## Uncommitted Work
   - [List any uncommitted changes]
   ```

3. **Update session-handoff.md with enhanced template:**
   ```markdown
   # Session Handoff

   > **Date:** YYMMDD
   > **Duration:** ~[X] hours
   > **Session ID:** [instance-id or "solo"]

   ---

   ## Mental Context (Critical for Reload)

   **What I was trying to do:**
   [1-2 sentences describing high-level goal]

   **Why this approach:**
   [Key architectural/design decisions and rationale]

   **What I learned:**
   - [Insight 1 about the codebase]
   - [Insight 2 about dependencies]
   - [Gotcha or unexpected behavior discovered]

   ---

   ## Technical Context

   **Completed This Session:**
   - [Task 1] — Done
   - [Task 2] — Done

   **In Progress:**
   - [Task] — [X]% complete
     - **Next step:** [specific action]
     - **Blocker:** [if any]

   **File Changes Summary:**
   | File | Change Type | Description |
   |------|-------------|-------------|
   | [file1.ts] | Modified | [what changed] |
   | [file2.ts] | Added | [purpose] |
   | [file3.ts] | Deleted | [why removed] |

   ---

   ## Decisions Made

   | Decision | Options Considered | Chosen | Rationale |
   |----------|-------------------|--------|-----------|
   | [decision1] | A, B | A | [why] |
   | [decision2] | X, Y, Z | Y | [why] |

   ---

   ## Blockers & Questions

   | Blocker/Question | Status | Action Needed |
   |------------------|--------|---------------|
   | [blocker1] | Waiting | [what's needed] |
   | [question1] | Open | [who to ask] |

   ---

   ## Environment Context

   **Branch:** [current branch name]
   **Uncommitted Changes:** [Yes/No — list if yes]
   **Tests Status:** [Passing/Failing — count if failing]
   **Build Status:** [OK/Broken]

   ---

   ## Next Session Should

   1. **[FIRST]** [Most important action with specific command]
   2. [Second priority]
   3. [Third priority]

   ---

   ## Quick Start
   ```bash
   # Branch and status
   git checkout [branch]
   git status

   # Resume from here:
   /asdf:code [feature-path]
   # OR
   [specific command to continue]
   ```

   ---

   ## Context Files to Load
   - `astraler-docs/03-features/[feature]/spec.md`
   - `astraler-docs/04-operations/active/[feature].md`
   - [Any other relevant files]
   ```

4. **Trigger related updates:**
   - Run `/asdf:sync` if any specs need updating
   - Run `/asdf:status` to refresh project heartbeat

5. **Verify completeness:**
   - All changes committed? (remind if not)
   - Specs up to date?
   - Blockers documented?

6. **Report:**
   - Summary of handoff notes created
   - Remind human of any pending actions

---

## When to Run

- **Always** before ending a session
- After significant milestone completion
- Before switching to different feature
- When pausing work for extended period

---

## Rules

- **Capture everything** — Next session has no memory of this one
- **Be specific** — "Fix auth bug" is useless; "Fix JWT expiry check in auth/middleware.ts:47" is useful
- **Include quick start** — Reduce context-loading time for next session
- **Honest about blockers** — Don't hide problems, document them
- **Mental context matters** — Decisions and rationale are hardest to reload

---

## Help

**Usage:** `/asdf:handoff`

**Arguments:** None

**Behavior:**
1. Release any feature locks held by this session
2. Gather session context (completed, in-progress, blockers)
3. Update implementation-active.md
4. Create enhanced session-handoff.md
5. Trigger sync if specs outdated
6. Verify all changes committed

**Output Sections:**
- Mental Context (critical for reload)
- Technical Context (tasks, files)
- Decisions Made (with rationale)
- Blockers & Questions
- Environment Context (branch, tests)
- Quick Start commands

**Examples:**
- `/asdf:handoff` — Create handoff before ending session

**Related:**
- `/asdf:status` — Quick status update
- `/asdf:onboard` — For next session orientation
- `/asdf` — All commands
