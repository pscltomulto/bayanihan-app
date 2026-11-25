# 0005 — Post a task (U3)

Date: 2026-11-25. Status: approved by the owner in Lavish. Reference: `design/u3-post.html` (proposal: `design/proposals/u3-post-proposal.html`). Builds on decisions 0002–0004.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Form shape | Three short steps (What · Where · When and points), then Review |
| 2 | Points | A number field plus the U0 fee box filled live from `/tasks/quote` |
| 3 | Pin (owner's choice) | "Use my location" only: the poster stands at the job and the phone's location is the exact spot; accuracy shown with Try again, a poor-accuracy (> 100 m) warning with "Use it anyway", optional landmark; permission-blocked, location-off and timeout states; no map-drag fallback. Others see only the area |
| 4 | Schedule (owner's choice) | Date and time pickers for "Done by", enforcing 1 hour to 30 days from now |
| 5 | Photos | Optional; picked in the form, uploaded right after the post with progress on the success screen |
| 6 | Photo proof | The `proof_photos_required` switch shown, on by default |
| 7 | Review | A separate review screen; button "Post task · hold N pts"; no edit after posting |
| 8 | Not enough points | Inline shortfall on the points step with an "Add N pts" hand-off to Wallet (says so when cash-in isn't live); the 409 at confirm stays as a backstop |
| 9 | After posting | The U0 success screen; install sheet after the first post; held-for-review variant; no push prompt; skills prompt not repeated |
| 10 | Drafts | One draft saved on the phone, resumed from "+" |
| 11 | Weak signal | Manual retry that reuses the same Idempotency-Key, so a task is never posted twice; no auto-posting |

## API gaps found (backend work, not built)

- The 5-open-task limit (`LIMIT_REACHED`) can't be read from `/me` before the form.
- A task photo can't be deleted.
- A task can't be edited after posting, though spec §6 runs the keyword filter "on post and edit".
- The barangay and the `OUTSIDE_SERVICE_AREA` check only come back when the post is sent.
- `/tasks/quote` doesn't return the minimum offer.
- No notification when a held task is approved or hidden.
- Still open from 0002–0004: a readable 429 retry time, and barangay and category list endpoints.
