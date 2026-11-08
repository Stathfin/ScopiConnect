---
name: scopi-issuer-analysis
description: Structured review of one Indonesian listed company (IDX ticker) using SCOPI Connect data - business, financial trend, quality, valuation, ownership, upcoming events. Use when the user asks to analyse, review or "bedah" a single stock such as BBCA or ADRO.
---

# Analysing one IDX company with SCOPI

Follow `scopi-connect-basics` for `request_id`, cost and wording.

## Calls (in order, stop early if the user only wants part)

1. `company__get_metrics` - price, market cap, P/E, P/BV, ROE, leverage,
   liquidity, CFO/NI, growth.
2. `company__get_statement_series` - revenue, net income and operating cash
   flow over the last 5 years or the last 8 quarters.
3. `company__get_earnings_quality` - are profits backed by cash?
4. `company__get_forensic_scores` - Altman Z, Beneish M, Montier C.
5. `company__get_peer_valuation` - premium or discount to sector peers.
6. `company__profile_ownership` - controller, free float, top holders.
7. `company__get_dividend_history` - only if the user cares about income.
8. `scopi__get_issuer_agenda` - upcoming RUPS, dividend, disclosure dates.

For banks, add `company__get_banking_metrics` and skip Altman Z (it does not
apply to financial issuers).

Ask before calling Premium tools (`scopi__get_saved_valuation_methods`,
`company__get_valuation_band`). Use them only if the user wants fair value.

## Write-up

Use these sections, each short and with dated figures:

1. **Snapshot** - what the company does (from sector/profile), price and size.
2. **Trend** - revenue, profit and cash flow direction; name the turning points.
3. **Quality** - cash backing of profits, forensic screens, balance-sheet risk.
4. **Valuation** - own multiples vs peers; model band only if fetched.
5. **Ownership and events** - controller, float, what is coming up.
6. **What to watch** - two to four concrete items the user can check next.

End with: "Data from Stathfin via SCOPI Connect, as of <dates>. Not investment
advice."

Do not give a buy/sell verdict. If the user asks for one, summarise the
strongest point for and against instead.
