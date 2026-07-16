# KRA — Kenya Revenue Authority: Tax System Research

> **Research note** · compiled 2026-07-16 by Harith (Sentinel)
> Sources: kra.go.ke official publications, KRA pre-populated VAT return guide (27-11-2023), KRA tax rates page, Capital FM / AllAfrica (2026-07-14 fuel VAT extension).
> **Accuracy flag:** VAT rate regime changed repeatedly via Finance Acts 2023/2024/2025. Treat rates as "as of research date" and verify against current iTax before coding business logic.

---

## 1. What KRA is

The **Kenya Revenue Authority (KRA)** is the state agency responsible for assessing, collecting, and accounting for tax revenue in Kenya. It operates the national tax administration through several digital systems:

- **iTax** — the core web portal for filing returns, registering for taxes, generating payment slips (itax.kra.go.ke).
- **eTIMS** (Electronic Tax Invoice Management System) — the current e-invoicing platform replacing the older TIMS/VSAT ETR devices. Mandatory for issuing compliant tax invoices.
- **iCMS** — customs management system (imports/exports declarations).
- **M-Service App** — mobile companion to iTax for filing and payments.

---

## 2. Major tax types KRA administers

| Tax | Base | Rate / Note |
|---|---|---|
| Income Tax (PAYE) | Employment income | Progressive; monthly filing by 9th of following month |
| Income Tax (Corporate) | Company profit | 30% standard (resident companies) |
| VAT | Taxable supplies | **16% general**, **0% zero-rated**, **exempt**, plus temporary **8%** on fuel (see §4) |
| Excise Duty | Specified goods/services | Specific or ad valorem per Excise Act 2015 |
| Withholding Tax | Payments to vendors | Various rates |
| Advance Tax | Commercial vehicles | Per ton / per passenger capacity |

---

## 3. VAT — the core of the Reconciliation API

### 3.1 Filing mechanics
- VAT returns filed **online via iTax** by the **20th of the following month** using the **VAT3 Return form**.
- VAT registration is mandatory once taxable turnover exceeds **KSh 5,000,000** in any 12-month period.
- VAT is an **input/output** system:
  - **Output tax** = VAT charged on sales (collected from customers).
  - **Input tax** = VAT paid on business purchases (recoverable).
  - **Net VAT payable** = Output tax − Input tax (creditable).

### 3.2 Pre-populated (auto-fill) VAT returns — CRITICAL for reconciliation
Since **January 2024**, KRA pre-populates VAT returns from data it already holds:
- **eTIMS / TIMS** invoice transmission data.
- **iCMS** customs import data.
- Other KRA system sources.

The taxpayer reviews the pre-populated figures, amends where needed, and submits. This is exactly why a **reconciliation API matters**: your books vs. what KRA auto-generated will drift, and the gap must be explained.

### 3.3 VAT Return section structure (from KRA pre-populated guide)
The VAT3 return is organised into numbered sections. The ones relevant to reconciliation:

| Section | Meaning |
|---|---|
| **SEC_A** | Registered person / period details |
| **SEC_B** | Supplies made (taxable sales / output) — standard rated |
| **SEC_C** | Zero-rated supplies |
| **SEC_D** | Exempt supplies |
| **SEC_E** | Taxable purchases (input tax) — standard rated |
| **SEC_F** | Imported taxable supplies (customs / iCMS) |
| **SEC_G** | Other (e.g., supplies subject to 8% / other rate) |
| **SEC_H** | Zero-rated / exempt related adjustments |
| **SEC_I** | VAT payable computation / summary |
| **SEC_J / K** | Previously declared / adjustments |
| **SEC_L** | Declaration |

> ⚠️ The exact column layout per section changes between VAT3 revisions. The mapping in the KRA Reconciliation API (`section_mappings.py`) must be treated as **version-sensitive** and validated against the live iTax template.

---

## 4. VAT rates — important correction vs. earlier assumption

Earlier project notes assumed a standing 8% "other rate" mapped to SEC_G. The actual legal position:

- **16%** — general rate, applies to all taxable goods/services except zero-rated/exempt.
- **0%** — zero-rated, specific supplies in the Second Schedule to the VAT Act 2013 (e.g., exports, certain agricultural inputs).
- **Exempt** — no VAT charged, no input credit (e.g., financial services, residential housing).
- **8% (Other rate)** — *Originally applied to petroleum products, but was **deleted by Finance Act 2023** (effective 1 July 2023).*

**However** — as of **14 July 2026**, the government has **temporarily re-extended the reduced 8% VAT on petroleum products** for three months (until **14 October 2026**) as a fuel-price stabilization measure (Capital FM / AllAfrica, 2026-07-14).

**Implication for the API:** the section→rate mapping must be **configurable in Settings**, not hardcoded, because:
1. The 8% rate is politically temporary (expires 14 Oct 2026 unless extended).
2. Future Finance Acts will shift rates again.
3. Different taxpayers/sections legitimately carry different rates.

The current `section_mappings.py` design (SEC_B→16%, SEC_F→16%, SEC_G→8%, SEC_H→0%, SEC_I→Exempt) is a *reasonable default* but should be treated as seed data, overridable per tenant.

---

## 5. eTIMS — the invoice data source

- eTIMS is the mandatory e-invoicing system. Even non-VAT-registered businesses must onboard for certain flows.
- Invoices transmitted to KRA become the basis of pre-populated **SEC_B (sales)** and **SEC_E (purchases)** data.
- Onboarding: etims.kra.go.ke.
- Two integration paths relevant to an API:
  1. **Device/EFT** — physical fiscal device.
  2. **Online/API (eTIMS API)** — direct invoice submission; this is the integration surface a reconciliation tool would mirror.

---

## 6. Relevance to the KRA Reconciliation API project

| Research finding | Project implication |
|---|---|
| KRA auto-populates VAT returns from eTIMS + iCMS | Reconciliation must compare **our computed VAT** vs **KRA's pre-populated figures** per section |
| Section-based return (SEC_A–SEC_L) | Multi-file CSV upload should split by section; VAT group auto-assigned by filename regex (SEC_B, SEC_F, etc.) |
| Rate regime is volatile & temporary | Section→VAT mapping **must be configurable** in Settings, not hardcoded |
| VAT3 template revisions | `section_mappings.py` needs versioning / validation against live template |
| Filing deadline 20th of following month | Reconciliation runs should target pre-deadline windows |

---

## 7. Open questions / things to verify before coding
1. **Latest VAT3 CSV template** — obtain the current iTax export format (column headers per section). The older assumptions may be stale.
2. **eTIMS API auth model** — OAuth2 vs certificate; the project's `refresh_token_service.py` was flagged dead — confirm the real auth flow.
3. **Rate table source of truth** — should the API pull rates from a config table or call KRA? Recommend: local configurable table, manually updated on Finance Act changes.
4. **Multi-currency / forex** — imports (SEC_F) may involve USD; confirm if KRA stores KES-equivalent.
5. **Exempt vs 0% distinction** in SEC_H — affects input tax recovery; must not be conflated.

---

## 8. Sources
- KRA homepage — https://www.kra.go.ke
- Pre-populated VAT return step-by-step guide (27-11-2023) — https://www.kra.go.ke/images/publications/Pre-populated-VAT-return-Step-by-Step-Guide-27-11-2023.pdf
- KRA tax rates (VAT section) — https://www.kra.go.ke/.../tax-rates (iTax help)
- Capital FM / AllAfrica — "Govt extends 8% VAT on fuel for three more months" (2026-07-14) — https://allafrica.com/stories/202607140453.html

---
*Last updated: 2026-07-16 · Harith*
