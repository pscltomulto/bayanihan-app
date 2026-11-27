# API gaps for the PWA

Date: 2026-11-27. Phase U9 handoff (decisions 10 C and 12 B in `design/proposals/u9-handoff-proposal.html`).

One deduplicated list of what the backend (`../bayanihan-api`) doesn't do yet that the approved designs need. Sources: the "API gaps found" sections of decisions 0002–0010 and the "What a PWA will need from the API" list in 0001. The screen-by-screen endpoint map is in `design/u9-handoff.html`.

How to read it:

- **Tiers are proposals.** The owner can move any gap to another tier.
- **Area:** auth, tasks, applications, work/chat, trust, wallet, notifications, profile, docs, platform.
- **Status:** *open* (nothing in the design covers it), *worked around in the design* (the approved screens work without it, with less), *owner call* (needs a product decision first).
- **From:** the decision number(s) that raised it. Where several decisions raised the same gap, it appears once with all of them.

Counts: 6 block the pilot, 24 before public launch, 24 later, 3 owner calls, 5 docs only.

## Blocks the pilot

**P1. The browser can't reach the API yet.** The app needs CORS for its origin and a decision on how the browser holds the Sanctum token, or a move to Sanctum's cookie-based SPA auth. Nothing else works until this is done.
Area: platform · Status: open · From: 0001

**P2. A 429 retry time the browser can read.** Send `Retry-After` on every 429 and expose it through CORS, or add `errors.retry_after`. Chat 429s should also say which limit was hit (30 texts a minute or 20 photos an hour).
Area: platform · Status: open · From: 0002, 0003, 0004, 0005, 0006, 0007, 0008, 0009, 0010

**P3. Rate limits keyed by IP behind carrier NAT.** `health` and the OTP limits are keyed by IP, and many phones on one carrier share an address. Check them against real PWA traffic before the pilot, since a shared limit would lock people out of sign-in.
Area: auth · Status: open · From: 0001

**P4. Barangay and category lists.** There is no endpoint for either. Profile setup, the Browse filters, the post form and skills all need them.
Area: platform · Status: open · From: 0003, 0004, 0005, 0006, 0007, 0008

**P5. Consent isn't recorded.** Acceptance of the terms and privacy notice isn't stored, and `POST /me/verification` has no field for the separate ID/selfie consent.
Area: auth · Status: open · From: 0003

**P6. The dispute result isn't in the task.** The task's dispute block has no `outcome`, `worker_share` or `resolved_at`, so the decision card can't say who got what.
Area: trust · Status: open · From: 0008

## Before public launch

**L1. `tasks_done` on the person block.** The trust chips need it on the feed, the task detail and the task cards. Today it costs one `/users/{ulid}` call per person.
Area: tasks · Status: open · From: 0004, 0007, 0008

**L2. Full and closed look the same to others.** People other than the poster can't tell a full task from a closed one, because `applications_closed_reason` is sent to the poster only.
Area: tasks · Status: open · From: 0004

**L3. `/tasks/quote` doesn't return the minimum offer.** The post form has to quote the minimum in its copy, which can go stale.
Area: tasks · Status: open · From: 0005

**L4. No notice when a held task is approved or hidden.** A poster whose task was held for review isn't told the result.
Area: notifications · Status: open · From: 0005

**L5. Tier caps and ID image retention can't be read.** Copy that quotes them is written into the app and can go stale.
Area: profile · Status: open · From: 0003

**L6. ID rejection details.** An `other` rejection has no note the user can act on, and `underage` has no `can_resubmit` flag.
Area: trust · Status: open · From: 0003

**L7. A live selfie isn't enforced.** The API accepts any image as the selfie.
Area: trust · Status: open · From: 0003

**L8. Will cancelling cost standing?** The poster can't tell whether cancelling now falls inside the free first hour, so the dialog can only say it "can lower" standing. Add `cancel_lowers_standing` or `free_cancel_until` to the poster's `GET /tasks/{ulid}`.
Area: applications · Status: worked around in the design · From: 0006

**L9. Closed applications lead nowhere.** `GET /me/applications` rows carry only the task's title, points and deadline, and a closed task's detail is 404 to the applicant, so a closed row can't open anything.
Area: applications · Status: open · From: 0006

