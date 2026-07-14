# Database

**Engine**: PostgreSQL (via `psycopg` driver)
**ORM**: SQLAlchemy 2.0+ with mapped columns
**Migrations**: Alembic
**Naming convention**: All constraints use Alembic naming conventions (`pk_`, `ix_`, `uq_`, `fk_`, `ck_` prefixes)

## Connection

```
postgresql+psycopg://kra_user:testing123@localhost:5432/kra_reconciliation
```

## Tables

### `users`

| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | autoincrement |
| username | VARCHAR(100) | unique, indexed |
| email | VARCHAR(255) | nullable |
| password_hash | VARCHAR(255) | bcrypt |
| role | VARCHAR(20) | default "checker" |
| is_active | BOOLEAN | default true |
| created_at | TIMESTAMPTZ | server default now() |
| updated_at | TIMESTAMPTZ | server default now(), onupdate now() |

### `refresh_tokens`

| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | autoincrement |
| token_hash | VARCHAR(64) | unique, indexed, SHA-256 of raw token |
| user_id | INTEGER FK → users.id | ON DELETE CASCADE |
| created_at | TIMESTAMPTZ | |
| expires_at | TIMESTAMPTZ | |
| revoked_at | TIMESTAMPTZ | nullable |

### `reconciliation_sessions`

| Column | Type | Notes |
|--------|------|-------|
| id | VARCHAR(36) PK | UUID |
| user_id | INTEGER FK → users.id | ON DELETE CASCADE, indexed |
| from_date | DATE | |
| to_date | DATE | |
| is_compared | BOOLEAN | default false |
| comparison_results | JSON | cached summary dict |
| session_type | ENUM('sales','purchases') | stored as VARCHAR(50) |
| created_at | TIMESTAMPTZ | |
| last_accessed_at | TIMESTAMPTZ | updated on access |

### `session_invoices`

| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | autoincrement |
| session_id | VARCHAR(36) FK → reconciliation_sessions.id | ON DELETE CASCADE |
| row_number | INTEGER | |
| source | ENUM('SAP','KRA') | |
| pin | VARCHAR(100) | |
| partner_name | VARCHAR(255) | |
| invoice_number | VARCHAR(100) | |
| invoice_date | DATE | |
| cu_number | VARCHAR(100) | |
| vat_group | VARCHAR(50) | |
| base_amount | NUMERIC(18,2) | |

**Indexes**: `(session_id, source, row_number)` unique, `(session_id, cu_number)`, `(session_id)`

### `session_reconciliation_results`

| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | autoincrement |
| session_id | VARCHAR(36) FK → reconciliation_sessions.id | ON DELETE CASCADE |
| row_number | INTEGER | |
| cu_number | VARCHAR(100) | |
| status | VARCHAR(50) | ReconciliationStatus enum value |
| amount_match | BOOLEAN | |
| vat_match | BOOLEAN | |
| date_match | BOOLEAN | |
| partner_name_matches | BOOLEAN | |
| pin_matches | BOOLEAN | |
| sap_invoice_number | VARCHAR(100) | nullable snapshot |
| sap_partner_name | VARCHAR(255) | nullable |
| sap_pin | VARCHAR(100) | nullable |
| sap_invoice_date | DATE | nullable |
| sap_base_amount | NUMERIC(18,2) | nullable |
| sap_vat_group | VARCHAR(50) | nullable |
| kra_invoice_number | VARCHAR(100) | nullable snapshot |
| kra_partner_name | VARCHAR(255) | nullable |
| kra_pin | VARCHAR(100) | nullable |
| kra_invoice_date | DATE | nullable |
| kra_base_amount | NUMERIC(18,2) | nullable |
| kra_vat_group | VARCHAR(50) | nullable |

**Indexes**: `(session_id, row_number)` unique

### `sap_connections`

| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | autoincrement, indexed |
| name | VARCHAR(100) | default "Primary SAP Connection" |
| base_url | VARCHAR(500) | |
| company_db | VARCHAR(100) | |
| username | VARCHAR(100) | |
| password | VARCHAR(500) | stored plaintext (SAP needs it for login) |
| verify_ssl | BOOLEAN | default true |
| is_active | BOOLEAN | default true |
| version | INTEGER | optimistic locking counter |
| created_at | DATETIME | |
| updated_at | DATETIME | |
| updated_by_id | INTEGER | nullable |

### `system_settings`

| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | singleton row |
| active_connection_id | INTEGER FK → sap_connections.id | ON DELETE SET NULL, nullable |
| amount_tolerance | NUMERIC(10,2) | default 10.00 |
| base_amount_policy | VARCHAR(50) | skip/reject_session/treat_as_zero |
| unmapped_vat_policy | VARCHAR(50) | reject_invoice/needs_review |
| ignore_missing_cu | BOOLEAN | default false |
| include_credit_notes | BOOLEAN | default true |
| include_debit_notes | BOOLEAN | default true |
| skip_cancelled | BOOLEAN | default true |
| kra_section_mappings | JSON | {"SEC_B":"16","SEC_F":"16","SEC_G":"16","SEC_H":"8","SEC_I":"8"} |
| version | INTEGER | optimistic locking counter |
| updated_at | DATETIME | |
| updated_by_id | INTEGER | nullable |

### `vat_mappings`

| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | autoincrement, indexed |
| connection_id | INTEGER FK → sap_connections.id | ON DELETE CASCADE |
| module | VARCHAR(20) | "sales" or "purchases" |
| sap_code | VARCHAR(50) | e.g. "I1", "O1", "X1" |
| description | VARCHAR(200) | |
| canonical_value | VARCHAR(50) | VAT_16/VAT_8/ZERO_RATED/EXEMPT |
| is_builtin | BOOLEAN | built-in codes can't be deleted |
| is_system_generated | BOOLEAN | |
| created_at | DATETIME | |
| updated_at | DATETIME | |

**Unique constraint**: `(connection_id, module, sap_code)`

### `setting_audit_logs`

| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | autoincrement, indexed |
| user_id | INTEGER | nullable |
| user_email | VARCHAR(255) | nullable |
| action | VARCHAR(100) | e.g. "update_sap_connection" |
| changes_json | JSON | {"field": {"old": x, "new": y}} |
| reason | TEXT | nullable |
| created_at | DATETIME | |

## Migrations

Alembic manages schema changes.

```bash
alembic upgrade head
alembic revision --autogenerate -m "description"
```

**Migration history** (in `alembic/versions/`):

1. `ac33783ac85e` — create users table
2. `ee725638eb0d` — add email, is_active, refresh_tokens
3. `81e1418a458c` — change vat_group to string
4. `dd58b3443d62` — add pin_matches and partner_name columns
5. `5df3450cc2c3` — add pagination and results model
6. `27d2cfa7b5ca` — add enterprise settings tables

## Key Queries

- **Session cleanup**: Delete sessions where `last_accessed_at < now - 30min` for a user
- **Projections**: Ordered by status priority (CASE expression), CU number, invoice numbers
- **SAP index**: Grouped by `MatchKey(cu_number, vat_group)` for O(1) lookup during reconciliation
- **CU index**: Grouped by `CuKey(cu_number)` for VAT mismatch resolution

## Performance Notes

- `session_invoices` has composite index on `(session_id, source, row_number)` for paginated queries
- `session_invoices` has index on `(session_id, cu_number)` for reconciliation grouping
- `reconciliation_sessions` indexed on `user_id` for cleanup queries
- Session expiry check uses `last_accessed_at` index
- Export queries use `CASE` expression with `STATUS_PRIORITY` dict for deterministic ordering
