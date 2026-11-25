---
name: uix
description: Bayanihan app UX/UI system and review checklist. Use whenever designing, critiquing or improving any screen or mockup of the Bayanihan PWA (design/*.html), choosing tokens or components, writing UI copy, or checking a design against PWA, accessibility, privacy and API rules. Trigger words - design, mockup, screen, UX, UI, polish, improve the look, review the design, uix.
---

# Bayanihan UIX

The Bayanihan app is a mobile-first PWA for neighbors in Dumaguete City who post small paid tasks and do them for points (1 point = ₱1). Most users are on mid-range Android phones, mobile data, often outdoors, often one-handed. Design for them first.

Read before any design work: `CLAUDE.md`, `docs/BUILD_PLAN.md`, `docs/decisions/`, and the relevant parts of `../bayanihan-api/docs/spec.md` and `../bayanihan-api/docs/api/openapi.yaml`.

## Two modes, always proposal first

**Design** a new screen or phase mockup, or **improve** an existing one. Either way, nothing is applied until the owner has approved a proposal in Lavish.

1. **Propose.** Write `design/proposals/<target>-proposal.html` (self-contained, same rules as mockups):
   - Improve: the critique (checklist findings ranked HIGH/MED/LOW, each pointing at the exact element), then each proposed fix as a before/after pair side by side at phone width, numbered so the owner can comment per fix.
   - Design: the flow (screens and the states each needs) and the key screens drawn at phone width; where there is a real design choice, two or three options side by side with a recommendation.
   - End with "Decisions needed" (what the owner must pick) and "API gaps" if any.
   - Do not edit the target mockup in this step.
2. **Review.** The proposal goes to the owner in Lavish (`npx -y lavish-axi design/proposals/<target>-proposal.html`, then poll). The owner approves, rejects or changes each numbered item.
3. **Apply.** Apply only the approved items to `design/<target>.html`, then give a before/after summary naming each applied fix and each rejected one left out. The finished mockup goes through Lavish again for final approval.

## Design system

Mirror shadcn/ui + KokonutUI so every mockup piece maps to a real component later. Tokens live as CSS custom properties on `:root`, redefined under `@media (prefers-color-scheme: dark)`.

- **Color:** neutral stone scale for surfaces and text; one brand color (teal) for primary actions and points; semantic success / warning / danger with soft backgrounds. Text contrast at least 4.5:1, large text and icons 3:1.
- **Type:** system UI stack. Scale 12 / 14 / 16 / 18 / 22 / 28. Body 16 px on mobile (prevents zoom on input focus). Numbers in points use tabular figures.
- **Spacing:** 4 px grid (4, 8, 12, 16, 24, 32). Screen gutter 16 px.
- **Radius:** 8 inputs, 12 cards, 999 pills and the primary floating action.
- **Elevation:** borders first; one soft shadow level for sheets and dialogs only.
- **Motion:** 150–250 ms ease-out, for state change only (sheet in, toast, button press). Respect `prefers-reduced-motion`.
- **Components** (name the shadcn/KokonutUI counterpart in a comment-free legend at the bottom of each mockup): Button, Input, Textarea, Select, Card, Badge, Avatar, Tabs, Sheet (bottom), Dialog, Toast, Skeleton, Empty state, Inline alert, Bottom navigation, Points amount, Tier chip, Standing chip, Countdown.

## Checklist (every screen)

Rank findings HIGH / MED / LOW. HIGH = a user can't complete the task, loses money or sees data they shouldn't.

1. **Task first.** One primary action per screen, visible without scrolling, in thumb reach (bottom third). Secondary actions are quieter.
2. **Money is never a surprise.** Points shown with peso value; fee (5%, min ₱10) and what lands in escrow shown before the commit button; balances say available vs locked.
3. **States.** Loading (skeleton, not spinner-only), empty (says what to do next), error (says what happened and how to retry), offline (what still works), and every API refusal the screen can hit: 401 sign in again, 403, 404, 409 (e.g. `ACCOUNT_REVIEW_PENDING` with its reason, invalid task state), 410 image deleted, 422 field errors inline, 429 with the `Retry-After` time.
4. **Trust and privacy.** Never show another user's phone, legal name, birth date, ID images, or a task's exact pin unless the API returns it to this user (poster and accepted worker only, until hidden). Approximate areas on maps. Show tier and standing where trust decisions are made (applicants, profiles).
5. **Touch and layout.** Targets ≥ 44×44 px with ≥ 8 px between; no horizontal scroll at 360 px; long Filipino strings fit; safe-area insets on the installed app; inputs use the right keyboard (`inputmode`, `autocomplete`).
6. **Accessibility.** Labels on every input, visible focus ring, meaning never by color alone, headings in order, touch and screen-reader friendly order, alt text on task photos.
7. **PWA.** Install prompt at a good moment (after a first success, not on first load); offline banner; update-available prompt; push permission asked in context (e.g. after posting), never on load.
8. **Copy.** Plain, short, second person, friendly; verbs on buttons ("Post task", "I can do this"); errors blame the system, not the user; no jargon (say "points held" not "escrow" in UI, explain once).
9. **API fit.** Every field and action exists in `openapi.yaml`; the screen lists its endpoints; anything missing goes on the API gaps list, never faked.
10. **Consistency.** Same component for the same job across screens; tokens only, no one-off colors or sizes.

## Output

- Mockups: self-contained HTML in `design/` (inline CSS/JS, no external assets or CDNs), fake Dumaguete data, several phone-width frames side by side for the states, plus 768 px and desktop checks.
- Proposals: `design/proposals/<target>-proposal.html`, kept after approval as the record of what was decided.
- Then the Lavish loop from `CLAUDE.md` (`npx -y lavish-axi design/<file>.html`, poll, apply, reply) until the owner approves. Never `lavish-axi share`.
