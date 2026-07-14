# KRA Reconciliation API

**Path**: `/home/amar-salim/Documents/CODING/kra-reconciliation-api`
**Stack**: FastAPI, SQLAlchemy, PostgreSQL, Alembic, Pandas
**Python**: 3.14

## What It Does

Reconciles KRA (Kenya Revenue Authority) tax data — matches sales/purchases between local records and SAP Business One.

## Architecture

- **FastAPI** backend with async endpoints
- **SQLAlchemy** ORM + **Alembic** migrations
- **PostgreSQL** database
- **SAP Service Layer** integration for data sync
- **Reporting engine** — Excel export with ZIP packaging

## Key Docs

- `implementation_plan.md` — full architecture and phase plan
- `docs/reconciliation_engine.md` — reconciliation logic
- `docs/sap_integration.md` — SAP connector details
- `docs/auth_docs.md` — authentication flow

## Status

Active development.

## Related

- [[n8n Workflows]] (SAP integration)