**L10. My tasks and My jobs cards are too thin.** `/me/tasks` and `/me/jobs` cards have no other-person name, `auto_release_at`, unread count, pending cancel request, `can_rate` or `rate_until`.
Area: tasks · Status: worked around in the design · From: 0007, 0008

**L11. `hidden_at` is never sent.** The worker can't be told when the exact pin will lock.
Area: work/chat · Status: open · From: 0007

**L12. Declining a cancel request sends no notice.** The person who asked isn't told the answer.
Area: notifications · Status: open · From: 0007

**L13. Decided disputes have no amounts.** A decided dispute has no `fee`, `worker_receives` or `poster_refund`, so the app would have to compute the fee itself.
Area: trust · Status: open · From: 0008

**L14. The rating block has no `reveal_at`.** The app can't say when a hidden rating becomes visible without computing the 14-day window itself.
Area: trust · Status: open · From: 0008

**L15. Evidence photo count.** The dispute doesn't say how many evidence photos are allowed or left, which "Add photos · N left" needs.
Area: trust · Status: open · From: 0008

**L16. No fee quote before paying.** `amount_charged_centavos` only comes back after `POST /wallet/cash-ins`, so the user can't see the total before committing.
Area: wallet · Status: open · From: 0009

**L17. No list of my cash-ins or cash-outs.** An unfinished one can only be found from the ulid saved on the phone.
Area: wallet · Status: open · From: 0009

**L18. Cash-out failure times.** Cash-outs have no `failed_at`, `refund_by` or `refunded_at` for the status timeline.
Area: wallet · Status: open · From: 0009

**L19. History rows are bare.** Cash-in and cash-out rows in the history give only the ulid: no channel, masked number, net amount or status.
Area: wallet · Status: open · From: 0009

**L20. Staff corrections have no reason.** An `adjustment` row has no reason the user can read.
Area: wallet · Status: open · From: 0009

**L21. Notification text is English only.** Notifications are stored in English, and Filipino is in scope.
Area: notifications · Status: open · From: 0010

**L22. Notices link only to a task.** Notifications carry only `task_ulid`, so cash-in, cash-out, dispute and question notices can't open their own screen.
Area: notifications · Status: worked around in the design · From: 0010

**L23. No `cash_in_failed` notification.** A failed cash-in isn't announced.
Area: notifications · Status: open · From: 0010

**L24. No self-service account closing.** "Close my account" can only point to Help.
Area: profile · Status: worked around in the design · From: 0010

## Later

**R1. Web push.** `POST me/devices` takes FCM tokens only. The PWA needs a web push subscription endpoint and a way to remove a push device without signing out. Push shows "Coming soon" until then.
Area: notifications · Status: worked around in the design · From: 0001, 0002, 0010

**R2. Maps in the browser.** The spec plans the Maps SDK for Android; the PWA needs a browser map for the approximate-area map strip and map view. The list works without it.
Area: platform · Status: open · From: 0001

**R3. Image compression in the browser.** The ~300 KB target (spec) is met in the browser before upload; the API keeps its size limits. Client work, listed so it isn't lost.
Area: platform · Status: open · From: 0001

**R4. Unread count without the full list.** The count only comes with the notifications list; the bell asks for one item to get it.
Area: notifications · Status: worked around in the design · From: 0002, 0010

**R5. Category icons use Material names.** `category.icon` holds Material icon names and the app uses Lucide, so the app needs a mapping.
Area: tasks · Status: worked around in the design · From: 0002

**R6. No text search on `GET /tasks`.** The design has no search until `q` exists.
Area: tasks · Status: worked around in the design · From: 0004

**R7. The map gets one feed page.** At most 50 tasks; fine for the pilot.
Area: tasks · Status: worked around in the design · From: 0004

**R8. The open-task limit can't be read before the form.** The 5-open-task limit (`LIMIT_REACHED`) isn't on `/me`, so it only shows up as a 409 at posting.
Area: tasks · Status: worked around in the design · From: 0005

**R9. A task photo can't be deleted.**
Area: tasks · Status: open · From: 0005

**R10. A task can't be edited after posting.** Spec §6 runs the keyword filter "on post and edit", but there is no edit. The design has no edit after posting.
Area: tasks · Status: worked around in the design · From: 0005

