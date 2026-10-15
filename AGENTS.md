# Dates Slovenia — Persistent Codex Project Rules

## Product truth
Dates Slovenia is a **Slovenian date-activity discovery app** for new and established couples, not a Tinder-like service. It is Slovenia-only in V1. Default language: Slovenian (sl-SI). Price currency: EUR.

## Confirmed V1
- Curated Home with recommended/featured/category carousels.
- Browse/search and advanced filters: categories, budget/free, distance, duration, home/indoor/outdoor, commute suitability, preference-to-novelty slider.
- Location-aware Slovenia map pins for real geolocated ideas.
- Onboarding to collect interests and optional city/vibe/budget.
- Date details, Save for Later, local guest persistence, optional auth/backend sync.
- Secure editorial date database, simple founder content workflow.

## Not V1
No matchmaking, stranger chat, partner linking, shared lists, challenges, streaks, AI relationship coach, automated reminder engine, real bookings, or mandatory subscriptions. Add these only with explicit founder approval and a separate spec.

## Technology
Prefer Expo + React Native + TypeScript + Expo Router, Supabase Auth/Postgres/RLS/Storage, react-native-maps, expo-location, TanStack Query, AsyncStorage, and EAS. Before installing dependencies check current Expo compatibility. Do not add unnecessary infrastructure.

## Non-negotiable implementation behavior
- Ship **working interactions** rather than screens pretending to work.
- If credentials are absent, run clearly labeled DEMO MODE. Never pretend a fake purchase, login, server save, route time, or venue record is real.
- Real backend must enforce RLS: published content publicly readable; user saves/preferences user-private; editorial writes restricted. Never ship service-role secrets to mobile.
- Guests can browse and save. On login, safely merge guest bookmarks into their account.
- Foreground location is optional and requested only when useful; city selection works without it.
- At-home date ideas have no map pins or invented coordinates.
- No fabricated venue names, prices, operating hours, booking slots, images, or travel times.
- Only use photos/video with documented permission, ownership, or compatible licensing.
- Every new feature has empty/loading/error states, Slovenian UI copy, and relevant tests.
- Support account deletion for registered users before launching publicly.
- Do not implement/paywall the core app without founders deciding the premium offering.
- Keep external service credentials in env/secure backend; `.env.example` includes placeholders only.
- Make reasonable assumptions and write them to `docs/DECISIONS.md`.

## Workflow for Codex
1. Read `CODEX_MASTER_PROMPT.md` fully, then inspect current code.
2. Build sequential phases, starting with a polished functioning local prototype.
3. Prefer reusable typed services, pure/testable filter functions, and focused components.
4. After changes, run available typecheck, lint, unit tests, and device/Expo compatibility checks.
5. Report exactly what works, what remains, and what must be manually configured by the founders.
6. Maintain `README.md`, `docs/SETUP.md`, `docs/QA_CHECKLIST.md`, `docs/CONTENT_PLAYBOOK.md`, and `docs/ROADMAP.md`.

## UI principles
Warm, modern editorial look; no generic dating-site swiping, exaggerated hearts, or clutter. Build excellent cards, fast navigation, strong location + pricing clarity, and accessible touch targets. Draft palette is editable, not a final founder-approved brand.

## Launch principles
Actual production content must be editorially verified. Demo fixtures are labeled demo and kept out of production. Do not assert the app has been published or that tests passed unless they truly did.
