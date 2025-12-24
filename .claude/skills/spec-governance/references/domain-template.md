# Domain Spec Template

Use this template when creating domain specifications.

---

```markdown
# [Domain Name] Domain

> **Version:** 1.0.0
> **Status:** Active | Planned | Deprecated
> **Created:** YYMMDD
> **Last Updated:** YYMMDD

---

## 1. Domain Purpose

[2-3 sentences describing this domain's responsibility]

### Bounded Context
- **Owns:** [What this domain is responsible for]
- **Does Not Own:** [What belongs to other domains]

---

## 2. Business Rules

| ID | Rule | Enforcement |
|----|------|-------------|
| [DOMAIN]-001 | [Rule description] | [How enforced] |
| [DOMAIN]-002 | [Rule description] | [How enforced] |

### Rule Details

#### [DOMAIN]-001: [Rule Name]
- **Description:** [Detailed explanation]
- **Rationale:** [Why this rule exists]
- **Exceptions:** [When rule doesn't apply]

---

## 3. Entities

### [EntityName]

```typescript
interface EntityName {
  id: string;
  field1: string;
  field2: number;
  status: EntityStatus;
  createdAt: Date;
  updatedAt: Date;
}

type EntityStatus = 'active' | 'inactive' | 'pending';
```

### Relationships
- `EntityA` has many `EntityB`
- `EntityB` belongs to `EntityA`

---

## 4. State Machine

### [Entity] Lifecycle

```
[Initial] → [State1] → [State2] → [Final]
              ↓
           [Error]
```

| Transition | From | To | Trigger | Validation |
|------------|------|-----|---------|------------|
| create | - | pending | User action | [rules] |
| activate | pending | active | Admin approval | [rules] |

---

## 5. Integration Points

### Inbound (Other domains → This domain)
| Source | Event/API | Purpose |
|--------|-----------|---------|
| [Domain] | [Event name] | [What it triggers] |

### Outbound (This domain → Other domains)
| Target | Event/API | Purpose |
|--------|-----------|---------|
| [Domain] | [Event name] | [What it notifies] |

---

## 6. API Contracts

### Internal APIs

#### GET /api/[domain]/[resource]
- **Purpose:** [Description]
- **Auth:** Required/Public
- **Response:** [EntityName][]

#### POST /api/[domain]/[resource]
- **Purpose:** [Description]
- **Auth:** Required
- **Body:** CreateEntityDto
- **Response:** EntityName

### Events Published

| Event | Payload | When |
|-------|---------|------|
| [domain].[entity].created | { id, ...fields } | [Trigger] |
| [domain].[entity].updated | { id, changes } | [Trigger] |

---

## 7. Error Codes

| Code | Name | Description | Resolution |
|------|------|-------------|------------|
| [DOMAIN]_001 | [ErrorName] | [When this occurs] | [How to fix] |
| [DOMAIN]_002 | [ErrorName] | [When this occurs] | [How to fix] |

---

## 8. Dependencies

### Internal
- **[Domain]** — [Why needed]

### External
- **[Service/Library]** — [Why needed]

---

## Related Features

- [YYMMDD-feature-name](../03-features/YYMMDD-feature-name/spec.md)
```
