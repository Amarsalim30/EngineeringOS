# Issues

## Open
- `refresh_token_service.py` imports `generate_refresh_token` from `core.security` but that function is named `generate_opaque_token` in security.py — this file is unused (auth_service.py handles refresh tokens instead)
- SAP password stored in plaintext in `sap_connections` table (needed for Service Layer login but is a security concern)
- `SAPClient` uses synchronous `httpx.Client` — blocks the event loop during SAP calls in async context
- No rate limiting on auth endpoints
- No database connection pooling configuration beyond `pool_pre_ping=True`
- Export does not handle the case where `comparison_results` JSON has been corrupted or has a stale schema version
- `kra_section_mappings` default is hardcoded in `SettingsService.get_or_create_system_settings` — should be centralized in domain constants
- VAT normalizer `_DEFAULT_INPUT_MAP` / `_DEFAULT_OUTPUT_MAP` are class-level dicts — not thread-safe if `load_from_db` is called concurrently

## Roadmap / Next Tasks

### DONE
- [x] **Configurable purchase CU source** (`9ecdb42`, 2026-07-16) — added `purchase_cu_source` to system settings; CU number can now be pulled from multiple SAP payload locations (e.g. journal memo). Migration `f0b1c2d3e4a5_add_purchase_cu_source.py`, updated sales/purchases API, sap_client, invoice_service, sap_mapper, settings_service, frontend SystemSettingsCard, added tests.
- [x] **SAP Field Mappings** (`f2cef12`, 2026-07-15) — SAPFieldExtractor service + full CRUD API + 877-line React component.
- [x] **Section mappings from filename** (`1d4f161`, 2026-07-15) — upload detects section (SEC_B→16%, etc.) from filename.
- [x] **Normalization refactor** (`e2cc9ee`, 2026-07-15) — split monolithic normalization into modular package.

### PENDING (next feature)
- [ ] **Multiple-file KRA CSV upload with auto VAT assignment**
  - Replace current single download-template + single-file upload flow.
  - Accept multiple KRA CSV files; parse each, normalize to `Invoice` objects (same as current operation).
  - **VAT group auto-assigned by file section** (regex on filename), NOT from CSV column:
    | File | Section | VAT Rate |
    |------|---------|----------|
    | `SEC_B_WITH_VAT_PIN1.csv` | B – General Rated Supplies (Sales) | 16% |
    | `SEC_F_WITH_VAT_PIN1.csv` | F – General Rated Purchases (local) | 16% |
    | `SEC_G_WITH_VAT_PIN1.csv` | G – Other Rated Purchases | 8% (petroleum etc.) |
    | `SEC_H_WITH_VAT_PIN1.csv` | H – Zero-Rated Purchases | 0% |
    | `SEC_I_WITH_VAT_PIN1.csv` | I – Exempt Purchases | Exempt |
  - Regex extracts section only (e.g. `SEC_B` → 16%).
  - **Best UX:** make section→VAT mapping configurable in Settings (not hardcoded).

### PENDING (follow-up, later)
- [ ] **Settings UI overhaul** — current Settings page feels overwhelming, confused, overengineered. Will simplify/restructure for clarity.


- `refresh_token_service.py` imports `generate_refresh_token` from `core.security` but that function is named `generate_opaque_token` in security.py — this file is unused (auth_service.py handles refresh tokens instead)
- SAP password stored in plaintext in `sap_connections` table (needed for Service Layer login but is a security concern)
- `SAPClient` uses synchronous `httpx.Client` — blocks the event loop during SAP calls in async context
- No rate limiting on auth endpoints
- No database connection pooling configuration beyond `pool_pre_ping=True`
- Export does not handle the case where `comparison_results` JSON has been corrupted or has a stale schema version
- `kra_section_mappings` default is hardcoded in `SettingsService.get_or_create_system_settings` — should be centralized in domain constants
- VAT normalizer `_DEFAULT_INPUT_MAP` / `_DEFAULT_OUTPUT_MAP` are class-level dicts — not thread-safe if `load_from_db` is called concurrently

## Closed

- (none yet)

## Technical Debt

- `refresh_token_service.py` is dead code — `auth_service.py` handles all refresh token operations
- `test_*.db` files in project root are leftover SQLite test databases
- Multiple plan files (`plan.md`, `plan2.md`, `plan3.md`, `implementation_plan.md`, `implementation_plan2.md`) are stale
- `# KRA Reconciliation API — Implementatio.md` is a misnamed file (truncated filename)
- `graphify-out/` contains cached AST analysis artifacts that should be gitignored
