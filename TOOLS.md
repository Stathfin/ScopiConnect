# SCOPI Connect tool index

Every tool below is read-only and costs SCOPI Credits. Every call needs a
`request_id` (ASCII, 1–100 characters): new ID for new work, same ID only to
retry the identical call. See `skills/scopi-connect-basics/SKILL.md` for the
full calling rules. Current credit prices are on the Stathfin MCP docs, not
here. Your MCP client can also list exact argument schemas via `tools/list`.

## Company data

- `company__get_metrics` — headline metrics for one ticker: price, market
  cap, P/E, P/BV, ROE, leverage, liquidity, CFO vs net income, growth. Each
  value carries its own date. Args: `ticker`, `metrics`.
- `company__compare_metrics` — same metrics across 2–6 tickers, never for one
  ticker. Args: `tickers`, `metrics`.
- `company__get_statement_series` — time series of statement line items
  (revenue, net income, operating cash flow), newest first. For trend
  questions. Args: `ticker`, `statement`, `periodType`.
- `company__compare_statements` — biggest line-item movers between the two
  latest periods of one statement. Args: `ticker`, `statement`,
  `periodType`.
- `company__get_dividend_history` — split-adjusted dividend history. Use
  `dpsHistory` for fiscal-year totals; never sum `payments`. Args: `ticker`.
- `company__profile_ownership` — sector, controlling group, free float,
  foreign/retail/local mix, top holders. Args: `ticker`.

## Earnings quality and banks

- `company__get_earnings_quality` — is profit backed by cash: CFO vs net
  income (TTM), accruals, Sloan ratio. Args: `ticker`.
- `company__compare_earnings_quality` — same quality signals across 2–6
  tickers. Args: `tickers`.
- `company__get_forensic_scores` — Altman Z, Beneish M, Montier C screens.
  Screens, not proof of wrongdoing. Skip Altman Z for banks. Args: `ticker`.
- `company__get_banking_metrics` — CASA, LDR, NIM, NPL gross/net for one bank
  with report dates. Non-banks return not-applicable. Args: `ticker`.
- `company__compare_banks` — same bank ratios across 2–6 tickers.
  Args: `tickers`.

## Peers, sectors, market moves

- `company__get_peer_valuation` — own P/E and P/BV vs sector peer medians,
  premium/discount percentages. Args: `ticker`.
- `company__explain_market_move` — why one ticker moved over 1/5/20/60
  trading days, with official evidence. Args: `ticker`,
  `lookbackTradingDays`.
- `sector__compare_metric` — one statement-derived ratio (margins, ROIC,
  gearing, coverage, working-capital days) across 2–6 tickers, computed
  identically. Args: `tickers`, `metricIds`.
- `sector__compare_coal_cost` — reviewed cash-cost rows for coal issuers; pass
  coal tickers or `allCoal: true`. Args: `ticker`.
- `scopi__get_issuer_agenda` — upcoming RUPS, dividend and disclosure events
  for one ticker. Args: `ticker`, optional `windowDays` (1–90).

## Operating KPIs

- `company__get_operating_kpis` — physical operating series from annual
  reports and Public Expose: coal production/cash cost, CPO OER/ASP, nickel
  and gold output, lifting, cement volume, tower tenancy, ARPU, BOR,
  occupancy, store count, marketing sales, order book, traffic. Omit
  `metricKeys` to list every recorded series first. Args: `ticker`, optional
  `metricKeys`, `maxYears` (1–15).

## Screeners

- `market__run_preset_screen` — members of one named thematic preset with
  latest prices; preset names are exact. Args: `preset`, optional `limit`.
- `market__run_custom_screen` — screen the IDX cross-section by 1–8 metric
  filters with one sort and a 1–50 limit. Ratios are decimals. Args:
  `metric`, `op`, `value` (plus sort and limit).
- `market__run_custom_bank_screen` — screen bank tickers by NIM, NPL, CASA,
  LDR plus P/E and P/BV. Slower, ratios computed per bank. Args: `metric`,
  `op`, `value`.

## Broker summary (Plus or Pro)

- `market__get_broker_summary` — top net buyers/sellers of one ticker over up
  to 366 days. Licensed IPOT data, can lag the market. Args: `ticker`,
  `from`, `to`, optional `investor`, `limit`.
- `market__get_broker_activity` — what one broker bought/sold most across
  stocks over up to 92 days. Args: `broker`, `from`, `to`.
- `market__get_broker_accumulation` — weekly and cumulative broker positions
  in one ticker over up to 366 days. Args: `ticker`, `from`, `to`.

## Macro

- `macro__get_snapshot` — latest values of named macro series (IHSG, USD/IDR,
  BI rate, yields, CPI). Keys must come from the enum. Args: `series`.
- `macro__get_sbn_ownership` — foreign ownership of tradable government
  securities (SBN): holdings, share %, daily/monthly change. A stock, never a
  flow. Takes no arguments.
- `macro__get_probability_cone` — range projection for IHSG, or one ticker,
  1–90 days or 1–60 sessions. Not a forecast promise. Args:
  `horizonCount`, `horizonUnit`, optional `ticker`.

## Issuer lookup

- `search_issuers` — find an issuer by name or ticker. Cheap first call to
  confirm the right ticker.
- `get_issuer_summary` — one sourced summary of an issuer. Args: `ticker`.

## Saved research (Pro only)

- `get_saved_research` — read your own saved research, only the text you
  choose to share. Needs My Research access.
- `changes_since_review` — what changed since a saved review, by record ID
  and baseline revision.

## Fair value (Pro only)

- `scopi__get_saved_valuation_methods` — saved DCF/multiples estimates for
  one ticker with their valuation day. Model output, not a price target.
  Args: `ticker`.
- `company__get_valuation_band` — rating band (very_expensive to
  very_attractive) from saved-model medians vs current price. Only when a
  qualified saved model exists. Args: `ticker`.
