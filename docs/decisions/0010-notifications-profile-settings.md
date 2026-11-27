# 0010 — Notifications, profile and settings (U8)

Date: 2026-11-27. Status: approved by the owner in Lavish. Reference: `design/u8-me.html` (proposal: `design/proposals/u8-me-proposal.html`). Builds on decisions 0002–0009.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Notification list | Grouped by day, with All / Unread tabs |
| 2 | Read and the bell | Tapping a notice marks it read, plus "Mark all read"; the bell shows a count (9+) |
| 3 | Where notices open | Each opens its screen; `new_sign_in` and `account_warning` open a sheet; the mockup maps all 27 kinds plus an unknown-kind fallback |
| 4 | Push | "Coming soon" until web push has a subscription endpoint; later a short explainer before the browser prompt, with on, blocked and iPhone states. Still only turned on from Notifications in Me (0002 #10.4) |
| 5 | Me hub | The U1 header and progress card, then grouped rows (You · App · Account), sign out last; bell on Me too |
| 6 | Edit profile | The U1 profile page, prefilled, Save docked; the photo saves when picked; a "See how neighbors see you" link |
| 7 | Skills | Chips inside Edit profile with a "3 of 5" counter; if one request fails, the screen says which part saved |
| 8 | Public profile | The U2 layout unchanged plus ⋯ Report (U6); skills shown only once the API returns them |
| 9 | Standing (own only) | Number, band meter, the rules, the latest drop, "Only you see this" |
| 10 | Devices | This phone plus "Sign out everywhere" with a confirm dialog (the API has only revoke-all) |
| 11 | Language | A Me row opening a small sheet; saved with `PATCH /me` |
| 12 | Sign out and closing | Sign out with a dialog; "Close my account" explains and points to Help (no close endpoint) |

## Open owner call

- A user can change their name after ID approval and keep the "ID verified" chip, so neighbors could see a verified name that no longer matches the ID. Options for the backend: lock the name after approval, or drop the chip until the ID is re-reviewed. Not decided; the mockup shows today's API behavior.

## API gaps found (backend work, not built)

- No device list or single-device sign-out.
- No way to remove a push device without signing out.
- No self-service account closing.
- Notification text is stored in English only.
- Notifications link only by `task_ulid`, so cash-in, cash-out, dispute and thread notices can't deep-link.
- No `cash_in_failed` notification.
- `GET /users/{ulid}` has no skills.
- No standing history, and the +2 is never reported.
- No version on profile edits, so the last save wins.
- Still open from 0002–0009: web push subscription endpoint, unread count without the full list, a readable 429 retry time, and the other gaps listed there.
