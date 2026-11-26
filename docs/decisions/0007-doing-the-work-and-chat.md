# 0007 — Doing the work and chat (U5)

Date: 2026-11-26. Status: approved by the owner in Lavish. Reference: `design/u5-work.html` (proposal: `design/proposals/u5-work-proposal.html`). Builds on decisions 0002–0006.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Task page | One page for both sides: a 4-step bar, the task, the other person, the pin, and one action docked at the bottom |
| 2 | Exact pin | A map card with the landmark and "Only you and Ana see this", shown only to the poster and the accepted worker; falls back to the approximate area once the API stops sending the pin |
| 3 | Directions | One "Open in Google Maps" button |
| 4 | Start | A confirm sheet with optional before photos (the API takes them only with Start) and a note that location is checked once |
| 5 | Proof | A full-screen form with one progress bar (all photos go in one request); retry is safe |
| 6 | Confirm | The poster checks the work on the task page; "Confirm and pay Ben" opens a sheet with held, fee and worker-receives amounts |
| 7 | Changes | No request-changes step: "Not quite done?" points the poster to chat, with Report a problem (U6) as the backstop |
| 8 | Auto-release | Exact release time, time left and a bar, amber in the last 2 hours |
| 9 | Cancel by agreement | Ask from the ⋯ menu, answer from a banner, with the cooldown and the poster's no-start cancel; standing only in the viewer's own dialogs |
| 10 | Chat | A full-screen thread with a task strip on top |
| 11 | Live updates | Live over Reverb, polling every 15 s when the connection drops, a "New" divider, no "Seen" ticks |
| 12 | After the task ends | The exact closing time from `locked_at`, then read-only chat; removed photos show "Image removed" |

## API gaps found (backend work, not built)

- No request-changes status or endpoint, and nothing pauses `auto_release_at` (owner may revisit; decision 7 works without it).
- No read receipts: the other side's read marker isn't returned and no read event is broadcast.
- Only `message.sent` is broadcast; status changes reach an open task page only as system chat messages.
- `hidden_at` is never sent, so the worker can't be told when the pin will lock.
- `/me/tasks` and `/me/jobs` cards have no other-person name, `auto_release_at`, unread count or pending cancel request.
- Chat 429s don't say whether the text limit (30/min) or the photo limit (20/h) was hit.
- Declining a cancel request sends the asker no notice.
- Before photos can only be sent with Start.
- Docs: the `GET /tasks/{ulid}` example shows only the public view, message examples leave out `type`, and the channel is `task.{ulid}.{worker_ulid}`, not `task.{ulid}`.
- Still open from 0002–0006: a readable 429 retry time, `tasks_done` on the person block, and barangay and category list endpoints.
