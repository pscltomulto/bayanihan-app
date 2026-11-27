# 0011 — Clickable prototype and handoff (U9)

Date: 2026-11-27. Status: approved by the owner in Lavish. References: `design/u9-prototype.html`, `design/u9-handoff.html`, `docs/api-gaps.md` (proposal: `design/proposals/u9-handoff-proposal.html`). Builds on decisions 0001–0010.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Prototype file | One self-contained file, `design/u9-prototype.html`, so state carries across the loop |
| 2 | Two people | Ana's and Ben's phones side by side from 768 px, an Ana / Ben switch below that, and a hand-over card (placed above the phones so keyboard order matches the screen) |
| 3 | Scope | The happy path sign-in → post → apply → pick → start → proof → confirm → rate → cash out, plus detours: wrong code, no signal while posting, finish your profile first, not quite done → dispute, payments not live, offline |
| 4 | Jumping | A step rail and a `#step-N` link per step; jumping sets balances as if earlier steps happened. Step 4 opens on Ana's applicant list, matching U4 (cards, star and Starred filter, applicant detail with questions, pick confirm sheet) — added after the owner asked where applicant selection was |
| 5 | Mechanics | One small JS state object, real buttons; no network, storage or packages |
| 6 | Widths | 360 px phones plus a 768 view; 430 and desktop stay as the approved mockups |
| 7 | Accessibility standard | WCAG 2.2 AA plus house rules: 44 px targets, points read aloud, Filipino length, reduced motion |
| 8 | What was checked | A static audit of u0–u8 in both themes, plus a hands-on keyboard and 200% zoom test of the prototype. VoiceOver and TalkBack weren't run (no device available) |
| 9 | Fixes | A findings table in `design/u9-handoff.html#a11y`. Token and component fixes applied to u0 and the shared CSS in u1–u8 (CSS only; screens, copy and layouts unchanged): new `--line-strong`, `--on-ok`, `--focus` tokens; dark unread counts use `--bad-fg`; 44 px hit areas on small controls; `min-height` instead of fixed heights; a global `:focus-visible` ring. Markup issues are build notes |
| 10 | Handoff format | Screens table in `design/u9-handoff.html`, gaps in `docs/api-gaps.md`, nothing written twice |
| 11 | Screens table | Grouped by phase (89 screens, 696 states) with endpoints and codes, plus an endpoint index flagging operations no screen uses |
| 12 | Gap list | Three tiers (blocks the pilot / before public launch / later), each gap tagged with area and status, plus Owner calls and Docs only lists. Tiers are proposals the owner can move |

## Accessibility results

- Mockups: 3 HIGH (all fixed), 6 MED (3 fixed, 3 build notes), 3 LOW (build notes), 5 pass.
- Prototype: 2 issues fixed (focus lost during photo upload; hand-over card order), 1 LOW build note.
- Build notes include: the mockup phones are pictures (`role="img"`) with no real controls; icon controls named with `aria-label` on plain spans; points need screen-reader text ("400 points, 400 pesos"); badges and chips should wrap at 200% text.

## Open items carried forward

- Owner calls (in `docs/api-gaps.md`): name change after ID approval keeps the ID-verified chip (0010); the API spec still says Flutter and "web app Not in V1" (0001); a real request-changes step (0007).
- Blocks the pilot: browser access to the API (CORS and how the browser holds the Sanctum token), a readable 429 retry time, rate limits keyed by IP behind carrier NAT, barangay and category lists, recorded consent, and the dispute result on the task.
- U4's endpoint table doesn't name `GET /applications/{application_ulid}/photos/{sort}`, though the applicant cards show past-work photos.
