# Glossary Template

Use this template for `04-governance/glossary.md`.

---

```markdown
# Glossary

> **Version:** 1.0.0
> **Status:** Active
> **Last Updated:** YYMMDD

---

## 1. Overview

This document defines domain terminology and acronyms used throughout the project.

---

## 2. Quick Reference

| Term | Definition |
|------|------------|
| [Term 1] | [Brief definition] |
| [Term 2] | [Brief definition] |
| [Term 3] | [Brief definition] |

---

## 3. Domain Terms

### A

#### API (Application Programming Interface)
A set of protocols and tools for building software applications. In this project, refers to our REST API endpoints.

**Related:** REST, Endpoint, HTTP

---

### B

#### Backend
The server-side of the application that handles business logic, database operations, and API responses.

**Contrast with:** Frontend

---

### C

#### CRUD
Create, Read, Update, Delete — the four basic operations of persistent storage.

**Example:** User CRUD operations include creating accounts, viewing profiles, updating settings, and deleting accounts.

---

### D

#### Domain
A sphere of knowledge or activity. In our architecture, a domain represents a bounded context with its own business logic and data.

**Examples:** Auth domain, Payment domain, Order domain

---

### E

#### Entity
A domain object with a distinct identity. In our system, entities are persisted in the database.

**Examples:** User, Order, Product

---

### F

#### Feature
A user-facing capability or functionality. In ASDF, features are documented in `03-features/`.

**Format:** `YYMMDD-feature-name`

---

### M

#### MVP (Minimum Viable Product)
The version of a product with just enough features to satisfy early customers and provide feedback for future development.

---

### P

#### PII (Personally Identifiable Information)
Any data that could identify a specific individual.

**Examples:** Name, email, phone, address, SSN

**Handling:** Must be encrypted, access-logged, and deletion-capable (GDPR)

---

### R

#### RBAC (Role-Based Access Control)
A method of regulating access to resources based on the roles of individual users.

**Our Roles:** Super Admin, Admin, Manager, Member, Viewer

---

### S

#### Spec (Specification)
A detailed document describing requirements, design, and acceptance criteria for a feature or system.

**Location:** `astraler-docs/`

#### SLA (Service Level Agreement)
A commitment between service provider and client on aspects of the service such as availability, performance, and responsibilities.

**Example:** 99.9% uptime SLA

---

### T

#### Token
A piece of data used for authentication or authorization.

**Types in our system:**
- Access Token (JWT, 15 min)
- Refresh Token (opaque, 7 days)
- API Key (opaque, until revoked)

---

## 4. Acronyms

| Acronym | Full Form | Context |
|---------|-----------|---------|
| ADR | Architecture Decision Record | Decision documentation |
| API | Application Programming Interface | Backend services |
| ASDF | Astraler Spec-Driven Framework | Development methodology |
| AWS | Amazon Web Services | Cloud infrastructure |
| CI/CD | Continuous Integration/Deployment | DevOps pipeline |
| CRUD | Create, Read, Update, Delete | Data operations |
| DTO | Data Transfer Object | API contracts |
| ERD | Entity Relationship Diagram | Database design |
| GDPR | General Data Protection Regulation | Data privacy |
| JWT | JSON Web Token | Authentication |
| MFA | Multi-Factor Authentication | Security |
| MVP | Minimum Viable Product | Product scope |
| NFR | Non-Functional Requirement | Performance, security |
| ORM | Object-Relational Mapping | Database access |
| PII | Personally Identifiable Information | Data privacy |
| PR | Pull Request | Code review |
| RBAC | Role-Based Access Control | Authorization |
| REST | Representational State Transfer | API style |
| SLA | Service Level Agreement | Performance targets |
| SSO | Single Sign-On | Authentication |
| TDD | Test-Driven Development | Testing approach |
| UI/UX | User Interface/User Experience | Design |
| UUID | Universally Unique Identifier | IDs |
| WCAG | Web Content Accessibility Guidelines | Accessibility |

---

## 5. Project-Specific Terms

### [Your Domain] Terms

| Term | Definition | Example |
|------|------------|---------|
| [Term] | [Definition specific to your project] | [Usage example] |

---

## 6. Status Definitions

### Feature Status

| Status | Meaning |
|--------|---------|
| Planned | Scheduled for future development |
| In Progress | Currently being implemented |
| Review | Code complete, under review |
| Implemented | Deployed to production |
| Deprecated | Marked for removal |

### Document Status

| Status | Meaning |
|--------|---------|
| Draft | Initial version, not reviewed |
| Review | Under stakeholder review |
| Approved | Finalized and official |

---

## 7. Relationship Types

Used in ERD diagrams:

| Symbol | Meaning | Example |
|--------|---------|---------|
| `\|\|--\|\|` | One-to-one | User has one Profile |
| `\|\|--o{` | One-to-many | User has many Orders |
| `}o--o{` | Many-to-many | Products have many Tags |
| `\|\|--\|{` | One-to-many (required) | Order has many OrderItems |

---

## 8. Open Questions

| # | Question | Impact | Status |
|---|----------|--------|--------|
| 1 | [Terminology to clarify] | [Impact] | Open |

---

## 9. Changelog

### YYMMDD - v1.0.0 - Initial Draft
- Created glossary structure
- Added core domain terms
- Listed project acronyms
```

---

## Validation Rules

- [ ] Version header present
- [ ] Terms alphabetically organized
- [ ] Acronyms table included
- [ ] Domain-specific terms documented
- [ ] Status definitions clear
