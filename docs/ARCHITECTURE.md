# NULL Platform — Architecture

## Product

NULL ITSM: a multi-tenant enterprise IT service management and endpoint
management platform, commercially deployable. One shared infrastructure,
independent customer organizations.

## Design system (preserved throughout)

Warm ivory/cream backgrounds, black navigation sidebar, purple/lavender
active states and accents, editorial serif page headings, clean sans-serif
interface type, fine borders and subtle shadows, black-and-white
infrastructure photography. No blue SaaS styling, no generic templates.

## Architecture decisions

| Area | Decision |
| --- | --- |
| Hosting | Cloud-hosted backend; AWS reference provider, subject to infrastructure approval |
| Frontend | Existing HTML, CSS, JavaScript; shared API clients; org context |
| Backend | Python FastAPI, modular monolith (no unnecessary microservices) |
| Database | PostgreSQL, SQLAlchemy, Alembic migrations |
| Auth | OIDC (AWS Cognito as production provider), MFA |
| Authorization | RBAC with granular permissions, least privilege, elevated-access approval |
| Cache / queue | Redis, Celery workers |
| Storage | Amazon S3 (S3-compatible) |
| App hosting | AWS ECS Fargate |
| Monitoring | AWS CloudWatch, centralized logging |
| API | REST, versioned (`/api/v1`); WebSockets where live updates need them |
| Agent | NULL Endpoint Agent, separately managed component, own repository |

## Multi-tenancy

- `organization_id` on every tenant-owned record.
- PostgreSQL Row-Level Security.
- Verified organization membership; server-side tenant resolution.
- Tenant-aware background jobs; org-specific permission enforcement.
- Never rely exclusively on frontend filtering for tenant security.
- One organization can never read or modify another's records.

## Unified data model

Organizations, Users, Technicians, Roles, Permissions, Tickets, Requests,
Services, Knowledge Articles, Endpoints, Assets, Vulnerabilities, Patches,
Deployments, Changes, Problems, Configuration Items, Approvals,
Notifications, Audit Events. Shared model across modules — no duplicated
records, no disconnected modules. A vulnerability links to its affected
endpoint, its remediation ticket, its patch deployment, and its verification
scan.

## Phases

1. **Platform Foundation** — multi-tenant org architecture, provisioning,
   membership, auth, OIDC, MFA, users, invitations, RBAC, granular
   permissions, least privilege, elevated-access approvals, sessions,
   append-only audit logging, RLS, API authorization, migrations, backups,
   recovery. *In progress (backend repo).*
2. **Complete ITSM Engine** — Service Desk, Requests, Service Catalog,
   Knowledge Base, Change, Problem, Notifications and SLA.
3. **Endpoint and Security Engine** — endpoint agent, enrollment, heartbeats,
   inventory, remote job execution, all 16 System Manager screens, patching,
   vulnerability management (real engine: Greenbone/OpenVAS initially,
   multi-engine architecture, real CVE + CISA KEV), CMDB/assets.
4. **Enterprise Operations** — workflow designer, automation rules,
   conditional routing, approval automation, scheduled jobs, custom forms /
   fields / dashboards / reports, cross-module integrations, API integrations,
   webhooks, external IdPs, email integrations, compliance reporting.
5. **Commercial SaaS** — onboarding, subscription plans, entitlements, user
   and endpoint limits, usage tracking, billing integration, invoices,
   multi-tenant administration, tenant health, monitoring, alerting, backup
   and disaster recovery, performance and security testing, rate limiting,
   API documentation, deployment automation, operational documentation.

## Security requirements

Least privilege, backend authorization, RLS, secure auth, MFA, encrypted
transport, secret management, secure password hashing, API rate limiting,
input validation, audit logging, session revocation, secure agent auth,
command allowlisting and authorization, signed agent updates, cross-tenant
protection, security testing. Remote operations use authenticated,
authorized, auditable execution.

## Hard boundaries

- AWS provisioning and paid services require explicit infrastructure approval.
- No real payments until the payment provider and production credentials are
  approved.
- Never invent vulnerability findings or claim simulated commands ran on real
  devices.
- No placeholder functionality presented as production functionality.
- Clearly distinguish development fixtures from production data.
