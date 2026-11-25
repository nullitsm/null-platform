# NULL Platform — Development Register

Central register for the whole product. Updated at each phase boundary from
the component workstreams' reports. Statuses: not started | in progress |
done | blocked | deferred.

## Components

| Component | Repo | Status |
| --- | --- | --- |
| Frontend (ITSM screens) | nullitsm/nullitsm.github.io | Live |
| Backend (FastAPI) | nullitsm/null-itsm-api | In progress (Phase 1) |
| Endpoint agent | nullitsm/null-endpoint-agent | Planned (Phase 3) |
| Integrations | nullitsm/null-integrations | Planned (Phase 4) |
| Infrastructure | nullitsm/null-infrastructure | Planned (Phase 1 as code only) |

## Frontend rounds (in flight, Nov 25, 2026)

| Feature | Implementation | Backend | Frontend | Security | Test | Deployment |
| --- | --- | --- | --- | --- | --- | --- |
| Ticket storage (browser persistence) | In progress | Browser localStorage | Tickets page | — | Local verify pending | Push pending |
| Ticket detail page (unified timeline, 4 ticket types) | In progress | Browser stores | New detail page | — | Local verify pending | Push pending |
| Users & Roles page | In progress | Browser store | New page | — | Local verify pending | Push pending |
| Endpoint Management module (16 System Manager screens, identity header) | Queued | Demo/simulated until agent | After ticket round | RBAC + action log | Pending | Pending |
| Light / Dark / System theme | Queued | — | After endpoint module | — | Pending | Pending |

## Phase 1 — Platform Foundation (backend repo, in progress)

| Feature | Implementation | Backend | Frontend | Security | Test | Deployment |
| --- | --- | --- | --- | --- | --- | --- |
| FastAPI app, versioned API, health checks, docs | In progress | In progress | — | — | Pending | Dev env pending |
| DB schema + RLS (orgs, users, memberships, roles, permissions, role_permissions, user_roles, invitations, sessions, audit_logs) | In progress | In progress | — | RLS | Pending | — |
| Auth (OIDC, MFA, recovery, invitations, sessions, refresh) | In progress | In progress | Deferred | — | Pending | Cognito: AWS approval needed |
| Organization management | In progress | In progress | Deferred | — | Pending | — |
| Users & Roles API | In progress | In progress | Deferred | Granular perms | Pending | — |
| Append-only audit logging | In progress | In progress | — | Immutable | Pending | — |
| Docker dev environment | In progress | In progress | — | — | Pending | — |
| AWS staging (ECS, RDS, Redis, S3, Cognito, CloudWatch, backups, CI/CD) | Not started | Code only | — | — | Backups untested | **Blocked: infrastructure approval** |
| Frontend integration (sign-in, Command Center, Settings, Users & Roles, API client, org context) | Deferred | Contract only | Deferred | — | Pending | After frontend rounds settle |

## Phases 2–5 (not started)

| Phase | Implementation | Backend | Frontend | Security | Test | Deployment |
| --- | --- | --- | --- | --- | --- | --- |
| 2 — Complete ITSM Engine | Not started | — | — | — | — | — |
| 3 — Endpoint and Security Engine | Not started | — | — | — | — | Agent repo to be created |
| 4 — Enterprise Operations | Not started | — | — | — | — | — |
| 5 — Commercial SaaS | Not started | — | — | — | — | Payments: provider + prod credentials approval needed |

## Outstanding dependencies

- AWS infrastructure approval (staging, Cognito, managed Postgres/Redis, S3,
  backups) — nothing provisioned until approved.
- Payment provider + production credentials approval (Phase 5).
- Frontend rounds to settle before backend frontend-integration work.
- GitHub remote for the backend repo (to be created when Phase 1 lands).
- Baseline frontend head: `be38333228373760cad736a876b8305c150c79f6`.

## Expanded scope — competitive gaps (Nov 25, 2026)

Added to the master build: make NULL commercially competitive with
ServiceNow, ManageEngine, and Jira Service Management.

| Requirement | Tier | Status |
| --- | --- | --- |
| Email-to-ticket and email notifications | Essential | Not started |
| Employee self-service portal | Essential | Not started |
| Advanced SLA and escalation engine | Essential | Not started |
| Asset lifecycle and software licensing | Essential | Not started |
| SAML, SCIM and directory integrations | Essential | Not started |
| Customer data import and export | Essential | Not started |
| Platform administration console | Essential | Not started |
| Subscription billing and usage metering | Essential | Not started |
| Backup and disaster recovery | Essential | Not started |
| Security testing and monitoring | Essential | Not started |
| Network operations and monitoring | Enterprise | Not started |
| Advanced workflow orchestration | Enterprise | Not started |
| On-call scheduling | Enterprise | Not started |
| Major incident management | Enterprise | Not started |
| Service dependency mapping | Enterprise | Not started |
| Advanced reporting and custom dashboards | Enterprise | Not started |
| API developer portal | Enterprise | Not started |
| Release management and feature flags | Enterprise | Not started |
| Customer support and status page | Enterprise | Not started |
| Compliance and evidence management | Enterprise | Not started |
| NULL Intelligence AI assistant | Differentiator | Not started |
| Threat intelligence workspace | Differentiator | Not started |
| Infrastructure intelligence and topology | Differentiator | Not started |
| Predictive operational analytics | Differentiator | Not started |
| Automated vulnerability correlation | Differentiator | Not started |
| Cross-module remediation orchestration | Differentiator | Not started |

Also in scope: GRC module (SOC 2 / ISO 27001 / NIST CSF / CIS mappings —
helps customers manage compliance, does NOT certify NULL), integration
framework (Entra ID, Intune, Greenbone, Defender, Slack, Teams, GitHub,
Power BI, SMTP, M365, Google Workspace, AD, LDAP, SNMP, webhooks, payments),
final navigation structure (Command Center, Service Management, IT
Operations, Security, Infrastructure, ITSM Workflows, Automation, Analytics,
Administration, Platform Management), and 12 personas with per-persona
workspaces and permissions.
