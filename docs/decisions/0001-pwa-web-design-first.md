# 0001 — A PWA, designed on the web first

Date: 2026-11-23. Status: owner decision.

## Decision

- The user app is a mobile-first Progressive Web App in its own repository (`bayanihan-app`), separate from the API (`bayanihan-api`).
- Work starts with web design: HTML mockups reviewed in Lavish (phases U0–U9). No Flutter and no app framework yet; the stack is chosen in U10.

## Conflicts with the backend docs (flagged, not changed)

- `bayanihan-api/docs/spec.md` §1 lists "web app" under **Not in V1**, and §8 names a Flutter Android app (`google_maps_flutter`, `geolocator`). This decision changes both. The spec is not edited yet; the owner decides when.

## What a PWA will need from the API later (not built yet)

- Browser access to `/api/v1`: CORS for the app's origin, and a decision on how the browser holds the Sanctum token (or a move to Sanctum's cookie-based SPA auth).
- Web push instead of FCM-only device registration (`POST me/devices`).
- Maps in the browser instead of the Maps SDK for Android.
- Image compression before upload (spec: ~300 KB) done in the browser.
- Rate limits keyed by IP (`health`, OTP) see many users behind one carrier NAT; check them against real PWA traffic.

## Planned component library: KokonutUI (owner, 2026-11-23)

- https://github.com/kokonut-labs/kokonutui — MIT licence (free for commercial use; keep the copyright notice in copied source). Checked 2026-11-23: active, ~2.4k stars.
- KokonutUI **Pro** (kokonutui.pro) is a separate paid product. Use only the free registry unless the owner decides otherwise.
- It is React + Tailwind CSS + shadcn/ui + Motion, installed with the shadcn CLI into the codebase. Choosing it effectively picks React with Tailwind and shadcn/ui for U10; still the owner's call there, and no package is installed during the design phases.
- The HTML mockups (U0–U9) mirror its components' look and behaviour so the build can map each mockup component to a KokonutUI or shadcn/ui component.
