# Architecture

## Overview

FastAPI REST API that reconciles SAP Business One invoice data against KRA (Kenya Revenue Authority) CSV uploads. Supports both sales and purchases. Exports reconciliation results as Excel workbooks in a ZIP archive.

## System Design

```
┌─────────────┐     ┌──────────────────┐     ┌─────────────┐
│   Frontend  │────▶│    FastAPI        │────▶│  PostgreSQL  │
│  (port 3000)│     │   (port 8000)    │     │  (port 5432) │
└─────────────┘     └──────────────────┘     └─────────────┘
                           │
                           ▼
                    ┌──────────────────┐
                    │  SAP Business One │
                    │  Service Layer    │
                    │  (OData REST)     │
                    └──────────────────┘
```

## Layers

### API Layer (`app/api/v1/`)

FastAPI routers. Each router handles HTTP concerns only — validation, auth, response formatting. Business logic delegates to services.

- `auth.py` — registration, login, token refresh, logout, /me
- `sales.py` — GET SAP sales, POST upload KRA CSVs
- `purchases.py` — GET SAP purchases, POST upload KRA CSVs
- `reconciliation.py` — POST compare, GET export (ZIP streaming)
- `sessions.py` — paginated invoice/result retrieval
- `settings.py` — CRUD for SAP connection, system settings, VAT mappings, test connection, audit logs
- `templates.py` — download KRA CSV templates

### Service Layer (`app/services/`)

Pure business logic. No HTTP awareness.

| Service | Responsibility |
|---------|---------------|
| `invoice_service.py` | Fetches SAP documents, maps to canonical rows, returns `list[Invoice]` |
| `kra_service.py` | Parses KRA CSVs with flexible header mapping, validation, error collection |
| `reconciliation_service.py` | 4-phase matching engine (exact → VAT resolution → missing → duplicates) |
| `sap_mapper.py` | Flattens raw SAP JSON into `CanonicalReconciliationRow` with VAT grouping |
| `vat_normalizer.py` | SAP VAT code → canonical rate ("16", "8", "0", "EXEMPT") via DB or defaults |
| `normalization.py` | Invoice field normalization (PIN, partner name, dates, amounts) |
| `auth_service.py` | Refresh token create/verify/rotate/revoke |
| `user_service.py` | User CRUD, password verification |
| `settings_service.py` | Settings CRUD, optimistic locking, SAP test connection, audit logs, .env fallback |
| `template_service.py` | KRA CSV template generation (UTF-8 BOM) |
| `summary_service.py` | Shared summary builder (used by compare + export) |

### Domain Layer (`app/domain/`)

Pure business concepts. No framework dependencies.

- `reconciliation_status.py` — `ReconciliationStatus` enum (8 statuses)
- `reconciliation_constants.py` — `STATUS_ORDER`, `STATUS_PRIORITY`, `REMARK_MAP`, version constants
- `document_types.py` — `CanonicalReconciliationRow`, `IngestionProvenance` dataclasses
- `template_constants.py` — CSV template headers and example rows

### Data Layer

| Module | Role |
|--------|------|
| `app/models/` | SQLAlchemy ORM models (User, RefreshToken, ReconciliationSession, SessionInvoice, SessionReconciliationResult, SAPConnection, SystemSetting, VATMapping, SettingAuditLog) |
| `app/schemas/` | Pydantic models for request/response (Invoice, ReconciliationResult, Settings, User, etc.) |
| `app/repositories/` | DB query projections (`ReconciliationProjection` frozen dataclass) |
| `app/database/` | Engine, session factory, Base declarative class with naming conventions |

### Reporting Engine (`app/reporting/`)

Generates Excel exports as ZIP archives.

```
build_export()
  → get_projections()        # repository query
  → to_export_rows()         # projection → ReconciliationExportRow (adds remark)
  → ReconciliationSummary    # loaded from session.comparison_results JSON
  → ZipExporter.export()     # strategy dispatch
    → build_all()            # summary + detail workbooks
    → pack_zip()             # ZIP with _metadata.json
```

**Workbooks produced**:
- `01 Summary.xlsx` — record counts, match rate, needs-review breakdown
- `02 Exceptions.xlsx` — sheets per exception type (Missing CU, Missing SAP, Missing KRA, Amount Mismatch, VAT Mismatch, Duplicate CU, Multiple Issues)
- `03 Matches.xlsx` — matched rows (compact format)

**Key abstractions**:
- `ExportStrategy` ABC → `ZipExporter` (only registered strategy)
- `ExportStrategyRegistry` — format → strategy lookup, created at app startup
- `ExportContext` — frozen metadata (user, version, timestamp)
- `ExportArtifact` — filename + media_type + BytesIO content
- `WorkbookArtifact` — zip_path + content bytes
- `SheetDefinition` / `WorkbookDefinition` — declarative column/status mapping

## Reconciliation Engine (4-Phase)

### Phase 0: Preprocessing & Duplicate Scanning
- Group records by `MatchKey(cu_number, vat_group)` within each source (SAP, KRA)
- Empty CU numbers → `MISSING_CU_NUMBER` status
- Duplicate MatchKeys within same source → `DUPLICATE_SOURCE_KEY` status (excluded from matching)

### Phase 1: Exact Match
- Intersection of SAP and KRA `MatchKey` sets
- Amount within tolerance (`AMOUNT_TOLERANCE`, default KES 10) AND same sign → `MATCH`
- Otherwise → `AMOUNT_MISMATCH`

### Phase 2: CU-Level VAT Resolution
- Remaining unmatched records grouped by `CuKey(cu_number)`
- Single SAP + single KRA per CU → compare amounts (VAT groups differ)
- Amount match → `VAT_MISMATCH`; otherwise → `MULTIPLE_MISMATCHES`

### Phase 3: Missing Detection
- Unmatched SAP records → `MISSING_IN_KRA`
- Unmatched KRA records → `MISSING_IN_SAP`

### Sort Order
Results sorted by: status priority → CU number → VAT group → SAP index → KRA index.

## Key Design Decisions

- **Session-based**: Each fetch creates a `ReconciliationSession` with 30-minute idle expiry. Invoices and results stored relationally (not just in memory).
- **Dual data source**: SAP from live Service Layer API, KRA from CSV upload. Both stored under same session.
- **SAP client on app.state**: Single `SAPClient` instance per process, with automatic session renewal and retry.
- **Export strategy pattern**: New formats (PDF, CSV) can be added by implementing `ExportStrategy` and registering in the registry.
- **Optimistic locking**: Settings updates use version numbers to prevent concurrent overwrites.
- **.env fallback**: If no SAP connection in DB, falls back to `.env` SAP credentials.
- **VAT normalization**: SAP-specific codes (I1, O1, X1, etc.) normalized to canonical rates (16, 8, 0, EXEMPT) via database-configurable mappings.
- **Deterministic output**: Export includes SHA-256 checksum over canonical JSON of all rows — same data always produces same hash.
