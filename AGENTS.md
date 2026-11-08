# Instructions for AI agents

You are reading this because a user asked you to install or use SCOPI Connect.

## Install

Pick the section for the client you are running in. If you are not one of
these clients, tell the user SCOPI Connect does not support it yet and stop.

### Claude Code

Ask the user to run these two slash commands (you cannot run slash commands
yourself):

```
/plugin marketplace add Stathfin/ScopiConnect
/plugin install scopi@scopi-connect
```

If the user prefers no plugin, you may run this shell command instead:

```
claude mcp add --transport http --scope user --client-id claude-code-stathfin-beta --callback-port 8080 scopi https://stathfin.id/api/mcp
```

The client ID and callback port are required. The server does not support
dynamic client registration, so other values fail.

### After installing

1. Ask the user to run `/mcp`, select `scopi`, and sign in in the browser.
   Port 8080 must be free during sign-in.
2. Ask the user to approve a credit limit at
   https://stathfin.id/api/external/spending. This page only offers approval
   after the connection exists.
3. Verify with one cheap call: `search_issuers` with `query` "bank" and a new
   `request_id`.

Never ask the user to paste an access token into the chat.

## Use

Load the `scopi-connect-basics` skill before your first SCOPI call. The short
version:

- Every call needs a `request_id` (ASCII, 1-100 characters). Use a new one for
  new work, and the same one only when retrying the identical call.
- Calls cost the user SCOPI Credits. Prefer one comparison call over several
  single calls, and do not repeat a call whose result you already have.
- Quote figures with their source date. Do not present output as a buy or
  sell recommendation.

## Errors

| Response | Meaning | Action |
|---|---|---|
| 400 | Invalid arguments or `request_id` | Fix the input; use a new `request_id` if the input changes |
| 401 | Sign-in expired (15 minutes in beta) | Ask the user to run `/mcp` and reconnect |
| 403 | Missing permission: no credit approval, plan does not include the tool (for example fair value on Plus), or private research not disclosed | Read the error; send the user to the spending page or explain the plan limit. Do not retry |
| 409 `request_id_conflict` | Same `request_id` used with different input | Use a new `request_id` |
| 409 `request_in_flight` | The same call is still running | Wait, then retry with the same `request_id` |
| 429 | Rate limit | Wait about a minute, then retry with the same `request_id` |
| 503 / `capacity_busy` | Server busy or feature unavailable | Retry later with the same `request_id`; no credits were taken |
