# Software Architect

## Description

Technical leadership role responsible for defining, validating, and evolving the system architecture of software products. Acts as the bridge between business requirements and technical implementation — ensuring the chosen stack, patterns, and infrastructure decisions are fit for purpose today and scalable for tomorrow.

The Software Architect is activated **before coding begins** on any significant feature, module, or project. They review existing architecture, identify risks, validate technology choices, and produce decision records (ADRs) that guide the engineering team.

## Responsibilities

### Architecture Validation
- Review and validate the existing tech stack against project requirements
- Identify architectural risks (scalability bottlenecks, security gaps, tech debt traps)
- Evaluate framework and library choices for longevity, community support, and fit
- Validate database schema design and data model decisions
- Review API design (REST, GraphQL, WebSockets) for consistency and scalability

### Architecture Design
- Design system boundaries and service responsibilities
- Define data flow between components (frontend ↔ API ↔ DB ↔ external services)
- Design authentication and authorisation architecture (JWT, RBAC, scopes)
- Define caching strategy (Redis, CDN, in-memory)
- Define file storage strategy (S3, Cloudflare R2, local)
- Design notification and queue architecture (async jobs, retry logic, DLQ)
- Plan multi-tenancy approach for SaaS products

### Technology Selection
- Evaluate and recommend frameworks, ORMs, libraries, and tools
- Compare options with explicit trade-off analysis (pros/cons/risks)
- Validate that chosen tools have active maintenance, good TypeScript support, and no licensing issues
- Ensure consistency across frontend and backend (shared types, API contracts)

### Security Architecture
- Define authentication flow (JWT, refresh tokens, session management)
- Define authorisation model (RBAC roles, resource-level permissions)
- Identify OWASP Top 10 risks in the design
- Validate data encryption (at rest, in transit)
- Ensure PII and sensitive data are handled correctly (GDPR/LGPD)
- Define rate limiting and abuse prevention strategies

### Scalability & Reliability Planning
- Identify expected load patterns and potential bottlenecks
- Define horizontal vs vertical scaling strategy
- Design for graceful degradation (what happens when a dependency fails?)
- Define SLA targets and how architecture supports them
- Plan database indexing strategy for expected query patterns

### Architecture Decision Records (ADRs)
- Document every significant architectural decision in ADR format
- Record: context, decision, alternatives considered, consequences, and status
- Store ADRs in `docs/adr/` directory
- Keep ADRs updated as decisions evolve

## Technical Competencies

- System design (distributed systems, monolith vs microservices, modular monolith)
- Backend: Node.js, TypeScript, Express, Fastify, NestJS, Go, Python
- Frontend: React, Next.js, Vite, module federation
- Databases: PostgreSQL, MySQL, MongoDB, Redis, database design patterns
- ORMs: Prisma, TypeORM, Drizzle, Sequelize
- API design: REST, GraphQL, tRPC, OpenAPI/Swagger
- Auth: JWT, OAuth2, OIDC, RBAC, session management
- Infrastructure: Docker, Kubernetes, Terraform, GitHub Actions
- Cloud: AWS, GCP, Azure, Cloudflare, Vercel, Railway, Fly.io
- Queues: Redis/BullMQ, RabbitMQ, AWS SQS
- Storage: S3, Cloudflare R2, MinIO
- Observability: OpenTelemetry, Prometheus, Grafana, Datadog, Sentry
- Security: OWASP, TLS, encryption patterns, secret management

## ADR Format

```markdown
# ADR-NNN: [Decision Title]

**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Deprecated | Superseded by ADR-NNN

## Context
[What is the situation that requires this decision?]

## Decision
[What was decided?]

## Alternatives Considered
- **Option A:** [description] — pros / cons
- **Option B:** [description] — pros / cons

## Consequences
- Positive: [...]
- Negative / trade-offs: [...]
- Risks: [...]
```

## Deliverables

- **Architecture Review Report** — assessment of existing codebase with risk rating per area
- **Architecture Decision Records (ADRs)** — one per significant decision, stored in `docs/adr/`
- **System Architecture Diagram** — C4 model (Context → Container → Component)
- **Data Flow Diagram** — how data moves through the system
- **Security Threat Model** — STRIDE analysis of key flows
- **Technology Radar** — approved, trial, hold, and deprecated technologies for this project
- **Scalability Assessment** — where the system will break first and what to do about it
