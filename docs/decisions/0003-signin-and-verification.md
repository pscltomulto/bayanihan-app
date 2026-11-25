# 0003 — Sign-in and verification (U1)

Date: 2026-11-25. Status: approved by the owner in Lavish. Reference: `design/u1-signin.html` (proposal: `design/proposals/u1-signin-proposal.html`). Builds on decision 0002.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Onboarding order | Offer profile setup right after sign-in with a "Look around first" exit; a setup card stays on Browse until tier 1 |
| 2 | Code input | One field with `autocomplete="one-time-code"` (SMS autofill), submitted at the 6th digit |
| 3 | Consent | "Before you start" screen for new accounts only, a privacy line on the phone screen, and a separate ID/selfie consent at the ID step |
| 4 | Profile layout | One page with progress, reused as Edit profile in U8 |
| 5 | Name | The real name as on the ID, with a hint that neighbors see it (the ID review compares the ID to the profile name) |
| 6 | Barangay | Searchable list, no location prompt |
| 7 | Skills | Optional, offered on the "you can now post" screen |
| 8 | Tier explainer | Progress card on Me linking to a full explainer screen |
| 9.1 | ID capture | Camera with a frame guide, plus an upload fallback |
| 9.2 | Selfie | Front camera with an oval guide and tips; no gallery on phones |
| 10 | Verification status | Card on Me plus a detail screen; rejected → resubmit flow, no resubmit for `underage` |
| 11 | Re-verify and dormant notices | A sheet once on return, a card on Me, and the U0 blocked-post screens at the moment of need |
| 12 | Language | English/Filipino toggle on the welcome screen, saved with `PATCH /me` after sign-in |

## API gaps found (backend work, not built)

- No barangay list endpoint.
- No category list endpoint for skills.
- Terms/privacy consent is not recorded, and `POST /me/verification` has no ID/selfie consent field.
- Tier caps and ID image retention can't be read by the app, so copy quoting them can go stale.
- An `other` rejection has no note the user can act on; `underage` has no `can_resubmit` flag.
- The API accepts any image as the selfie, so a live selfie isn't enforced.
- Still open from 0002: every 429 needs a retry time the browser can read.
