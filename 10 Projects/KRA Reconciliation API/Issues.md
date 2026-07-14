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

## Closed

- (none yet)

## Technical Debt

- `refresh_token_service.py` is dead code — `auth_service.py` handles all refresh token operations
- `test_*.db` files in project root are leftover SQLite test databases
- Multiple plan files (`plan.md`, `plan2.md`, `plan3.md`, `implementation_plan.md`, `implementation_plan2.md`) are stale
- `# KRA Reconciliation API — Implementatio.md` is a misnamed file (truncated filename)
- `graphify-out/` contains cached AST analysis artifacts that should be gitignored
