# 0008 — Ratings, disputes and reports (U6)

Date: 2026-11-26. Status: approved by the owner in Lavish. Reference: `design/u6-trust.html` (proposal: `design/proposals/u6-trust-proposal.html`). Builds on decisions 0002–0007.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | When rating is offered | Asked on the success screen, then a card on the task page until the 14-day window closes |
| 2 | Rating form | A bottom sheet with large stars and a word for each, an optional comment (300), "can't change after sending" |
| 3 | Hidden ratings | You see your own, marked Hidden, with who sees it and when (both rated, or the window's end time) |
| 4 | Profile reviews | The U2 list plus a ⋯ on each review to report it |
| 5 | Poster's Report a problem | In U5's "Not quite done?" sheet and the ⋯ menu, showing the deadline (auto-release time); gone after it |
| 6 | Problem form | One screen: reason, statement (10–1,000), up to 4 photos, a "What happens next" box; button "Report and hold 400 pts" |
| 7 | Worker's answer | A warning banner, an amber countdown, the poster's side shown, "Give your side" docked |
| 8 | Dispute page | A 3-step bar, the decision due time, both sides as cards, "Add photos · N left" |
| 9 | Decision | An outcome card with amounts from the viewer's side, the team's note, read-only, Rate docked when ratings open |
| 10 | Report entry points | "Report …" in every ⋯ menu and on long-press of a chat message |
| 11 | Report form | A bottom sheet (scam first, details 500, "they aren't told"), then a toast; a repeat shows "You reported this on …"; no "my reports" list |
| 12 | Worker with a problem | Workers can't open a dispute (403), so a "Something wrong?" sheet explains and offers chat, Report the poster or Report task |

## API gaps found (backend work, not built)

- The task's dispute block has no `outcome`, `worker_share` or `resolved_at`.
- A decided dispute has no `fee`, `worker_receives` or `poster_refund`, so the client would have to compute the fee.
- The rating block has no `reveal_at`.
- The dispute doesn't say how many evidence photos are allowed or left.
- `/me/tasks` and `/me/jobs` cards have no `can_rate` or `rate_until`.
- No list of my reports (owner chose none for now).
- Dispute changes aren't broadcast.
- Docs: 403 `ACCOUNT_SUSPENDED` and 422 field errors missing; the `GET /tasks/{ulid}` example lacks the dispute and rating blocks.
- Still open from 0002–0007: a readable 429 retry time, `tasks_done` on the person block, and barangay and category list endpoints.
