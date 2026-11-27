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

Status: approved 2026-11-26 — `design/u4-apply.html`, decisions in `docs/decisions/0006-applying-and-choosing.md`.

"I can do this" apply flow and limits, my applications, the poster's applicant list (public profiles, ratings, verified badge, tasks done; ordered by the API, which puts better standing first, but standing itself is never shown to others), picking one worker, withdrawing and cancelling, pick reminders.

## U5 — Doing the work and chat

Status: approved 2026-11-26 — `design/u5-work.html`, decisions in `docs/decisions/0007-doing-the-work-and-chat.md`.

Assigned-task view for both sides (exact pin shown only to the poster and the accepted worker, hidden after completion), start task, photo proof upload, confirm or request changes, cancel request, auto-release countdown, task chat (text, images, read markers, rate-limit refusal).

## U6 — Ratings, disputes and reports

Status: approved 2026-11-26 — `design/u6-trust.html`, decisions in `docs/decisions/0008-ratings-disputes-reports.md`.

Two-way rating after completion, opening a dispute with evidence, dispute status and decision, reporting a user or task.

## U7 — Wallet

Status: approved 2026-11-26 — `design/u7-wallet.html`, decisions in `docs/decisions/0009-wallet.md`.

Balance (available and locked), ledger history, cash-in (fake gateway for now: create, wait, poll, expire), cash-out request and its states (pending, held for review, paid, failed and refunded), payout accounts with masked numbers, tier-2 requirement for cash-out.

## U8 — Notifications, profile and settings

Status: approved 2026-11-27 — `design/u8-me.html`, decisions in `docs/decisions/0010-notifications-profile-settings.md`.

Notification list and push permission prompt, own profile and edit, skills, public profile of another user, standing and its meaning, signed-in devices and sign-out everywhere, language (English / Filipino).

## U9 — Clickable prototype and handoff

Status: approved 2026-11-27 — `design/u9-prototype.html`, `design/u9-handoff.html` and `docs/api-gaps.md`, decisions in `docs/decisions/0011-prototype-and-handoff.md`. Accessibility fixes applied to the shared CSS in u0–u8.

Link the approved screens into click-through flows for the core loop (post → apply → pick → start → proof → confirm → rate → cash out), an accessibility pass, and a handoff table mapping every screen and state to its endpoint and error codes, plus a list of API gaps found during design.

## U10 — Stack decision (owner)

Status: decided 2026-11-27 — React 19 + Vite single-page PWA with KokonutUI/shadcn, Sanctum cookie sign-in on app./api. subdomains, hosted beside the API for the pilot. Decisions in `docs/decisions/0012-stack.md`.

Choose the PWA stack and hosting, then write the implementation phases. Inputs: the approved designs, the API gaps list, and the PWA items the API needs (see decision 0001).

---

# Implementation phases

Each phase is built against the approved mockup it names, follows the `uix` checklist and the accessibility build notes in `design/u9-handoff.html#a11y`, and is reviewed by the owner before the next starts. Packages are limited to the list approved in decision 0012. API work happens in `../bayanihan-api` under its own workflow and rules.

## I1 — API prerequisites (in bayanihan-api)

P1 browser access (Sanctum `statefulApi()`, stateful domains, a published `config/cors.php` with only the app origin and credentials, OTP verify starts a session, sign-out and "Sign out everywhere" end it, long phone sessions, cookie scope that doesn't widen the admin session, `REVERB_ALLOWED_ORIGINS`), P2 `Retry-After` on every 429 exposed through CORS, P3 IP-keyed limits behind carrier NAT. Needs the owner's domain.

Done when: from the app's origin, a browser signs in by OTP, `GET /me` answers 200 from the cookie, a POST without the CSRF header is refused, and a 429's retry time is readable in script.

## I2 — Repo scaffold and app shell

Vite + React + TypeScript, U0 tokens in Tailwind 4, Plus Jakarta Sans, Lucide, bottom tabs, routes, light/dark following the phone, lint/test/CI on GitHub Actions.

Done when: the shell matches `design/u0-foundation.html` at 360, 768 and desktop in both themes, and CI is green on the first push.

## I3 — PWA layer

Manifest, icon and splash, service worker, offline banner, update-ready prompt, install sheet (Android button, iPhone steps).

Done when: it installs on Android and iPhone, opens offline to the shell with the offline banner, and shows "update ready" after a new deploy.

## I4 — Sign-in and verification (U1)

Done when: every U1 state, including 401, 422 and 429, passes in Playwright against the staging API.

## I5 — Browse, task detail and posting (U2, U3)

Includes the map, photo compression, the local draft and Idempotency-Key retries. Needs P4 (barangay and category lists).

Done when: a post with photos uploads each under 300 KB, and "Try again" after a dropped connection holds the points once.

## I6 — Applying, choosing, work and chat (U4, U5)

Done when: two browsers run apply → pick → start → proof, with chat messages arriving live in both.

## I7 — Ratings, disputes, reports and wallet (U6, U7)

Against the FakeGateway. Needs P6 (dispute result on the task) before the pilot.

Done when: confirm → rate → cash out completes, and a repeated cash-out request with the same key creates one cash-out.

## I8 — Me, notifications, Filipino, accessibility and production

Done when: the U9 core loop passes in both languages with zero serious axe findings, served over HTTPS from the chosen host. Needs P5 (consent recorded) before the pilot.
