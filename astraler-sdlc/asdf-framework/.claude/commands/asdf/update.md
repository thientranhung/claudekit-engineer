---
description: UPDATE MODE - Update specific component with impact analysis
argument-hint: [component-name]
---

# UPDATE MODE: Component Update

**Component:** $ARGUMENTS

---

## Skills Required

- **Activate:** `impact-analysis` (for affected files detection)
- **Activate:** `refinement-loop` (for iterative updates)

---

## Workflow

### Step 1: Locate Component

1. Parse $ARGUMENTS to find target file
2. Search order:
   - `astraler-docs/01-system-core/**/$ARGUMENTS.md`
   - `astraler-docs/02-domains/$ARGUMENTS/domain.md`
   - `astraler-docs/03-features/*-$ARGUMENTS/spec.md`

**If not found:**
```markdown
**Component Not Found**

Cannot find "$ARGUMENTS" in:
- 01-system-core/
- 02-domains/
- 03-features/

Available components:
[List first 10 matching files]

Try: `/asdf:update [exact-name]`
```

---

### Step 2: Check Spec Lock

**If component is a spec (02-domains/ or 03-features/):**

1. Check `04-operations/spec-locks/[component].lock`

**If locked:**
```markdown
**SPEC LOCKED**

Component: [component]
Locked by: [instance-id]
Since: [timestamp]
Last activity: [time ago]

[STOPPED] Cannot edit while locked.

Options:
1. Wait for lock release
2. Contact editor to coordinate

To check status: /asdf:status [component]
```

**If not locked:**
1. Create lock file
2. Proceed to Step 3

---

### Step 3: Show Current Content

```markdown
**Current [component].md (v[X.Y.Z])**

[Summary of key sections - max 20 lines]

---

What would you like to update?
- Type your changes (natural language OK)
- Or type 'view' to see full file
- Or type 'cancel' to abort
```

---

### Step 4: Apply Changes

1. Parse user input for intended changes
2. Update relevant sections
3. Increment version (minor bump)
4. **Run impact analysis:**
   - Scan for files that reference this component
   - Identify potentially affected specs

---

### Step 5: Present Update Preview

```markdown
**Updated [component].md to v[X.Y.Z]**

**Changes made:**
| Section | Before | After |
|---------|--------|-------|
| [section] | [old] | [new] |

**Impact Analysis:**
These changes may affect:
- [file1.md] (references [section])
- [file2.md] (depends on [entity])

Options:
- **[confirm]** Save changes to [component].md only
- **[feedback]** Adjust further
- **[impact]** Update all affected files
- **[cancel]** Discard changes
```

---

### Step 6: Handle Options

**On confirm:**
1. Save updated component
2. Release spec lock
3. Update changelog

**On feedback:**
1. Accept additional input
2. Return to Step 4

**On impact:**
1. Save current component
2. For each affected file:
   - Open in refinement loop
   - Present suggested changes
   - Wait for confirm/feedback
3. Release spec lock when all done

**On cancel:**
1. Discard changes
2. Release spec lock

---

### Step 7: Report

```markdown
**Update Complete**

- **Component:** [component]
- **Version:** v[X.Y.Z-1] → v[X.Y.Z]
- **Files Updated:** [N]

**Changes:**
- [component].md: [summary]
- [affected1].md: [summary] (if impact chosen)

Lock released.
```

---

## Updateable Components

| Location | Examples |
|----------|----------|
| 01-system-core/01-architecture/ | master-map, tech-stack, data-architecture |
| 01-system-core/02-standards/ | code-standards, api-standards |
| 01-system-core/03-design/ | design-system, ui-ux |
| 02-domains/ | auth, payments, notifications |
| 03-features/ | Any feature spec |

---

## Rules

- **Lock before edit** — Acquire spec lock for domains/features
- **Impact aware** — Always analyze downstream effects
- **Version track** — Increment version on save
- **Cascading option** — Offer to update affected files
- **Confirm required** — Never save without explicit confirmation

---

## Help

**Usage:** `/asdf:update [component]`

**Arguments:**
- component: Name of file to update (without .md extension)

**Behavior:**
1. Locate component in docs structure
2. Acquire lock (for specs)
3. Show current content
4. Accept changes
5. Run impact analysis
6. Present preview with options
7. Apply and release lock

**Examples:**
- `/asdf:update tech-stack` — Update technology stack
- `/asdf:update auth` — Update auth domain spec
- `/asdf:update checkout` — Update checkout feature spec

**Related:**
- `/asdf:init` — Initial setup
- `/asdf:spec` — Create new spec
- `/asdf:sync` — Sync after code changes
