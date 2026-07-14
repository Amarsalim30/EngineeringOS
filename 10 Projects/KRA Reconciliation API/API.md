# API

**Base URL**: `/api/v1`
**Auth**: OAuth2 Bearer token (JWT access token + refresh token)

## Endpoints

### Auth

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/auth/register` | No | Register new user (username, password, optional email/role) |
| POST | `/auth/login` | No | Login with username+password, returns access+refresh tokens |
| POST | `/auth/token` | No | OAuth2 token endpoint (form-data, for Swagger UI) |
| POST | `/auth/refresh` | No | Rotate refresh token, returns new access+refresh tokens |
| POST | `/auth/logout` | No | Revoke a refresh token |
| GET | `/auth/me` | Yes | Get current authenticated user profile |

### Sales

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/sales?from=YYYY-MM-DD&to=YYYY-MM-DD` | Yes | Fetch SAP sales invoices (Invoices + CreditNotes) into a new session. Auto-cleans expired sessions (>30 min idle). Returns first 100 invoices. |
| POST | `/sales/upload?session_id=X` | Yes | Upload KRA CSV files for sales. Auto-detects section from filename. Replaces previously uploaded KRA data for the session. |

### Purchases

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/purchases?from=YYYY-MM-DD&to=YYYY-MM-DD` | Yes | Fetch SAP purchase invoices (PurchaseInvoices + PurchaseCreditNotes) into a new session. Same behavior as sales. |
| POST | `/purchases/upload?session_id=X` | Yes | Upload KRA CSV files for purchases. Same behavior as sales upload. |

### Reconciliation

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/reconciliation/compare` | Yes | Run 4-phase reconciliation engine on session. Returns summary. Cached — subsequent calls return cached results. |
| GET | `/reconciliation/{session_id}/export?format=zip` | Yes | Export results as ZIP with Excel workbooks. Must run /compare first. |

### Sessions

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/sessions/{session_id}/invoices?source=SAP&page=1&limit=100` | Yes | Paginated invoices for a session (source: SAP or KRA) |
| GET | `/sessions/{session_id}/results?page=1&limit=100` | Yes | Paginated reconciliation results for a compared session |

### Templates

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/templates/{type}` | Yes | Download KRA CSV template (sales or purchases). UTF-8 BOM for Excel. |

### Settings

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/settings` | Yes | Get composite settings (SAP connection + system settings + VAT mappings) |
| PUT | `/settings/sap-connection` | Yes | Create/update active SAP connection (optimistic locking via version) |
| PUT | `/settings/system-settings` | Yes | Update reconciliation rules and tolerances (optimistic locking) |
| PUT | `/settings/vat-mappings` | Yes | Update SAP VAT code → canonical rate mappings |
| POST | `/settings/test-sap` | Yes | Test SAP connectivity (host reachable, auth, company valid) |
| GET | `/settings/audit-logs?limit=50` | Yes | Get configuration change audit log |

## Authentication

JWT-based with refresh token rotation:

- **Access token**: 30-minute expiry, contains `sub` (username), `exp`, `iat`, `jti`
- **Refresh token**: 7-day expiry, SHA-256 hashed in DB, single-use (rotation on refresh)
- **Password**: bcrypt hashed
- **Token URL**: `/api/v1/auth/token` (for OAuth2 flow)

### Headers

```
Authorization: Bearer <access_token>
```

## Request/Response Formats

### Login Response

```json
{
  "access_token": "eyJ...",
  "refresh_token": "abc123...",
  "token_type": "bearer"
}
```

### Invoice Object

```json
{
  "pin": "P051234567A",
  "partner_name": "ABC Customer Limited",
  "invoice_number": "INV-2026-0001",
  "invoice_date": "2026-01-15",
  "cu_number": "CU00012345",
  "vat_group": "16",
  "base_amount": 10000.00,
  "source": "SAP"
}
```

### Reconciliation Summary

```json
{
  "total_sap": 500,
  "total_kra": 480,
  "matches": 450,
  "missing_in_sap": 15,
  "missing_in_kra": 30,
  "missing_cu": 5,
  "mismatches": 20,
  "duplicate_cu": 5,
  "match_percentage": 90.0,
  "completion_percentage": 90.0,
  "total_reconciled_rows": 500,
  "mismatch_stats": { "amount": 12, "vat": 8, "date": 0 }
}
```

### Reconciliation Status Values

`Match`, `Missing in SAP`, `Missing in KRA`, `Missing CU Number`, `Amount Mismatch`, `VAT Mismatch`, `Multiple Mismatches`, `Duplicate Source Key`

## Errors

| Status | Meaning |
|--------|---------|
| 400 | Bad request (missing fields, invalid CSV, session type mismatch, session expired, not compared yet) |
| 401 | Invalid/missing JWT token |
| 403 | Inactive user or insufficient role |
| 404 | Session not found |
| 409 | Optimistic lock conflict on settings update |
| 500 | Internal error (SAP query failed, reconciliation engine failure) |
| 502 | SAP Service Layer connection error |

### SAP Error Handlers

- `SAPConnectionError` → 502
- `SAPQueryError` → 400
- `SAPConfigurationError` → 500
