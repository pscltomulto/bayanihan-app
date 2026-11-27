# 0012 — PWA stack (U10)

Date: 2026-11-27. Status: approved by the owner in Lavish ("accepted": every recommended option and the package list). Reference: `design/proposals/u10-stack-proposal.html` (sources and versions checked 2026-11-27). Builds on decisions 0001–0011.

## Decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Framework | React 19 + Vite, with KokonutUI and shadcn/ui components copied in as source (MIT notices kept). KokonutUI is React-only, so the app is React while the admin stays Vue |
| 2 | Rendering | A client-only single-page app served as plain files; no server rendering (everything is behind sign-in) |
| 3 | Offline and install | vite-plugin-pwa (manifest, icons, Workbox service worker) with an "update ready" prompt. iPhone limits accepted: no install event (show steps instead), push only from the Home Screen on iOS 16.4+, storage may be cleared after 7 days unused, no background sync |
| 4 | Sign-in | Sanctum cookie (SPA) auth with the app on `app.<domain>` and the API on `api.<domain>`; no bearer token in browser storage (today's tokens never expire and would be readable by any injected script) |
| 5 | Data and chat | TanStack Query for loading and caching; Laravel Echo + pusher-js for Reverb chat; money retries only by hand with the same Idempotency-Key, never automatic |
| 6 | Language and forms | i18next for English / Filipino; React Hook Form + Zod, with API 422 errors mapped onto fields |
| 7 | Maps and photos | MapLibre with OpenFreeMap tiles for approximate-area maps; photos compressed in the browser with our own canvas code (no package) |
| 8 | Testing and quality | TypeScript (pinned 6.0.x for typescript-eslint), Vitest + Testing Library + MSW, Playwright + axe for the core loop, ESLint 9 with react-hooks and jsx-a11y, Prettier |
| 9 | Hosting | A second site on the API's VPS for the pilot (HTTPS), Cloudflare Pages as the fallback; CI on GitHub Actions |

## Packages approved

42 packages, all free and open source, none from KokonutUI Pro: 20 that ship to phones and 22 for build and test. The full list with versions, reasons and licences is in the proposal's "Packages this would add" table. Versions are re-checked at install time in I2; adding anything outside that list still needs the owner's OK.

## Still needed from the owner

- The domain to register (the proposal uses `app.example.ph` / `api.example.ph` as placeholders). Needed for I1's cookie and CORS setup and for I8's deploy.
- The owner calls in `docs/api-gaps.md` (O1 name change after ID approval, O2 spec says Flutter, O3 request-changes step). O2 should be settled now: `bayanihan-api/docs/spec.md` §1 and §8 (including maps) should say PWA.

## Not verified when deciding

- Free-plan limits of Cloudflare Pages / Vercel / Netlify and latency from the Philippines.
- The Safari 7-day storage rule comes from a vendor write-up, not Apple.
- Only 4 KokonutUI components were sampled for Next.js-only imports; each component is checked when copied in.
- Whether Laravel's session cookie can be scoped so the admin session isn't widened to the app's address — checked in I1.
