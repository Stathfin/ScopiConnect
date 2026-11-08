---
name: scopi-value-trap-check
description: Check whether a cheap-looking Indonesian stock (low P/E or P/BV, high dividend yield) shows value-trap warning signs, using SCOPI Connect data. Use when the user asks "is X cheap or a trap?", "kenapa murah?", or wants to vet a cheap screen result.
---

# Value-trap check with SCOPI

Follow `scopi-connect-basics` for `request_id`, cost and wording.

A low multiple is only cheap if the earnings and assets behind it hold up.
Look for evidence that they do not.

## Calls

1. `company__get_metrics` - confirm the stock really is cheap (P/E, P/BV,
   dividend yield) and note the dates.
2. `company__get_peer_valuation` - cheap vs its own sector, or is the whole
   sector cheap?
3. `company__get_statement_series` - 5 years of revenue, net income and
   operating cash flow.
4. `company__get_earnings_quality` - CFO vs net income, Sloan ratio.
5. `company__get_forensic_scores` - distress (Altman Z, non-financials) and
   manipulation flags (Beneish M, Montier C).
6. `company__get_dividend_history` - only if the yield is the attraction;
   use `dpsHistory`, never sum `payments`.
7. `company__explain_market_move` with 60 days - optional, if the price fell
   sharply and the user wants to know why.

For several candidates, use the compare tools instead of repeating calls.

## Warning signs to report

| Sign | Where |
|---|---|
| Earnings falling for 2+ years while P/E looks low | statement series |
| Profit not backed by cash (CFO/NI well below 1, Sloan beyond the 10% line) | earnings quality |
| Distress zone or manipulation flags | forensic scores |
| Discount to peers that already persisted (cheap for a reason) | peer valuation |
| Dividend above earnings or a one-off year driving the yield | dividend history vs net income |
| Rising debt while profits shrink | metrics, statement series |

## Write-up

- Verdict line: "no clear trap signs", "some warning signs" or "several warning
  signs" - describe the evidence, not a recommendation.
- A table of each sign: present / absent / not applicable, with the figure and
  its date.
- What would change the picture.
- End with: "Data from Stathfin via SCOPI Connect, as of <dates>. Not
  investment advice."
