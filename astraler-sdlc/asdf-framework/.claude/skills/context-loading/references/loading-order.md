# Context Loading Order

Detailed guide for loading ASDF documentation hierarchy.

---

## The Four Tiers

### Tier 1: System Core (Global DNA)

**Path:** `astraler-docs/01-system-core/`

**Priority:** Highest — Rules here apply to EVERYTHING

**Contents:**
```
01-system-core/
├── 01-architecture/
│   ├── master-map.md         # System overview, component map
│   ├── tech-stack.md         # Languages, frameworks, tools
│   ├── data-architecture.md  # Database design, data flow
│   └── infrastructure.md     # Deployment, hosting
├── 02-standards/
│   ├── coding-standards.md   # Code style, patterns
│   ├── api-standards.md      # API design rules
│   ├── testing-strategy.md   # Test approach
│   └── performance-slas.md   # Performance targets
├── 03-design/
│   ├── ui-ux-design-system.md # Visual standards
│   └── component-library.md   # Reusable components
├── 04-governance/
│   ├── security-policy.md    # Security rules
│   ├── decision-log.md       # Key decisions
│   └── glossary.md           # Terms
└── project-status.md         # Current state
```

**Load When:** Always (every task)

**Key Files for Most Tasks:**
- `coding-standards.md` — How to write code
- `api-standards.md` — How to design APIs
- `tech-stack.md` — What technologies to use

---

### Tier 2: Domains (Business Logic)

**Path:** `astraler-docs/02-domains/[domain-name]/`

**Priority:** High — Domain rules for specific business areas

**Contents:**
```
02-domains/
├── authentication/
│   └── auth-domain.md
├── payments/
│   └── payments-domain.md
├── orders/
│   └── orders-domain.md
└── notifications/
    └── notifications-domain.md
```

**Load When:** Working on features that touch that domain

**Key Info:**
- Business rules (AUTH-001, PAY-001, etc.)
- Entity definitions
- Integration points
- Domain-specific error codes

---

### Tier 3: Features (Actionable Specs)

**Path:** `astraler-docs/03-features/YYMMDD-feature-name/`

**Priority:** Medium — Specific feature requirements

**Contents:**
```
03-features/
└── YYMMDD-feature-name/
    ├── spec.md       # The specification
    └── changelog.md  # Version history
```

**Load When:** Implementing or modifying specific feature

**Key Info:**
- Functional requirements (FR-XXX)
- Acceptance criteria (AC-XXX)
- Technical design
- API contracts

---

### Tier 4: Operations (Session State)

**Path:** `astraler-docs/04-operations/`

**Priority:** Context — Current execution state

**Contents:**
```
04-operations/
├── implementation-active.md  # What's being worked on
├── session-handoff.md        # Last session state
└── changelog/
    └── YYMMDD-event.md       # Project-level changes
```

**Load When:** Starting/resuming work, ending session

**Key Info:**
- Current task and progress
- Blockers
- Session continuity notes

---

## Loading Patterns

### Pattern 1: Starting Fresh Session

```
1. Read session-handoff.md → Understand last state
2. Read implementation-active.md → Check blockers
3. Read relevant feature spec → Get requirements
4. Read domain spec → Understand business rules
5. Skim system-core standards → Refresh global rules
```

### Pattern 2: New Feature Development

```
1. Read system-core fully → Understand all constraints
2. Identify relevant domain(s)
3. Read domain spec(s)
4. Read similar existing features → Learn patterns
5. Create new feature spec
```

### Pattern 3: Bug Fix

```
1. Read feature spec where bug exists
2. Read domain for business rules
3. Read coding-standards for patterns
4. Fix bug
5. Update spec if behavior changes
```

### Pattern 4: Code Review

```
1. Read coding-standards → Know what to check
2. Read api-standards → If API involved
3. Read feature spec → Verify implementation matches
4. Review code against spec
```

---

## Conflict Resolution

When specs conflict:

| Conflict | Resolution |
|----------|------------|
| System Core vs Domain | System Core wins |
| System Core vs Feature | System Core wins |
| Domain vs Feature | Domain wins (feature must comply) |
| Old spec vs New code | Trigger reverse sync |

**Example:**
- System Core says: "All APIs return JSON"
- Feature spec says: "Return XML for legacy support"
- **Resolution:** Feature spec is invalid. Either update system core to allow exceptions, or feature must use JSON.

---

## Loading Efficiency

### Don't Over-Load
- If implementing auth feature, don't load payments domain
- If fixing typo, don't read entire system-core

### Do Load Enough
- If feature touches two domains, load both
- If unsure about standards, load them

### Cache Mental Model
- After loading system-core once, you know the rules
- Only re-read if you need specific details
