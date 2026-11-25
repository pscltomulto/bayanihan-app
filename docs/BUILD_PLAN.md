# Bayanihan app — UI build plan

Design first. Each phase produces one or more self-contained HTML mockups in `design/`, reviewed and approved by the owner in Lavish before the next phase starts. No app code, framework or package until phase U10, after the owner picks the stack.

Each phase runs: `ux-designer` writes a proposal (`design/proposals/`) with the `uix` skill → owner decides each item in Lavish → the agent applies only the approved items to the mockup → owner approves the finished mockup in Lavish. Later improve passes follow the same propose → decide → apply loop.

Every phase must meet:

- Screens designed mobile first (360–430 px), checked at 768 px and desktop.
- Loading, empty, error, offline and API-refusal states drawn for each screen.
- Each screen lists the API endpoints it uses, taken from `../bayanihan-api/docs/api/openapi.yaml`.
- Fake Dumaguete data only (names, barangays, points); no real personal data; private fields shown only where the API returns them to that user.
- Owner approval in Lavish recorded in `.claude/state/<run-id>/status.md`.

---

## U0 — Design foundation and PWA shell

Status: approved 2026-11-25 — `design/u0-foundation.html`, decisions in `docs/decisions/0002-design-foundation.md`.

Colors, type scale, spacing and radius tokens (light and dark); core components (buttons, inputs, cards, badges, tier and standing chips, points/peso amounts, bottom navigation, sheets, toasts, dialogs); app shell with bottom nav; PWA pieces (app icon, splash and theme color, install prompt, offline banner, update-available prompt); shared states (skeleton loading, empty, error, 429 with retry time, 409 refusal).

Acceptance: one `design/u0-foundation.html` with every token and component, both themes, and the shell at three widths.

## U1 — Sign-in and verification

Status: approved 2026-11-25 — `design/u1-signin.html`, decisions in `docs/decisions/0003-signin-and-verification.md`.

Phone entry and SMS OTP (with resend timer and OTP rate limit), profile completion to reach tier 1 (name, photo, barangay, bio), tier explainer (what each tier can do and its posting cap), ID verification upload for tier 2 (ID + selfie), pending / approved / rejected states, 90-day re-verification and dormant-review notices.

## U2 — Browse and task detail

Status: approved 2026-11-25 — `design/u2-browse.html`, decisions in `docs/decisions/0004-browse-and-task-detail.md`.

Task feed as a list and a map (approximate locations only), search and filters (category, distance, points), task detail for a non-party (approximate area, poster's public profile, points offer), quote/fee preview.

## U3 — Post a task

Status: approved 2026-11-25 — `design/u3-post.html`, decisions in `docs/decisions/0005-post-a-task.md`.

Post form (title, category, description, photos, schedule, location pin, points), fee and escrow breakdown before confirming, tier cap errors, insufficient points with a route to cash in, and the blocked states from `/me` `can_post` + `post_blocked_reason` (suspended, closed, tier_too_low, dormant_review, reverify_required).

## U4 — Applying and choosing a worker

"I can do this" apply flow and limits, my applications, the poster's applicant list (public profiles, ratings, verified badge, tasks done; ordered by the API, which puts better standing first, but standing itself is never shown to others), picking one worker, withdrawing and cancelling, pick reminders.

## U5 — Doing the work and chat

Assigned-task view for both sides (exact pin shown only to the poster and the accepted worker, hidden after completion), start task, photo proof upload, confirm or request changes, cancel request, auto-release countdown, task chat (text, images, read markers, rate-limit refusal).

## U6 — Ratings, disputes and reports

Two-way rating after completion, opening a dispute with evidence, dispute status and decision, reporting a user or task.

## U7 — Wallet

Balance (available and locked), ledger history, cash-in (fake gateway for now: create, wait, poll, expire), cash-out request and its states (pending, held for review, paid, failed and refunded), payout accounts with masked numbers, tier-2 requirement for cash-out.

## U8 — Notifications, profile and settings

Notification list and push permission prompt, own profile and edit, skills, public profile of another user, standing and its meaning, signed-in devices and sign-out everywhere, language (English / Filipino).

## U9 — Clickable prototype and handoff

Link the approved screens into click-through flows for the core loop (post → apply → pick → start → proof → confirm → rate → cash out), an accessibility pass, and a handoff table mapping every screen and state to its endpoint and error codes, plus a list of API gaps found during design.

## U10 — Stack decision (owner)

Choose the PWA stack and hosting, then write the implementation phases. Inputs: the approved designs, the API gaps list, and the PWA items the API needs (see decision 0001).
