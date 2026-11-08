---
name: scopi-connect-basics
description: Rules for calling SCOPI Connect (Stathfin) MCP tools on Indonesian stocks. Load before the first SCOPI tool call in a conversation, or when a SCOPI call returns an error.
---

# Using SCOPI Connect

SCOPI Connect tools read Stathfin's Indonesian stock market data. Each call
costs the user SCOPI Credits, so treat calls as paid.

## request_id

- Every tool call needs `request_id`: ASCII, 1-100 characters.
- Use a fresh ID for every new piece of work, for example
  `bbca-metrics-20261108-1`.
- Reuse an ID only to retry the identical call (same tool, same arguments).
  The retry returns the original result without a second charge.
- Reusing an ID with different arguments is a conflict error.
- After a failed read is released, retry with a new ID.

## Spending credits well

- Plan the calls before making them. Tell the user roughly how many calls you
  intend when the plan needs more than about five.
- Prefer comparison tools (`company__compare_metrics`, `company__compare_banks`,
  `sector__compare_metric`, `company__compare_earnings_quality`) over several
  single-ticker calls.
- Screens, comparisons, peer valuation, market moves and broker summary are
  Heavy class (about 2x Basic). Valuation models and the valuation band are
  Premium (about 33x Basic, Pro only). Use Premium tools only when the user asks
  about fair value or valuation models.
- Do not call again for data already in the conversation.

## Reading results

- Every result has `status`. Only `ok` carries data. For anything else, read
  `reason` and explain it; do not invent the missing number.
- Values carry their own dates (market date, report period). Always state
  them, for example "P/E 14.2x (price 7 Nov 2026, TTM to Q3 2026)".
- Ratios are decimals unless labelled: ROE 0.15 means 15%.
- Broker summary data can lag the market; check `coverage.dataThrough`.
- Tickers are 4 uppercase letters (BBCA). Broker codes are 2 characters (YP).

## Wording

- The data is information, not advice. Never write "buy", "sell" or a price
  target as your own recommendation.
- Valuation-model estimates and bands are model output with a valuation date,
  not a promise.
- Forensic and earnings-quality scores are screens, not proof of wrongdoing.
- Broker activity is activity, not a signal.

## Errors

| Error | Action |
|---|---|
| 400 | Invalid arguments. Fix them; new input needs a new `request_id`. |
| 401 | Sign-in expired. Ask the user to run `/mcp` and reconnect. |
| 403 | Missing permission. Read the error: no credit approval (send the user to https://stathfin.id/api/external/spending), or the plan lacks the tool (Plus has no fair-value tools). Do not retry. |
| 409 `request_id_conflict` | Same ID with different input. Use a new `request_id`. |
| 409 `request_in_flight` | Still running. Wait, then retry with the same `request_id`. |
| 429 | Rate limit. Wait, then retry with the same `request_id`. |
| 503 or `capacity_busy` | Busy or unavailable. No credits taken. Retry later with the same `request_id`. |
