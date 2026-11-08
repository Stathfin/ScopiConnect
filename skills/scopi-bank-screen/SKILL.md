---
name: scopi-bank-screen
description: Find, screen and compare Indonesian listed banks by prudential ratios (NIM, NPL, CASA, LDR) and valuation using SCOPI Connect. Use for questions like "bank mana yang NPL rendah dan murah?" or "compare BBCA, BBRI, BMRI".
---

# Indonesian bank screen with SCOPI

Follow `scopi-connect-basics` for `request_id`, cost and wording.

## Pick the tool

| User wants | Tool |
|---|---|
| Compare 2-6 named banks | `company__compare_banks`, then `company__compare_metrics` for P/E, P/BV, ROE |
| Find banks meeting conditions | `market__run_custom_bank_screen` |
| Deep dive on one bank | `company__get_banking_metrics` plus the `scopi-issuer-analysis` skill |

## Screen filters

`market__run_custom_bank_screen` accepts `nim_ratio`, `npl_gross`,
`casa_ratio`, `ldr_ratio`, `pe_ttm` and `pbv`. Ratios are decimals:

- NIM 5% -> `0.05`
- NPL gross 3% -> `0.03`
- CASA 60% -> `0.60`
- LDR 90% -> `0.90`

It is slower than other screens because ratios are computed per bank. Results
are candidates, not recommendations.

## Reading the ratios

- **NIM**: higher means more margin on lending; from the latest annual report.
- **NPL gross**: share of bad loans; lower is better. Above 5% is the
  regulator's warning level.
- **CASA**: share of cheap current and savings deposits; higher lowers funding
  cost.
- **LDR**: loans vs deposits; roughly 78-92% is the regulator's comfort band.
  Very low may mean idle funds, very high may mean liquidity strain.

Non-bank tickers come back as not applicable; say so rather than dropping them.

## Write-up

- One table: ticker, NIM, NPL, CASA, LDR, P/BV, ROE, each with its report date.
- Two or three sentences on the trade-offs, for example "cheapest P/BV but
  highest NPL".
- End with: "Data from Stathfin via SCOPI Connect, as of <dates>. Not
  investment advice."