**R11. Barangay and service area only on posting.** The barangay and the `OUTSIDE_SERVICE_AREA` check come back only when the post is sent.
Area: tasks · Status: worked around in the design · From: 0005

**R12. `expired` has three causes.** It covers deadline passed, poster cancelled and staff removed, so the app can't say which happened.
Area: applications · Status: open · From: 0006

**R13. Pre-pick questions have no unread signal.** They have counts only.
Area: applications · Status: open · From: 0006

**R14. Both pick reminders look the same.** They share the `pick_reminder` kind with text only, so the app can't tell them apart.
Area: notifications · Status: worked around in the design · From: 0006

**R15. Live updates beyond chat.** Only `message.sent` is broadcast. Task status changes reach an open task page only as system chat messages, and dispute changes aren't broadcast at all.
Area: work/chat · Status: worked around in the design · From: 0007, 0008

**R16. No read receipts.** The other side's read marker isn't returned and no read event is broadcast. The design shows no "Seen" ticks.
Area: work/chat · Status: worked around in the design · From: 0007

**R17. Before photos only with Start.** The API takes before photos only in the Start request.
Area: work/chat · Status: worked around in the design · From: 0007

**R18. No list of my reports.** The owner chose none for now.
Area: trust · Status: worked around in the design · From: 0008

**R19. History has no `kind` filter.** The phone filters the loaded pages until `GET /wallet/history` has one.
Area: wallet · Status: worked around in the design · From: 0009

**R20. `limit_reached` covers two limits.** `cash_in.blocked_reason` `limit_reached` means either the daily limit or 3 open payments.
Area: wallet · Status: open · From: 0009

**R21. Devices.** There is no device list and no single-device sign-out; the design offers "Sign out everywhere" only.
Area: auth · Status: worked around in the design · From: 0010

**R22. `GET /users/{ulid}` has no skills.** The public profile shows skills only once the API returns them.
Area: profile · Status: worked around in the design · From: 0010

**R23. Standing history.** There is no standing history, and the +2 is never reported.
Area: profile · Status: open · From: 0010

**R24. No version on profile edits.** Two phones editing the profile: the last save wins.
Area: profile · Status: open · From: 0010

## Owner calls

**O1. Name change after ID approval keeps the "ID verified" chip.** A user can change their name after the ID is approved and keep the chip, so neighbors could see a verified name that no longer matches the ID. Options: lock the name after approval, or drop the chip until the ID is re-reviewed. The mockups show today's behavior.
Area: trust · Status: owner call · From: 0010

**O2. The API spec still says Flutter and "web app: Not in V1".** `bayanihan-api/docs/spec.md` §1 lists "web app" under Not in V1 and §8 names a Flutter Android app; decision 0001 changed both. When is the spec updated?
Area: docs · Status: owner call · From: 0001

**O3. A real request-changes step.** There is no request-changes status or endpoint, and nothing pauses `auto_release_at`. Decision 0007 #7 works without it (chat, then Report a problem); the owner may revisit.
Area: work/chat · Status: owner call · From: 0007

## Docs only

Fixes to `bayanihan-api/docs/api/openapi.yaml`. No behavior changes.

**D1. Task detail example.** The `GET /tasks/{ulid}` example leaves out `my_application`, shows only the public view (not the poster's or worker's), and lacks the dispute and rating blocks.
Area: docs · Status: open · From: 0004, 0007, 0008

**D2. Applying errors.** The apply docs leave out errors the code returns: `TIER_TOO_LOW`, `ACCOUNT_SUSPENDED` / `ACCOUNT_CLOSED`, `TASK_UNDER_REVIEW`, `INVALID_TRANSITION`, `APPLICATIONS_CLOSED` with reason accepted, and both `earliest_start_at` 422s. `GET /applications/{ulid}` has no 200 example.
Area: docs · Status: open · From: 0006

**D3. Chat docs.** Message examples leave out `type`, and the channel is `task.{ulid}.{worker_ulid}`, not `task.{ulid}`.
Area: docs · Status: open · From: 0007

**D4. Ratings and disputes errors.** 403 `ACCOUNT_SUSPENDED` and the 422 field errors are missing.
Area: docs · Status: open · From: 0008

**D5. Wallet failure reasons.** Cash-in and cash-out `failure_reason` values aren't documented.
Area: docs · Status: open · From: 0009
