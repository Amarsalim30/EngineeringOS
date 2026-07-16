# KRA Reconciliation API

**Path:** `/home/amar-salim/Documents/CODING/kra-reconciliation-api`
**Knowledge base:** `/home/amar-salim/EngineeringOS/10 Projects/KRA Reconciliation API/`
**Backend:** FastAPI | **Database:** PostgreSQL | **Priority:** High

## Purpose

Compare SAP Business One invoices against KRA CSV uploads (purchases and sales) and export structured reconciliation reports.

## Current Milestone

**Bulk upload and reconciliation engine** — Phase 2 complete (SAP integration, KRA CSV parsing, reconciliation engine, export engine, settings CRUD).

## Architecture (app/)

```
├── api/v1/           # FastAPI routers (auth, sales, purchases, reconciliation, sessions, settings, templates)
├── core/             # Config, security, SAP client, dependencies
├── database/         # SQLAlchemy engine, Base, model registry
├── domain/           # Pure business concepts (enums, constants)
├── models/           # SQLAlchemy ORM models (User, ReconciliationSession, etc.)
├── schemas/          # Pydantic request/response models
├── services/         # Business logic (reconciliation, KRA parsing, SAP mapping, auth)
├── repositories/     # DB query projections
├── reporting/        # Excel export engine (workbooks, ZIP, strategies)
```

## Code Quality Observations

### Strengths
- Clean layered architecture (api → services → repositories → models)
- Deterministic O(n) reconciliation algorithm with 4-phase matching
- Smart duplicate detection at the MatchKey level
- Robust session management with expiry cleanup
- Settings with optimistic locking and audit logging
- PIN matching is advisory — doesn't block reconciliation

### Issues Found (from code review + Issues.md)
1. **🔴 SAP password stored in plaintext** in `sap_connections` table
2. **🔴 SAPClient uses synchronous `httpx.Client`** — blocks the event loop in async endpoints (the SAP fetch endpoints are sync but FastAPI routes are sync — this is inconsistent, some routes should probably be async if they do I/O)
3. **🟡 Dead code:** `refresh_token_service.py` imports `generate_refresh_token` from security but it's named `generate_opaque_token`
4. **🟡 VAT normalizer class-level dicts** — `_DEFAULT_INPUT_MAP`/`_DEFAULT_OUTPUT_MAP` not thread-safe with concurrent `load_from_db`
5. **🟡 Section mappings default hardcoded** in `SettingsService.get_or_create_system_settings` — should be in `domain/reconciliation_constants.py`
6. **🟡 No rate limiting** on auth endpoints (brute force risk)
7. **🟡 Stale plan files** cluttering the project root
8. **🟡 SQLite test DBs** committed in project root — not gitignored
9. **🟡 Export doesn't handle corrupted `comparison_results` JSON**

## Open Questions

- Is this running in production or dev? (PostgreSQL isn't installed locally — the `test*.db` SQLite files suggest testing)
- Are there any existing tests that need to pass?
- What's the next concrete feature or fix you want to work on?
