# 0009 — Wallet (U7)

Date: 2026-11-26. Status: approved by the owner in Lavish. Reference: `design/u7-wallet.html` (proposal: `design/proposals/u7-wallet-proposal.html`). Builds on decisions 0002–0008.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Wallet home | A balance card (Available, Held for tasks, Cashing out), Add points and Cash out, recent history, a Payout accounts row |
| 2 | Payments not live | A notice in place of the two actions |
| 3 | History (owner's choice) | One flat list with type filters (All · Tasks · Added · Cash-outs); the phone filters the loaded pages until the API has a filter; task rows open the task, cash-in and cash-out rows open their status |
| 4 | Add points | One screen: amount, quick chips (the U3 shortfall filled in), GCash or Maya, the limits |
| 5 | Payment page | Opens in a new window while the app waits behind it |
| 6 | Waiting (owner's choice) | Back to Wallet with a pending card saying the balance doesn't include it yet and not to pay again; tap for a sheet with the points, amount charged, expiry and "Open the payment page again"; polling continues |
| 7 | Cash-in result (owner's choice) | A toast on Wallet, with "Back to your task" when started from U3; declined, cancelled, provider error and expired stay as a card on Wallet with "Try again" until dismissed |
| 8 | Cash-out gate | A fix card per `blocked_reason` (Verify your ID links to U1); the blocked screen stays as a backstop. "Held for review" isn't a separate state: the API returns held cash-outs as `processing` and a frozen wallet as `blocked_reason` `under_review` |
| 9 | Cash out | One form showing the fee and what the account receives, then a confirm sheet |
| 10 | Cash-out status | A timeline: requested, processing, paid — or failed then refunded |
| 11 | Payout accounts | Their own screen; add form explaining the 24-hour wait; remove dialog; numbers only masked |
| 12 | Weak signal | "Check again" reuses the same Idempotency-Key, so money never moves twice |

## API gaps found (backend work, not built)

- No fee quote before paying: `amount_charged_centavos` only comes back after the POST.
- No list of my cash-ins or cash-outs, so an unfinished one is only findable from the ulid saved on the phone.
- Cash-outs have no `failed_at`, `refund_by` or `refunded_at`.
- History rows give only the cash-in or cash-out ulid (no channel, masked number, net amount or status).
- Cash-in and cash-out `failure_reason` values aren't documented.
- `GET /wallet/history` has no `kind` filter (decision 3 filters on the phone until it does).
- `cash_in.blocked_reason` `limit_reached` covers both the daily limit and 3 open payments.
- Staff corrections (`adjustment`) have no reason the user can read.
- Still open from 0002–0008: a readable 429 retry time, and the other gaps listed there.
