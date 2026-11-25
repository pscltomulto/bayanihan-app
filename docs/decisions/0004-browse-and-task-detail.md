# 0004 — Browse and task detail (U2)

Date: 2026-11-25. Status: approved by the owner in Lavish. Reference: `design/u2-browse.html` (proposal: `design/proposals/u2-browse-proposal.html`). Builds on decisions 0002 and 0003.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Feed card | The U0 card unchanged: exactly the feed's fields, with trust chips |
| 2 | Filters | Filter sheet, plus removable chips shown only while a filter is on; nothing filtered by default |
| 3 | Default sort | Newest across Dumaguete with no location prompt; "Near me" asks for location only when tapped, then sorts nearest within 3 km |
| 4 | Map | "Open map" and the list/map toggle open the same full-screen map of approximate areas; tapping an area shows its task card; list and map side by side from 768 px |
| 5 | Search | None until `GET /tasks` accepts `q`; then a search icon left of the bell that expands into a field |
| 6 | Task detail | Text first in the U0 order, photo thumbnails, a small approximate-area map, "I can do this" pinned in the bottom bar |
| 7 | Poster | The U0 row with trust chips; tap for the public profile |
| 8 | Points | Offer plus "You'd receive … after the … fee" from the detail's `fee` and `worker_receives`, with ⓘ for the breakdown |
| 9 | Tier 0 button | "Finish your profile to apply", returning to the task afterwards; all 12 button states drawn |
| 10 | Quiet day | "Show tasks within 10 km" or "Post a task"; a separate "Clear filters" empty state |
| 11 | Profile reviews | One list, each review tagged "As worker" or "As poster", paged |

## API gaps found (backend work, not built)

- `GET /tasks` has no text search parameter `q`.
- The poster block on the feed and detail has no `tasks_done`, which the trust chips need; today it costs a request to `/users/{ulid}` per poster.
- People other than the poster can't tell a full task from a closed one (`applications_closed_reason` is poster-only).
- The map gets at most one feed page (50 tasks); fine for the pilot.
- Docs: the `GET /tasks/{ulid}` example in `openapi.yaml` leaves out `my_application`.
- Still open from 0002/0003: a readable 429 retry time, and barangay and category list endpoints.
