# Meeting Notes
Add multiple files support for upload ,according to section Of filename e.g SEC_B** mapped to 16% VAT group.
Make CU number configurable ,can take it from multiple places from SAP payload such as journal memo  

---

## 2026-07-16 — Roadmap Clarification (from Amar)

**Done:** `feat: Add configurable purchase CU source to system settings` (`9ecdb42`)

**Pending task 1 — Multiple-file KRA CSV upload with auto VAT assignment:**
- Replace single download-template + single-file upload.
- Parse multiple KRA CSVs → normalize to Invoices (same operation as now).
- VAT group auto-assigned by file section via regex on filename (SEC_B→16%, SEC_F→16%, SEC_G→8%, SEC_H→0%, SEC_I→Exempt).
- Best UX: section→VAT mapping configurable in Settings.
Done
**Pending task 2 — Settings UI simplification:**
- Current Settings page is overwhelming / confused / overengineered.
- Plan to fix the information architecture for clarity.
