# Feature Spec Template

Use this template when creating new feature specifications.

---

```markdown
# [Feature Name]

> **Feature ID:** YYMMDD-feature-name
> **Status:** Planned | In Progress (X%) | Implemented
> **Domain:** [Link to domain spec]
> **Created:** YYMMDD
> **Last Updated:** YYMMDD

---

## 1. Overview

[2-3 sentences describing what this feature does]

### Business Value
- [Why this feature matters]
- [What problem it solves]
- [Who benefits]

---

## 2. Requirements

### 2.1 Functional Requirements (MUST)

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-001 | [Requirement description] | High |
| FR-002 | [Requirement description] | Medium |

### 2.2 Non-Functional Requirements (SHOULD)

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performance | [e.g., < 200ms response] |
| NFR-002 | Availability | [e.g., 99.9% uptime] |

### 2.3 Out of Scope

- [Explicitly state what this feature does NOT include]
- [Prevents scope creep]

---

## 3. Technical Design

### Architecture
[Describe how this feature fits into the system]

### Key Files
- `path/to/file.ts` — [Purpose]
- `path/to/component.tsx` — [Purpose]

### Data Flow
```
[User Action] → [Component] → [API] → [Service] → [Database]
```

### Dependencies
- [Internal: other features/domains this depends on]
- [External: third-party services/libraries]

---

## 4. UI/UX

### Wireframes
[ASCII wireframe or link to design file]

### User Flow
1. User does X
2. System responds with Y
3. User sees Z

### States
- Loading state
- Empty state
- Error state
- Success state

---

## 5. API Contract

### Endpoints

#### POST /api/[resource]
**Request:**
```json
{
  "field": "type"
}
```

**Response (200):**
```json
{
  "id": "string",
  "created": "timestamp"
}
```

**Errors:**
| Code | Message | When |
|------|---------|------|
| 400 | Invalid input | [condition] |
| 401 | Unauthorized | [condition] |

---

## 6. Acceptance Criteria

| ID | Criteria | Testable? |
|----|----------|-----------|
| AC-001 | Given [context], when [action], then [result] | Yes |
| AC-002 | Given [context], when [action], then [result] | Yes |

---

## 7. Testing

### Unit Tests
- [ ] [Component/function to test]

### Integration Tests
- [ ] [API endpoint to test]

### E2E Tests
- [ ] [User flow to test]

---

## 8. Blockers

| Blocker | Status | Owner |
|---------|--------|-------|
| [Description] | Open/Resolved | [Name] |

---

## 9. Implementation Progress

| Task | Status | Notes |
|------|--------|-------|
| [Task 1] | Done/In Progress/Pending | |
| [Task 2] | Done/In Progress/Pending | |

---

## 10. Changelog

### YYMMDD - Initial Draft
- Created spec
- Defined requirements

### YYMMDD - [Update Title]
- [What changed]
- Reason: [Why]
```
