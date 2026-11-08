# SCOPI Connect

Bring Stathfin's Indonesian stock market data into the AI you already use.

SCOPI Connect gives your AI assistant read-only access to the data behind
[Stathfin](https://stathfin.id) and SCOPI. Your AI does the reasoning on your own
plan. Stathfin supplies the data and charges SCOPI Credits per data call.

> **Beta.** Claude Code is the only supported client today. Claude.ai (web and
> Desktop), ChatGPT and Codex are coming. See [Supported clients](#supported-clients).

## What your AI can read

| Area | Examples |
|---|---|
| Company fundamentals | Valuation and quality metrics, statement time series, line-item movers, dividends (split-adjusted), earnings quality, forensic scores (Altman Z, Beneish M, Montier C) |
| Valuation | Peer valuation vs sector medians, saved valuation-model estimates, valuation band *(Pro)* |
| Banks | CASA, LDR, NIM, NPL for one bank or a comparison; bank screens |
| Screens | Custom IDX screens on up to 8 metrics, curated thematic presets |
| Ownership | Controlling group, free float, top 1% holders, foreign/retail/local mix |
| Broker summary | Top net buyers/sellers per stock, what one broker bought across stocks, weekly accumulation (licensed IPOT data) |
| Market and macro | Why a stock moved, IHSG/USD-IDR/BI rate/yields/CPI, probability cones, foreign SBN ownership, coal cash costs |
| Agenda | Upcoming RUPS, dividend and disclosure dates |

Every value carries its own source date. Read-only: SCOPI Connect cannot place
orders, change your research or buy credits.

## Requirements

- A Stathfin account with an active **Plus** or **Pro** plan (plans differ in
  available tools and rate limits; fair-value tools are Pro only).
- SCOPI Credits in your Stathfin wallet (the same wallet SCOPI Chat uses).
  Each data call spends credits by price class; current prices are on the
  [MCP docs](https://stathfin.id/docs/mcp).

## Install

### Claude Code (plugin, recommended)

```
/plugin marketplace add Stathfin/ScopiConnect
/plugin install scopi@scopi-connect
```

Then run `/mcp`, choose **scopi** and sign in with your Stathfin account.

### Claude Code (MCP only, no skills)

```
claude mcp add --transport http --scope user \
  --client-id claude-code-stathfin-beta --callback-port 8080 \
  scopi https://stathfin.id/api/mcp
```

### First connection

1. Sign in and approve the requested permissions.
2. While signed in to Stathfin, open
   [stathfin.id/api/external/spending](https://stathfin.id/api/external/spending)
   and approve a credit limit for the connection. The connection must exist
   before this page offers the approval.
3. Ask your AI something, for example: *"Compare NIM and NPL of BBCA, BBRI and BMRI."*

Manage or disconnect apps at
[stathfin.id/api/external/oauth/connections](https://stathfin.id/api/external/oauth/connections).

## Skills included

| Skill | Use it for |
|---|---|
| `scopi-connect-basics` | Rules every SCOPI call follows: `request_id`, cost, data dates, wording |
| `scopi-issuer-analysis` | A structured one-company review |
| `scopi-value-trap-check` | "Cheap, but is it a trap?" checks |
| `scopi-bank-screen` | Finding and comparing Indonesian banks |

## Pricing

Each call spends SCOPI Credits by price class (Basic, Heavy, Premium).
Retrying with the same `request_id` returns the first result without a second
charge. A read that fails after it starts is refunded automatically.

Current prices: [stathfin.id/docs/mcp](https://stathfin.id/docs/mcp)

## Supported clients

| Client | Status |
|---|---|
| Claude Code | Beta |
| Claude.ai web / Desktop | Coming soon |
| ChatGPT | Coming soon |
| Codex | Coming soon |

During the beta, sign-in lasts 15 minutes. After that, run `/mcp` again to
reconnect.

## Important

Data and tool output are information, not investment advice or a buy/sell
recommendation. Your AI writes its own conclusions from the data; check
figures against their source dates before you act.

## Bahasa Indonesia

SCOPI Connect menghubungkan data saham Indonesia dari Stathfin ke AI yang sudah
Anda pakai, misalnya Claude. Anda memakai langganan AI sendiri, dan Stathfin
memotong SCOPI Credits untuk setiap pengambilan data. Perlu akun Stathfin Plus
atau Pro. Cara pasang ada di bagian [Install](#install). Data ini bukan
rekomendasi investasi.

## License

The skills and configuration in this repository are MIT licensed. Access to
Stathfin data is governed by the Stathfin
[terms of service](https://stathfin.id/terms-of-service) and
[privacy policy](https://stathfin.id/privacy-policy).
