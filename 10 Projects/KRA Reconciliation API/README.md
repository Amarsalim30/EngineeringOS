# KRA Reconciliation API

**Path**: `/home/amar-salim/Documents/CODING/kra-reconciliation-api`
**Stack**: FastAPI, SQLAlchemy, PostgreSQL, Alembic, openpyxl
**Python**: 3.14

## What It Does

Reconciles KRA (Kenya Revenue Authority) tax data — matches sales and purchase invoices between SAP Business One and KRA CSV uploads, then exports structured Excel reports.

## Core Flow

1. **Fetch** — User selects a date range; API pulls invoices + credit notes from SAP Service Layer (sales or purchases), creates a database-backed `ReconciliationSession`, normalizes line items by VAT group.
2. **Upload** — User uploads one or more KRA CSV files; API detects section from filename (`SEC_B`, `SEC_F`, etc.), maps headers via flexible aliases, normalizes to `Invoice` objects, stores under the same session.
3. **Compare** — Runs the 4-phase reconciliation engine (exact match by CU+VAT → CU-level VAT resolution → missing detection → duplicate detection), persists results row-by-row.
4. **Export** — Generates a ZIP archive containing Excel workbooks (`01 Summary.xlsx`, `02 Exceptions.xlsx`, `03 Matches.xlsx`) with `_metadata.json` and SHA-256 checksum.

## Architecture

```
app/
├── main.py                    # FastAPI lifespan, CORS, exception handlers
├── core/                      # Config, security, SAP client, dependencies
├── database/                  # SQLAlchemy engine, Base, model registry
├── domain/                    # Pure business concepts (enums, constants, dataclasses)
├── models/                    # SQLAlchemy ORM models
├── schemas/                   # Pydantic request/response models
├── services/                  # Business logic (reconciliation, KRA parsing, SAP mapping)
├── repositories/              # DB query projections
├── reporting/                 # Excel export engine (workbooks, ZIP, strategies)
└── api/v1/                    # FastAPI routers (auth, sales, purchases, reconciliation, sessions, settings, templates)
```

## Dependencies

- `fastapi` — web framework
- `sqlalchemy` + `psycopg` — ORM + PostgreSQL driver
- `alembic` — schema migrations
- `pydantic-settings` — settings from `.env`
- `python-jose` — JWT tokens
- `passlib[bcrypt]` — password hashing
- `httpx` — SAP Service Layer HTTP client
- `openpyxl` — Excel workbook generation
- `python-multipart` — file uploads

## Running

```bash
# Install deps (uv)
uv sync

# Set up database
alembic upgrade head

# Create initial user
# (via POST /api/v1/auth/register)

# Run server
uv run uvicorn app.main:app --reload --port 8000
```

## Key Docs

- `docs/reconciliation_engine.md` — reconciliation logic
- `docs/sap_integration.md` — SAP connector details
- `docs/auth_docs.md` — authentication flow
- `docs/session_store_kra_csv.md` — session management
- `docs/system_architecture_testing.md` — testing architecture

## Status

Active development — Phase 2 complete (SAP integration, KRA CSV parsing, reconciliation engine, export engine, settings CRUD).
