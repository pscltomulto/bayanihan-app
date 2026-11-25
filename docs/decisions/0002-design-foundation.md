# 0002 — Design foundation (U0)

Date: 2026-11-25. Status: approved by the owner in Lavish. Reference: `design/u0-foundation.html` (proposal: `design/proposals/u0-foundation-proposal.html`).

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Personality | In between: neutral surfaces, teal only for points and the main action, warmth in the copy |
| 2 | Color | Primary teal `#0F766E`; success `#15803D`, warning `#B45309`, danger `#B91C1C`, always with an icon and a word |
| 3 | Theme | Follow the phone's light/dark setting |
| 4 | Type | Plus Jakarta Sans (SIL OFL), self-hosted in the build; mockups name it with a system-font fallback |
| 5 | Shape | Balanced: cards 12 px, inputs and buttons 8 px, pills fully round |
| 6 | Navigation | Five bottom tabs with Post in the middle as a "+" icon only (accessible label "Post a task"); bell top right |
| 7 | Money | Always "250 pts · ₱250" |
| 8 | Trust | Chips. Others see only ID verified, rating and tasks done; tier and standing only on your own Me, marked "Only you see" |
| 9 | Home | List first, with a small map strip of approximate areas |
| 10.1 | App icon | A roof carried by two neighbors, with a maskable safe zone |
| 10.2 | Splash | Teal splash; status bar follows the app surface per theme |
| 10.3 | Install | Install sheet after the first success, plus a permanent row in Me |
| 10.4 | Push | Turned on only from Notifications in Me; never prompted after posting or applying |
| 11 | No-photo card | Category icon tile |

## API gaps found (backend work, not built)

- 429 responses need a retry time the browser can read: `Retry-After` on every 429 and exposed through CORS, or an `errors.retry_after` field.
- Web push needs a subscription endpoint; `POST me/devices` handles FCM only.
- The unread notification count only comes with the full notifications list.
- `category.icon` uses Material icon names; the app uses Lucide, so it needs a mapping.
