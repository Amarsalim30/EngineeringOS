# M-PESA / T-Kash Agent Business — Domain Reference

> Context: uncle runs (or wants to track) a mobile-money agency. His request: "withdrawals, float, sales, purchases, total."
> See also: [[Uncle M-PESA Request - Open Questions]] and Harith memory `2026-07-16-uncle-mpesa-research.md`.

## How to get the raw data (for parsing/reconciliation)
- **Consumer M-PESA statement**: M-PESA app → Statements → pick period → download **PDF** (password = ID number or DOB). Also USSD `*334#`, SMS `STMT` to 456, or email request. Covers up to 5 years via email.
- **Business / Till statements (where "TDR" likely comes from)**: Safaricom **M-PESA for Business** portal → `business.safaricom.co.ke`. Till owners get transaction statements here; likely CSV/Excel export. This is the right source for an agent's till daily report.
- **Key point**: a till/agent statement is the input we'd parse. Format is usually PDF (passworded) or CSV from the business portal.

## Float (working capital)
- Electronic cash an agent pre-loads to serve customers. Bought wholesale from Safaricom / a super-agent.
- If float runs out → cannot do withdrawals (cash-out). Must be replenished ("float top-up").
- Float = asset on hand, NOT revenue.

## Revenue = commission on deposits + withdrawals
Agent gets a share of the customer-paid fee. Aggregated lines: 80% agent / 20% principal. Non-aggregated: less.
- Deposits are FREE to customers; agent still earns a deposit commission.
- Withdrawal commission bands (agent share, KES): 50–100→5, 101–500→8, 501–1k→10, 1k–1.5k→12, 1.5k–2.5k→15, 2.5k–3.5k→20, 3.5k–5k→25, 5k–7.5k→30, 7.5k–10k→35, 10k–15k→45, 15k–20k→60, 20k–25k→65, 25k–30k→70, 30k–35k→70, 35k–40k→100, 40k–45k→150, 45k–50k→180, 50k–150k→200. (Unregistered customers: N/A above 35k.)
- Deposit commission bands (agent share, KES): 50–100→4, 101–510→8, 511–1k→9, 1k–1.5k→10, 1.5k–2.5k→11, 2.5k–3.5k→12, 3.5k–5k→14, 5k–7.5k→20, 7.5k–10k→28, 10k–15k→40, 15k–20k→55, 20k–25k→71, 25k–30k→87, 30k–35k→103, 35k–40k→119, 40k–45k→135, 45k–50k→150, 50k–70k→190, 70k–150k→190.

## Agent types
- **Retail agent**: cash-in/cash-out to customers. Earns deposit + withdrawal commission.
- **Master agent**: recruits agents for Safaricom.
- **Super agent**: wholesale float distribution to other agents (banks/FSPs). "Sales" = float sold to sub-agents; "purchases" = float bought from Safaricom.

## T-Kash (Telkom) — alternative network
- USSD `*325#`. Withdrawal agent fees KES 11–303 (up to 150k). Sending 6–103.
- If uncle is on T-Kash not M-PESA, commission/charge math differs — confirm network Saturday.
- Max transaction 250k, daily 500k, wallet balance 500k.

## What "total" probably means
Total float moved, total commissions earned, or net cash position (float in − withdrawals paid out + commission). Confirm with uncle.

## Open questions (ask Saturday)
1. Exact meaning of "TDR" — till daily report? transaction report? Get a sample file.
2. Agent type — retail / super / shop-with-agency?
3. Sample statement/export (PDF or CSV) from Safaricom/T-Kash business portal.
4. What "total" means to him.
5. Network — M-PESA or T-Kash?
