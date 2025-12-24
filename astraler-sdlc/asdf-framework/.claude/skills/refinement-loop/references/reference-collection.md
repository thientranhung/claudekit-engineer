# Reference Collection Protocol

Systematic gathering of source documents to inform spec creation.

---

## When to Trigger

- At start of `/asdf:init` or `/asdf:spec`
- When user types "reference" during refinement loop
- When AI identifies information gaps

---

## Collection Flow

### Step 1: Initial Prompt

```markdown
**Do you have existing documents to reference?**

Categories:
- **Business** — PRD, BRD, requirements, user stories
- **Technical** — API specs, architecture docs, schemas
- **Data** — ERD, database schemas, data dictionaries
- **Design** — Wireframes, mockups, design system
- **External** — Third-party API docs, competitor analysis

Provide file path(s) or type "no" to continue.
```

### Step 2: Parse Documents

For each provided document:

1. **Detect format** — PDF, Markdown, text, image
2. **Extract relevant content:**
   - Requirements (functional/non-functional)
   - User stories and acceptance criteria
   - Technical specifications
   - Data structures and relationships
   - UI/UX requirements
   - Constraints and rules
3. **Report extraction:**
   ```markdown
   **Extracted from [filename]:**
   - [N] functional requirements
   - [M] non-functional requirements
   - [P] user stories
   - [Q] technical specs
   ```

### Step 3: Identify Gaps

After parsing, identify missing information:

```markdown
**I could use more info about:**
- [Topic 1] — [Why it's needed]
- [Topic 2] — [Why it's needed]

Any additional references?
```

### Step 4: Loop Until Complete

Repeat Steps 2-3 until user says "no" or "no more".

---

## Document Categories

### Business Documents
- PRD (Product Requirements Document)
- BRD (Business Requirements Document)
- User stories / Epics
- Market research
- Competitor analysis

**Extract:** Requirements, success metrics, user personas, constraints

### Technical Documents
- API specifications (OpenAPI/Swagger)
- Architecture diagrams
- System design docs
- Integration specs

**Extract:** Endpoints, data contracts, dependencies, patterns

### Data Documents
- ERD (Entity Relationship Diagram)
- Database schemas
- Data dictionaries
- Migration scripts

**Extract:** Entities, relationships, constraints, indexes

### Design Documents
- Wireframes
- Mockups
- Design system docs
- Component libraries

**Extract:** UI components, user flows, states, interactions

### External Documents
- Third-party API docs
- SDK documentation
- Compliance requirements
- Industry standards

**Extract:** Integration requirements, constraints, authentication

---

## Gap Detection

### Common Gaps by Document Type

| Starting With | Usually Missing |
|---------------|-----------------|
| BRD only | Technical specs, data model |
| API spec only | User stories, UI requirements |
| Wireframes only | Business rules, data validation |
| Database schema only | User flows, business context |

### Gap Questions

When gaps detected, ask specific questions:

```markdown
**Missing information:**

1. **Authentication** — What auth method? (JWT, OAuth, session?)
2. **Error handling** — What happens when [scenario]?
3. **Data validation** — What are the constraints for [field]?

Can you provide docs for these, or should I use sensible defaults?
```

---

## Integration with Spec Generation

After collection complete:

1. **Merge extracted info** into spec structure
2. **Map requirements** to appropriate sections
3. **Flag unresolved gaps** as "Open Questions" in spec
4. **Continue to draft** with enriched context

---

## Rules

- **Ask first** — Always offer reference collection opportunity
- **Parse thoroughly** — Extract all relevant info
- **Report clearly** — User should know what was extracted
- **Identify gaps** — Proactively ask for missing info
- **Respect "no"** — Don't force reference collection
