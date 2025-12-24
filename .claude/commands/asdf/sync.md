---
description: SYNC MODE - Reverse sync code changes back to specs
argument-hint: [feature-path] (optional)
---

# SYNC MODE: Reverse Sync

**Target:** $ARGUMENTS (or scan all active features if blank)

---

## Workflow

1. **Activate skill:** `reverse-sync`

2. **Identify Scope:**
   - If path provided: sync that specific feature
   - If blank: check `astraler-docs/04-operations/implementation-active.md` for active work

3. **Compare Code vs Spec:**
   For each feature in scope:
   - Read current spec from `astraler-docs/03-features/[feature]/spec.md`
   - Analyze actual implementation in codebase
   - Identify deviations:
     - **Additions** — Code has features not in spec
     - **Changes** — Implementation differs from spec
     - **Removals** — Spec features not implemented

4. **Document Each Deviation:**
   Format:
   ```markdown
   ### [Section Name]

   <!-- Original: [what spec said] -->
   [What was actually implemented]

   **Reason:** [Why the change was necessary]
   [Reverse Synced: YYMMDD]
   ```

5. **Update Spec:**
   - Modify spec to reflect actual implementation
   - Preserve original text in HTML comments for audit trail
   - Add date annotation

6. **Update Changelog:**
   - Add entry to `astraler-docs/03-features/[feature]/changelog.md`
   - Format:
     ```markdown
     ## [YYMMDD] Reverse Sync

     - [List of changes made]
     - Reason: [Brief explanation]
     ```

7. **Verify:**
   - Re-read updated spec
   - Confirm it accurately describes current implementation
   - Report: "Sync complete. [N] sections updated."

---

## When to Trigger

- After implementation deviates from spec (called from `/asdf:code`)
- When discovering code-spec drift during review
- Before session handoff to ensure docs are current
- After bug fixes that reveal spec inaccuracies

---

## Rules

- **Truth over convenience** — Document what IS, not what should be
- **Preserve history** — Use HTML comments for original text
- **Explain why** — Every change needs a reason
- **Atomic updates** — Sync one feature at a time for clarity
