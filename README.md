# NULL Platform

Master repository for the NULL ITSM commercial SaaS platform. This repo does not
hold the product code — it tracks the component repositories, the architecture,
and the central development register, so the whole product can be managed from
one place without forcing everything into a single enormous repository.

## Component repositories

| Component | Repository | Status |
| --- | --- | --- |
| Frontend | [nullitsm/nullitsm.github.io](https://github.com/nullitsm/nullitsm.github.io) | Live — the ITSM screens site at https://nullitsm.github.io/ |
| Backend | [nullitsm/null-itsm-api](https://github.com/nullitsm/null-itsm-api) | In development (local, remote pending) — FastAPI modular monolith |
| Endpoint agent | [nullitsm/null-endpoint-agent](https://github.com/nullitsm/null-endpoint-agent) | Planned — Phase 3 |
| Integrations | [nullitsm/null-integrations](https://github.com/nullitsm/null-integrations) | Planned — Phase 4 |
| Infrastructure | [nullitsm/null-infrastructure](https://github.com/nullitsm/null-infrastructure) | Planned — Phase 1 as code only |

See [`repos.yaml`](repos.yaml) for the machine-readable manifest (role, URL,
branch, status, notes per component).

## Product

NULL ITSM is a multi-tenant enterprise IT service management and endpoint
management platform: Service Desk, Requests, Service Catalog, Knowledge Base,
Change, Problem, Patching, Vulnerability Management, CMDB, and the Endpoint
Management module with its 16 System Manager screens — all in the NULL
editorial design system (warm ivory, black sidebar, purple accents, serif
headings, black-and-white infrastructure photography).

## Architecture (summary)

- Modular monolith first — no unnecessary microservices.
- Backend: Python FastAPI, PostgreSQL (Row-Level Security), SQLAlchemy,
  Alembic, Redis, Celery, REST API, WebSockets.
- Frontend: existing HTML/CSS/JavaScript, shared API clients, org context.
- Auth: OIDC (AWS Cognito as production provider), MFA, RBAC with granular
  permissions, least privilege, append-only audit logging.
- Multi-tenancy: `organization_id` on tenant records, server-side tenant
  resolution, verified membership, tenant-aware jobs. Never frontend-only
  filtering.
- Infrastructure: Docker, AWS-compatible infrastructure as code,
  S3-compatible storage, centralized logging, CI/CD, separate dev / staging /
  production environments.

Full decisions and the five-phase plan: [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md).

## Development register

The central register lives in [`docs/DEVELOPMENT_REGISTER.md`](docs/DEVELOPMENT_REGISTER.md).
It tracks every feature across Frontend, Backend, Database, Security,
Integration, Testing, and Deployment. It is updated at each phase boundary
from the component workstreams' reports.

## Rules

- AWS provisioning and paid services require explicit infrastructure approval.
  Build deployment configurations without incurring charges.
- No real payments until the payment provider and production credentials are
  approved.
- Never invent vulnerability findings or claim simulated commands ran on real
  devices.
- No placeholder functionality presented as production functionality.
- No simultaneous edits to shared files. Frontend and backend proceed in
  parallel only once their contracts are established.

## Related (not tracked here)

- `jamiahmcquiller/null-systems` — the earlier personal-account hosting of the
  ITSM screens; superseded by the move to `nullitsm/nullitsm.github.io`.
- The NULL platform console (`jamiahmcquiller/nexus`) is a separate product
  surface and is not part of this master repository.
