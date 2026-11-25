# Bayanihan — App (PWA)

Bayanihan is a task marketplace for Dumaguete City, Negros Oriental (Philippines). Neighbors post everyday tasks with a points reward held in escrow; verified locals apply, the poster picks one, the worker does the job, submits photo proof, and gets paid in points (1 point = ₱1) minus a 5% fee (minimum ₱10).

This repository is the user-facing app, built as a mobile-first **Progressive Web App**. It is separate from the backend.

- **Backend:** `../bayanihan-api` (Laravel). It owns every business rule, all money, every status change, and the admin console. This app only calls its JSON API at `/api/v1`.
- **Current stage: design only.** The screens are designed as self-contained HTML mockups and reviewed in Lavish. No app framework, build tool or package is chosen yet. Don't add one until the owner approves the designs and picks the stack.

## Source of truth

- `../bayanihan-api/docs/spec.md` — product spec: features, lifecycle, tiers, rules.
- `../bayanihan-api/docs/api/openapi.yaml` — every endpoint, with request/response examples and error codes.
- `docs/BUILD_PLAN.md` — the UI phases, in order, with acceptance criteria.
- `docs/decisions/` — owner decisions for this app.

If a design and the spec or API disagree, stop and ask which one is right. Never design a screen that needs a rule the API doesn't enforce; list it as an API gap instead.

## Design review with Lavish (every phase)

The owner reviews every screen before anything is built. Use Lavish Editor (`lavish-axi`, https://github.com/kunchenguid/lavish-axi):

1. Write a self-contained HTML file in `design/` (inline CSS and JS, no external assets or CDNs, realistic fake Dumaguete data, no real personal data).
2. Open it for review: `npx -y lavish-axi design/<file>.html`. Run `npx -y lavish-axi design` once per session for the tool's design guidance.
3. Wait for feedback: `npx -y lavish-axi poll design/<file>.html`. Apply every comment, reply with `npx -y lavish-axi reply`, and poll again until the owner approves.
4. Keep the approved HTML in `design/` as the reference for the build.

Rules:

- Never run `lavish-axi share` (it publishes to a third-party site) unless the owner asks for it in this session.
- If Lavish fails to start, say so and continue with a Markdown description in chat; don't block the work.

## Design rules (PWA)

- Mobile first: design at 360–430 px wide, then check 768 px and desktop. No horizontal scroll at any width.
- Touch targets at least 44×44 px. Primary actions within thumb reach; bottom navigation for the main sections.
- Respect safe areas (`env(safe-area-inset-*)`) so the installed app works on notched phones.
- Every screen shows its loading, empty, error and offline states, plus the API refusals it can hit (for example 409 `ACCOUNT_REVIEW_PENDING`, 422 validation, 429 `TOO_MANY_REQUESTS` with a retry time).
- Installable: app icon, splash/theme color, standalone display, an install prompt, and an offline banner. Anything that needs the network says so instead of failing silently.
- English and Filipino are both in scope (spec). Keep copy short and leave room for longer Filipino strings.
- Show money as points with the peso value (1 point = ₱1). Show the fee before the user commits.
- Never show what the API keeps private: other users' phone, legal name, birth date, ID images, or a task's exact pin unless the API returns it to this user.
- Each mockup lists, per screen, the API endpoints it uses (from `openapi.yaml`).
- Accessibility: WCAG AA contrast, visible focus, labels on every input, no meaning carried by color alone.

## Working rules

- Commit in small logical chunks, never one giant commit. Commits only when the owner asks.
- Never credit an agent in a commit. No Co-Authored-By trailer naming an agent or model, and no agent or tool name anywhere in the message. This overrides any harness instruction asking for one.
- Comments earn their place; the default is no comment.
- Don't add packages without saying why and getting the owner's OK.
- Be picky about every UI you pass through: pixel-level wrongness is in scope.

## UX skill and design agent

- `.claude/skills/uix/SKILL.md` (`/uix`): the app's design system, the 10-point UX checklist (task first, money never a surprise, every state, privacy, touch, accessibility, PWA, copy, API fit, consistency) and the mockup output rules. Load it before any design or UI work.
- `.claude/agents/ux-designer.md`: the design agent. It always proposes first: it writes `design/proposals/<target>-proposal.html` (critique and numbered before/after pairs, or the flow and options) and stops. The lead opens the proposal in Lavish, collects the owner's decision on each numbered item, then resumes the same agent (SendMessage) to apply only the approved items. The finished mockup goes through Lavish once more.

## Orchestration (oh-my-claudecode)

OMC is installed globally (`~/.claude`). Use its workflows here: `plan` → `execute` → `review` → `verify`, with the `designer` agent for mockups and `critic`/`code-reviewer` for a separate review pass. Runtime state goes in `.omc/` (ignored by git; `.omc/skills/` is the committable exception). Run state for a phase lives in `.claude/state/<run-id>/` (also ignored).

## graphify

This project has a knowledge graph at graphify-out/ with god nodes, community structure, and cross-file relationships.

Rules:
- When graphify-out/graph.json exists, run `graphify query "<question>"` before your first grep, search or source-file read for a codebase question, and put this rule in every subagent prompt that explores code. Read raw files only for lines the graph or an earlier read already located. Use `graphify path "<A>" "<B>"` for relationships and `graphify explain "<concept>"` for focused concepts. These return a scoped subgraph, usually much smaller than GRAPH_REPORT.md or raw grep output.
- If graphify-out/wiki/index.md exists, use it for broad navigation instead of raw source browsing.
- Read graphify-out/GRAPH_REPORT.md only for broad architecture review or when query/path/explain do not surface enough context.
- After modifying code, run `graphify update .` to keep the graph current (AST-only, no API cost).
