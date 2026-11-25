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

## Competitive scope — expanded (Nov 25, 2026)

Goal: commercially competitive with ServiceNow, ManageEngine, and Jira
Service Management — a product an IT department can depend on every day.

### Tier 1 — essential before commercial launch

- Email-to-ticket and email replies (inbound email worker, provider-agnostic).
- Employee self-service portal (`/portal`, org branding): submit tickets,
  browse catalog, track requests, search knowledge, password reset via
  authorized integrations, request hardware/software, approvals, notifications,
  satisfaction feedback.
- Advanced SLA and escalation engine: business hours calendar, SLA breach
  alerts, response/resolution timers, escalation rules.
- Asset lifecycle and software licensing: procurement, purchase orders,
  vendors, warranty, assignments, employee-to-device relationships,
  check-in/check-out, transfers, lost/stolen, depreciation, retirement,
  disposal, license management, compliance, renewal alerts.
- SAML, SCIM, directory integrations: SAML SSO, OIDC, SCIM provisioning,
  Active Directory, Microsoft Entra ID, LDAP, automatic deprovisioning,
  group sync, access certification, privileged access, temporary grants,
  service accounts, API credentials.
- Customer data import and export (migration in, portability out).
- Platform administration console (separate from customer admin): platform
  admin, customer org management, tenant health, subscriptions, usage,
  support tools, platform audit logs.
- Subscription billing and usage metering: plans, entitlements, user/endpoint
  limits, usage tracking, status, invoices. Architecture only — no real
  payments until provider + production credentials are approved.
- Backup and disaster recovery (automated backups, tested restore, DR,
  multi-zone, RTO/RPO).
- Security testing and monitoring.

### Tier 2 — enterprise capabilities

- Network operations and monitoring: device discovery, switch/router
  inventory, server monitoring, topology maps, interface/bandwidth/CPU/memory
  monitoring, availability, certificate expiry, DNS/DHCP, SNMP, syslog,
  outage alerts, service dependency mapping.
- Advanced workflow orchestration: visual workflow designer, rules,
  conditional routing, approval automation, scheduled jobs; every automation
  has execution history, error handling, retry policies, permissions.
- On-call scheduling, technician workload balancing, major incident
  management, parent/child tickets, ticket templates, recurring tickets,
  collision detection, satisfaction surveys.
- Advanced reporting and custom dashboards: technician/department
  performance, cost per ticket, first-contact resolution, MTTR, MTTA,
  compliance trends, vulnerability aging, patch success, change success,
  asset costs, license utilization, availability, custom report builder,
  scheduled PDF/CSV/Excel exports.
- API developer portal (keys, docs, usage), release management and feature
  flags, customer support and status page, service health.

### Tier 3 — product differentiators

- **NULL Intelligence**: AI-assisted operations — summarize incidents,
  correlate vulnerabilities, recommend remediation, detect patterns, help
  troubleshoot. Human approval required for consequential actions.
- **NULL Threat Intelligence**: the live threat map as a full intelligence
  workspace — real feed attribution, threat history, indicators, CVE
  correlations, affected endpoint identification.
- **NULL Infrastructure Intelligence**: assets, endpoints, network devices,
  services, vulnerabilities, dependencies in one operational picture, with
  topology; identify business services affected by a failing server or
  compromised endpoint.
- Predictive operational analytics, automated vulnerability correlation,
  cross-module remediation orchestration.

### Governance, Risk & Compliance

Dedicated GRC module: security event monitoring, centralized audit trails,
vulnerability management for NULL itself, secrets and encryption key
management, API security monitoring, login anomaly detection, session
management, security policy management, data retention policies. Compliance:
SOC 2, ISO 27001, NIST CSF, CIS Controls mapping, evidence collection,
control ownership, assessment history, exception management, compliance
reporting. These features help customers manage compliance — they do NOT
automatically make NULL certified or compliant. Reliability: backups, tested
restoration, DR, multi-zone, health/DB/queue monitoring, incident response,
RTO/RPO, availability reporting; explicit availability targets and recovery
objectives before accepting paying customers.

### Integrations

An integration framework, not hardcoded vendors per module. Priority
ecosystem: Microsoft Entra ID, Microsoft Intune, Greenbone, Microsoft
Defender, Slack, Microsoft Teams, GitHub, Power BI. Also SMTP, Microsoft
365, Google Workspace, Active Directory, LDAP, SNMP, webhook providers,
external payment services.

### Final navigation structure

- Command Center: Overview, My Workspace, Notifications
- Service Management: Tickets, Requests, Catalog, Knowledge
- IT Operations: Endpoints, Patching, Software, Remote Management
- Security: Vulnerabilities, Threat Intelligence, Compliance
- Infrastructure: Assets, CMDB, Network, Servers
- ITSM Workflows: Changes, Problems, CAB, Approvals
- Automation: Workflow Designer, Rules, Schedules
- Analytics: Reports, Dashboards, Performance
- Administration: Users, Roles, Organizations, Integrations
- Platform Management: Subscriptions, Billing, Tenant Management

### Personas (12)

Platform Super Administrators, Organization Administrators, IT Managers,
Service Desk Managers, Service Desk Technicians, Endpoint Administrators,
Security Analysts, Network Administrators, Change Managers, Auditors,
Employees / Requesters, External Vendors. Each gets an appropriate workspace
and permissions; the RBAC model is built around these personas.
