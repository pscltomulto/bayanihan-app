# 0006 — Applying and choosing a worker (U4)

Date: 2026-11-26. Status: approved by the owner. Reference: `design/u4-apply.html` (proposal: `design/proposals/u4-apply-proposal.html`). Builds on decisions 0002–0005.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Apply form | A full-screen form with the task and "380 pts you'd receive" on top, an optional note and the start time |
| 2 | Start time (owner's choice) | Exact date and time pickers, like the U3 "Done by" pickers, opening on the next full hour; past times and times after the task deadline disabled; both `earliest_start_at` 422s drawn |
| 3 | Distance | A "Show how far I am" switch that asks for location only when tapped |
| 4 | My applications | Posted / Applied tabs inside My tasks, each with Active / Closed |
| 5 | Applicant list | Full cards: ID verified, rating, tasks done, jobs in this category, distance band, start time, note and past-work photos. Ordered by the API; standing never shown |
| 6 | Sort and filter | A sort chip opening a sheet, same pattern as the Browse filter sheet |
| 7 | Shortlist | A star on each card plus a "Starred 2/5" filter |
| 8 | Questions before the pick | A thread in the applicant detail with the remaining count ("2 of 3 left") |
| 9 | Pick | A confirm sheet: the exact pin is shared with the worker, the others are told, and the money (held, fee, worker receives) |
| 10 | Close / cancel (poster) | A ⋯ menu with confirm dialogs |
| 11 | Withdraw / drop out (worker) | A button on the application plus a dialog; dropping out after the pick says the worker's own standing goes down by 10, marked "only you see it" |
| 12 | Pick reminders | A waiting count and a "Pick by" chip on the task; tapping a reminder opens the applicant list |

## API gaps found (backend work, not built)

- The poster can't tell whether cancelling now will cost −10 standing (the free first-hour rule), so the cancel dialog can only say it "can lower" it. Suggest `cancel_lowers_standing` or `free_cancel_until` on the poster's `GET /tasks/{ulid}`.
- `GET /me/applications` rows carry only the task's title, points and deadline, and a closed task's detail returns 404 to the applicant, so a closed row can't open anything.
- `expired` covers deadline passed, poster cancelled and staff removed, so the app can't say which happened.
- Pre-pick questions have counts but no unread signal.
- Both pick reminders use the same `pick_reminder` notification kind with text only, so the app can't tell them apart.
- Docs: `openapi.yaml` for applying leaves out errors the code returns (TIER_TOO_LOW, ACCOUNT_SUSPENDED / ACCOUNT_CLOSED, TASK_UNDER_REVIEW, INVALID_TRANSITION, APPLICATIONS_CLOSED with reason accepted, both `earliest_start_at` 422s); `GET /applications/{ulid}` has no 200 example.
- Still open from 0002–0005: a readable 429 retry time, and barangay and category list endpoints.
