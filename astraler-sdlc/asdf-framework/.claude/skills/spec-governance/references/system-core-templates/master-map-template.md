# Master Map Template

**REQUIRED:** System architecture diagram (mermaid)

---

```markdown
# System Master Map

> **Version:** 1.0.0
> **Status:** Draft | Review | Approved
> **Last Updated:** YYMMDD

---

## 1. Overview

[2-3 sentences describing the system's purpose and core value proposition]

---

## 2. System Architecture Diagram (REQUIRED)

```mermaid
flowchart TB
    subgraph Client["Client Layer"]
        Web[Web App]
        Mobile[Mobile App]
        API_Client[API Clients]
    end

    subgraph Gateway["API Gateway"]
        LB[Load Balancer]
        Auth[Auth Middleware]
        Rate[Rate Limiter]
    end

    subgraph Services["Service Layer"]
        Service_A[Service A]
        Service_B[Service B]
        Service_C[Service C]
    end

    subgraph Data["Data Layer"]
        DB[(Primary DB)]
        Cache[(Cache)]
        Queue[(Message Queue)]
        Search[(Search Index)]
    end

    subgraph External["External Services"]
        Payment[Payment Gateway]
        Email[Email Service]
        Storage[Cloud Storage]
    end

    Web --> LB
    Mobile --> LB
    API_Client --> LB
    LB --> Auth
    Auth --> Rate
    Rate --> Service_A
    Rate --> Service_B
    Rate --> Service_C
    Service_A --> DB
    Service_A --> Cache
    Service_B --> DB
    Service_B --> Queue
    Service_C --> Search
    Service_B --> Payment
    Service_A --> Email
    Service_C --> Storage
```

---

## 3. Component Overview

### Client Layer
| Component | Technology | Purpose |
|-----------|------------|---------|
| Web App | [React/Vue/etc.] | [Main user interface] |
| Mobile App | [React Native/Flutter/etc.] | [Mobile experience] |
| API Clients | REST/GraphQL | [Third-party integrations] |

### Service Layer
| Service | Responsibility | Dependencies |
|---------|----------------|--------------|
| [Service A] | [What it does] | [DB, Cache] |
| [Service B] | [What it does] | [DB, Queue, Payment] |
| [Service C] | [What it does] | [Search, Storage] |

### Data Layer
| Store | Technology | Purpose |
|-------|------------|---------|
| Primary DB | [PostgreSQL/MySQL/etc.] | [Persistent data] |
| Cache | [Redis/Memcached] | [Session, hot data] |
| Queue | [RabbitMQ/SQS/etc.] | [Async processing] |
| Search | [Elasticsearch/Algolia] | [Full-text search] |

---

## 4. Communication Patterns

### Synchronous
| From | To | Protocol | Purpose |
|------|-----|----------|---------|
| Client | Gateway | HTTPS | User requests |
| Gateway | Services | HTTP/gRPC | Internal calls |

### Asynchronous
| Publisher | Event | Subscriber | Purpose |
|-----------|-------|------------|---------|
| [Service A] | [event.name] | [Service B] | [Why] |

---

## 5. Cross-Cutting Concerns

### Authentication & Authorization
- **Method:** [JWT/OAuth2/Session]
- **Provider:** [Internal/Auth0/Cognito]
- **RBAC:** [Roles defined]

### Observability
- **Logging:** [ELK/CloudWatch/etc.]
- **Metrics:** [Prometheus/Datadog/etc.]
- **Tracing:** [Jaeger/X-Ray/etc.]

### Security
- **TLS:** Required for all external traffic
- **Secrets:** [Vault/AWS Secrets Manager]
- **WAF:** [CloudFlare/AWS WAF]

---

## 6. Domain Map

```mermaid
flowchart LR
    subgraph Core["Core Domains"]
        Auth[Authentication]
        User[User Management]
    end

    subgraph Business["Business Domains"]
        Domain_A[Domain A]
        Domain_B[Domain B]
        Domain_C[Domain C]
    end

    subgraph Support["Supporting Domains"]
        Notification[Notifications]
        Analytics[Analytics]
    end

    Auth --> User
    User --> Domain_A
    User --> Domain_B
    Domain_A --> Domain_C
    Domain_B --> Notification
    Domain_C --> Analytics
```

| Domain | Type | Responsibility |
|--------|------|----------------|
| Authentication | Core | User identity, sessions |
| [Domain A] | Business | [Primary business logic] |
| Notifications | Supporting | Email, push, SMS |

---

## 7. Rules & Constraints

- [System-level rule 1]
- [System-level rule 2]
- [System-level constraint 1]

---

## 8. Dependencies

### External APIs
| Service | Purpose | Criticality |
|---------|---------|-------------|
| [Service] | [Why needed] | Critical/High/Low |

### Infrastructure
| Provider | Services Used |
|----------|---------------|
| [AWS/GCP/Azure] | [EC2, RDS, S3, etc.] |

---

## 9. Open Questions

| # | Question | Impact | Status |
|---|----------|--------|--------|
| 1 | [Question] | [Impact] | Open |

---

## 10. Changelog

### YYMMDD - v1.0.0 - Initial Draft
- Created system master map
- Defined architecture diagram
- Documented component relationships
```

---

## Diagram Requirements

The system architecture diagram MUST show:
- Client layer (web, mobile, API)
- Gateway/load balancer
- All major services
- Data stores (DB, cache, queue)
- External service integrations
- Communication flows
