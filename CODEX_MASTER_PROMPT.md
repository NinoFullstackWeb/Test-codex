# DATES SLOVENIA — COMPLETE CODEX BUILD INSTRUCTIONS

> **How to use this file:** Open a new project in Codex, make this file available in its repository, and tell Codex: **"Read CODEX_MASTER_PROMPT.md and AGENTS.md. Start Phase 0 and Phase 1. Implement rather than only designing screens. Continue incrementally, test each phase, and explain every action that requires my accounts or credentials."**
>
> **Working product name:** Dates Slovenia (renameable). **Primary market:** Slovenia. **Default language:** Slovenian (sl-SI). **Target:** iOS + Android mobile app. **Version:** V1 MVP. **Product owners:** two nontechnical founders.

---

## 1. Your job as Codex

You are the lead product engineer, mobile UX designer, backend architect, and QA engineer for Dates Slovenia. Build a functioning, maintainable MVP with real flows and a database—not a static design prototype.

**Operating rules:**

1. Read the entire specification and the project `AGENTS.md` before writing code. If the repository already contains code, inspect it and preserve what works; do not blindly overwrite.
2. Implement in small, verifiable phases. At the end of every phase, run available checks, fix failures, summarize changed files, and list manual setup tasks. Continue to the next phase if possible, rather than waiting for approval for ordinary implementation choices.
3. Make reasonable product/technical decisions without burdening nontechnical founders with trivial questions. Record meaningful choices in `docs/DECISIONS.md`.
4. Never pretend an integration works when it does not. Distinguish **demo mode**, **real backend mode**, and **production ready**.
5. Never create fake business prices, opening hours, availability, ratings, travel times, or venue endorsements and pass them off as verified facts.
6. Do not hardcode private keys, service-role tokens, Apple credentials, payment secrets, or privileged APIs into the client. Use `.env.example` and document where founders must supply credentials.
7. Favor reliability, accessibility, smooth UX, clear code, and low running costs over clever architecture. Use stable libraries compatible with the current Expo SDK, consulting official docs before installation.
8. When external setup or approvals prevent completion (store accounts, database credentials, location keys), build the local portion, create exact setup instructions, and clearly label the blocker. Do not claim a store submission or purchase is complete if it has not occurred.
9. Keep V1 scope tight. Architect for future couples features but do **not** implement them during V1.
10. Treat the app as a **date activity discovery app**, not a dating service that matches individuals. There are no dating profiles, swiping through people, chats with strangers, or matchmaking in V1.

## 2. Product vision and audience

Dates Slovenia helps couples and people who are dating answer **"Kam na dejt?"** quickly. Users discover thoughtfully curated date activities across Slovenia, personalize suggestions, view ideas on a map, filter by practical constraints, and save ideas for later.

It should also be useful to established couples. A “date” might be a romantic dinner, a nature walk, cooking together, an activity, a free outing, or something at home. The long-term vision is a relationship companion that encourages couples to spend quality time together, but **V1 is discovery-first**.

**Core outcomes:**
- Find a realistic date idea in under a minute.
- Feel that recommendations match interests and actual constraints.
- Make exploring new activities pleasant, not overwhelming.
- Remember dates worth trying through Save for Later.
- Work well in Slovenia, with Slovenian copy, EUR prices, and local geography.

**Success signal:** a new user completes or skips onboarding, discovers an idea, opens its details, saves it, and later returns to their saved list, without needing assistance.

## 3. Scope boundaries

### Confirmed V1 requirements — implement all

1. **Home screen** with a welcoming headline, recommended dates, popular/editorially trending dates, quick category selection, and horizontally scrollable date cards.
2. **Date discovery** through browse/search and an **advanced filter** interface.
3. **Interactive map** displaying geographically located date ideas in Slovenia as pins. Selecting pins opens previews and detail pages.
4. **Interest-based onboarding** so recommendations and category order can reflect the user’s preferences.
5. **Advanced filters** for distance, price range including free dates, categories, duration, suitable setting (including at-home ideas), and transportation preference (car, walking, bicycle, bus/public transit) with honest behavior where routing data is not available.
6. **Personalization-versus-discovery control**, implemented as a simple slider between familiar preferences and discovering new experiences. Avoid fabricated scientific “compatibility” percentages.
7. **Saved for Later**: bookmark and unbookmark date ideas; show all saved ideas on a dedicated screen. Save should work as a guest, persist locally, and sync safely if the user creates an account.
8. **Date detail page** with photos when licensed, description, category, locality, estimated total/budget price information with explicit price basis, typical duration, tags, map/open-directions action where relevant, and save/share actions.
9. **Slovenian-first mobile UI**, reliable navigation, loading/error/empty states, and responsive layouts for modern phones.
10. **Real content management path**, initially via Supabase Studio, so founders can publish and update ideas without editing source code.

### V1 supporting features — implement unless blocked by external setup

- Optional account creation and sign-in, not required to browse. Persist preferences/saves anonymously on device; authenticated users sync them across devices.
- Search by title, activity, category, and city (with normalized Slovenian diacritics if feasible).
- City selector and a permission-based current-location option. No location permission on first launch without context.
- Native share sheet for a date-idea link or short description; robust deep link if configured and a reasonable fallback if not.
- Basic settings: preferences, chosen city, language readiness, privacy/terms links, logout, delete account, contact/report outdated information.
- Founders' editable featured/popular ordering. Display labels such as “Priljubljene ideje” only when based on actual engagement or editorial curation; do not invent usage counts.

### Explicitly not required for V1 — preserve for the roadmap

- Couple accounts/linking two partners together.
- Shared wishlists, joint bucket lists, and shared date completion tracking.
- Couple challenges, streaks, badges, scores, Duolingo-style gamification.
- Relationship AI assistant, reminders to plan dates, notifications about new matching activities.
- User-to-user matchmaking, private chat, dating profiles, public social feed.
- Venue reservations or any claim of live booking availability.
- In-app venue payments, discounts, affiliate checkout.
- Fully functional premium subscriptions/paywalls; design extensibility, but do not charge anyone in the MVP.
- Automatic venue scraping or republishing Instagram/TikTok footage without permission.
- Complex route and public-transport calculations unless a real provider and key exist.

## 4. Technical architecture

**Recommended path: native-first with one codebase for iOS and Android.**

- **Mobile:** Expo + React Native + TypeScript + Expo Router, using current stable mutually compatible versions.
- **Database / backend / identity:** Supabase Postgres, Auth, Row Level Security (RLS), and Storage. Use Supabase Edge Functions only for actions requiring server trust (e.g., full account deletion).
- **Map:** `react-native-maps` for the native map, with Apple Maps on iOS and Google Maps on Android where supported. Use `npx expo install` for matching Expo versions and document any production API-key setup. Keep mapping behind an internal adapter so the library can be swapped later.
- **Location:** `expo-location` with foreground permission only. Provide manual city selection when permission is denied.
- **Remote server state:** TanStack Query; use a small and simple provider or store for filters, onboarding, and guest saved IDs. Do not add heavyweight global state without need.
- **Local persistence:** AsyncStorage or equivalent Expo-compatible storage for anonymous preferences/saves and onboarding flag.
- **Forms/validation:** TypeScript and Zod for validated forms/remote payloads; use additional form libraries only if helpful.
- **Rendering:** reusable React Native components and design tokens; no dependence on fragile bespoke animation frameworks. Use native-friendly libraries only when they materially improve the experience.
- **Testing:** TypeScript checks, ESLint, unit tests for filtering/recommendation/sync, component tests where practical, and a documented device QA checklist.
- **Deployment:** EAS development/preview/production build profiles. Include instructions for App Store Connect and Google Play Console, but do not simulate store submission.
- **Optional later:** RevenueCat for native subscriptions, after premium value and pricing are decided. Its real purchase flows require development builds and store configuration. Do not substitute Stripe Checkout inside native apps for subscriptions to digital app features without reviewing applicable store rules.

**Important:** The founding team has discussed a web app as an alternative, but no final decision was made. Choose native-first for this implementation because the requested experience is mobile-centric with maps, saved ideas, and potential native subscriptions. Keep business logic and data models portable. Do not attempt a full web PWA in parallel; a web content-management interface can come later.

## 5. UI design language

Aim for a modern, warm, editorial leisure/discovery brand rather than a generic dating-app aesthetic. Think travel discovery meets curated local experiences. Avoid overly intense pink, excessive hearts, and fake romantic clichés.

**Editable initial design suggestion (not a previously approved brand decision):**
- Background: warm off-white `#FAF8F5`.
- Primary brand/accent: warm coral `#F2685B`.
- Main text: deep charcoal `#22242A`.
- Secondary text: muted slate `#6A7078`.
- Pale warm card/background tint: `#FFF0EB`.
- Good contrast, clear type scale, rounded cards (14–20px equivalent), generous whitespace.
- Beautiful image-led date cards where licensed photos exist; polished neutral gradient/graphic placeholders when none exist.
- Short, approachable Slovenian copy (e.g., “Kam na dejt?”, “Zate”, “Priljubljeno”, “Razišči”, “Shranjeno”, “Brezplačno”, “Doma”, “Preseneti me”).

Set up reusable design tokens, a small component library (`DateCard`, `CategoryChip`, `FilterChip`, `PriceLabel`, `EmptyState`, `SectionHeader`, `PrimaryButton`, `SaveButton`, `MapPreviewCard`), safe-area handling, accessible tap targets, screen-reader labels, and proper long-text handling.

Do not let a missing image crash a screen or create blank unexplained cards. Respect reduced-motion preferences.

## 6. App navigation and individual screens

**Bottom tabs** (initial proposal): **Domov**, **Razišči**, **Zemljevid**, **Shranjeno**, **Profil**. Use simple recognizable icons. The date detail route is accessible from any tab. Optional filter UI is a sheet/full-screen modal depending on device behavior.

### 6.1 Splash and first-launch onboarding

- Branded loading state while determining guest vs authenticated state.
- Onboarding page 1: friendly two-line explanation of why this app exists and what it does.
- Onboarding page 2: ask users to choose interested categories (chips, multiple choice; recommend at least 3 but allow skip). Avoid collecting intimate/personal relationship details.
- Onboarding page 3: ask preferred kinds of dates/vibes (optional: romantic, playful, relaxed, adventurous, creative, cozy) and optional approximate budget.
- Onboarding page 4: select home city (e.g., Ljubljana, Maribor, Celje, Koper, Kranj, Novo mesto, Ptuj, Murska Sobota, Bled, or custom search) or press “Uporabi mojo lokacijo” which asks for foreground permission only after the tap.
- End with a clear “Razišči ideje” CTA. Account creation is **optional**; no sign-up wall before discovery.
- Persist choices locally and, if authenticated, in the user's private database records.
- Allow a user to change choices under Profile later.
- First-launch flow must also work offline using demo data.

### 6.2 Home / Domov

Implement a genuinely polished and populated discovery home:
- Greeting/title “Kam na dejt?” and brief supporting message.
- Visible search action; city/location control.
- Compact category chips/buttons prioritized by onboarding selections.
- “Priporočeno zate” horizontal card rail, using deterministic recommendation scoring.
- “Priljubljene ideje” rail using real save/engagement metrics if available, otherwise explicit editorial “Izbor uredništva” and controlled featured sorting.
- “Brezplačni dejti” and perhaps “Za malo drugačen dejt” rails when enough suitable content exists. Do not add empty sections.
- A useful CTA to the full Explore and Map views.
- Tapping cards opens details; heart icon saves without navigation.
- Pull-to-refresh and sensible skeleton, empty, and error states.

### 6.3 Explore / Razišči

- Search input at top; inline category quick filters; button to advanced filters.
- Date card list/grid suitable for mobile with cover image, title, location/type, approximate EUR price, duration, category, and saved control.
- Clear result count and “Počisti filtre” reset.
- Sort modes: “Zate” (personalized), “Najbližje” (if reference city/location exists), “Najceneje”, optional “Izbor uredništva”.
- Pagination or efficient loading; never load unlimited large remote datasets in one render.
- Reflect active filter chips, support removing individual filters.

### 6.4 Advanced filters

All supported filters must work together and remain applied when navigating between Explore and Map:

- **Price:** “Brezplačno” standalone toggle, user-selectable maximum and/or min-max estimated EUR amount. Clearly distinguish approximate *per person* vs *per couple* vs *total*, so amounts are not misleading. If mixed units cannot be reliably compared, either normalize in the data or label the limitation.
- **Distance:** configurable radius (e.g., 5, 10, 25, 50, 100 km, all Slovenia) relative to selected city or granted location. Label this as approximate geographic distance unless a real route provider is used. At-home ideas bypass location radius only when an explicit home filter or inclusive mode is selected; never show them as false map pins.
- **Duration:** up to 1 hour, 1–2 hours, 2–4 hours, half/full day, or flexible ranges; use a clearly documented overlap rule.
- **Categories:** multi-select; show user's liked categories first; all still available.
- **Indoor/outdoor/at home:** make at-home discoverable, not forced into geolocation.
- **Commute preference:** walking, bike, car, bus/public transit. Initial behavior can filter against manually maintained *suitability tags* if actual route/transport data is not integrated. Do not display fabricated journey times or transit routes. Explain “Primerno za prihod z ...” when only suitability is known.
- **Fit vs explore slider:** 0 = “Po mojem okusu”, 100 = “Nekaj novega”. It changes **ranking** of eligible results, not hard eligibility; a user must not lose valid dates without understanding why. Re-ranking should visibly work.
- Reset and Apply actions; accessibility labels; retained last-used filters.

### 6.5 Map / Zemljevid

- A real pannable, zoomable map, initially centered to show Slovenia or selected region.
- Markers generated from published date ideas with valid Slovenian lat/lng; no pins for home-only activities.
- A compact preview card when a marker is tapped: photo, name, estimated price, short description, save button, “Poglej idejo” action.
- Quick filter chips and filter button reuse the same filter state as Explore.
- Avoid huge marker overload; use available clustering compatible with chosen map library or viewport filtering where sensible. Start with simple markers for small seed data.
- User location indicator only with permission; refusal must not break map.
- “Open directions” goes to a genuine installed mapping app or system maps. If directions are not available, open location coordinates in system maps and clearly label it.
- A useful empty viewport state: “Tu še nimamo idej — razširi zemljevid ali filtre.”
- Only publish content within Slovenia for V1; do not assert the map itself physically cannot pan abroad.

### 6.6 Date detail

Must include:
- Hero photo or attractive fallback.
- Title, locality, helpful short summary, clear saved toggle.
- Duration, estimated price or free label, and explicit price basis (per person, per couple, or total).
- Primary and secondary categories and vibe/setting tags.
- Longer “Kaj lahko počneta?” description and practical considerations (booking needed? weather-dependent? seasonal? accessibility if verified).
- Media attribution where required and “Nazadnje preverjeno” only if a real checked date exists.
- Optional location/address with map action; hide address/map for at-home ideas.
- Useful external links only if verified and available (official venue info, activity organizer).
- Native share action; clear way back to list/map.
- “Sporočite napako” / report-outdated-info contact method.
- Do not make up opening times, popularity counts, restaurant reservations, offers, or venue guarantees.

### 6.7 Saved for Later / Shranjeno

- User can bookmark/unbookmark from any card or detail.
- Bookmarks persist between launches while signed out, and sync cross-device after login.
- Show empty state with useful CTA back to Explore.
- Simple search/filter saved items if easy; avoid bloating V1 with complex folders.
- Guests are not nagged to sign up constantly; explain account benefit in settings or during an optional sync prompt.
- On login, **union/merge** local IDs with server saves without data loss or duplicate records, then persist/clear local state only after successful confirmation.

### 6.8 Profile / Profil

- Guest identity with optional signup/sign-in CTA, and signed-in account email.
- Edit interests, vibes, budget, location/city, and discovery default.
- Show a plain-language privacy note on location and content personalization.
- Settings: privacy policy URL, terms URL, contact email, log out, account deletion for signed-in users, and version number.
- Account deletion must be functional or, if backend credentials not yet configured, unmistakably marked as nonproduction and listed as a release blocker. Do not create accounts in public production without a compliant deletion path.
- No partner account, couple score, gamification, or relationship profile in V1.

## 7. Content model and taxonomy

Date ideas should work for location-based outings **and** at-home activities. Data must be editorially maintainable and structured for filters.

### Recommended core categories

- `food_drink` — Hrana in pijača
- `nature_outdoors` — Narava in na prostem
- `culture` — Kultura
- `adventure` — Aktivno in adrenalinsko
- `creative` — Ustvarjalno
- `relaxation` — Sprostitev
- `entertainment` — Zabava
- `home` — Domači dejti
- `seasonal` — Sezonsko

Keep “Brezplačno” as a price property/filter, **not** a mutually exclusive category. Store additional descriptive tags (romantic, cozy, active, rainy-day, winter, etc.) separately from core categories.

### Proposed Supabase/Postgres tables

Create idempotent SQL migrations under `supabase/migrations/` and typed client interfaces. Use UUIDs except fixed lookup codes.

**`categories`**
- `id` text PK (stable machine code)
- `name_sl` text not null
- `name_en` text null (future)
- `icon_key` text
- `sort_order` int
- `active` bool default true

**`date_ideas`**
- `id` uuid PK, `slug` unique text, `title_sl` text, `summary_sl` text, `description_sl` text
- `title_en`, `summary_en`, `description_en` nullable for later English
- `location_type` enum/text constrained to `venue`, `outdoor`, `home`, `multi_stop` (do not pretend to support a full multi-stop route in V1)
- `venue_name`, `address`, `city`, `region`, `country_code` default `SI`
- `latitude`, `longitude` nullable for home ideas; enforce sensible Slovenian coordinates and valid pair if geolocated
- `price_min_eur`, `price_max_eur` nullable numeric; `price_basis` (`per_person`, `per_couple`, `total`, `unknown`); `is_free` boolean or derived from verified zero price
- `duration_min_minutes`, `duration_max_minutes` nullable int, consistent nonnegative values
- `setting` enum (`indoor`, `outdoor`, `mixed`, `home`)
- `vibe_tags` text array, `season_tags` text array, `commute_suitability` text array of `walk`, `bike`, `car`, `bus` (editorial suitability only)
- `official_url` nullable validated https URL, `practical_tips_sl` nullable
- `editorial_featured` boolean default false, `editorial_sort_order` nullable int
- `published` boolean default false, `verified_at` nullable timestamp
- `created_at`, `updated_at`

**`date_idea_categories`**
- `date_idea_id` FK, `category_id` FK, PK on both, `is_primary` optional boolean with integrity rules

**`date_idea_media`**
- `id`, `date_idea_id`, `storage_path`/verified asset path, `media_type`, `alt_sl`, `sort_order`
- `rights_holder`, `source_url`, `license_basis` (`owned`, `permission`, `licensed`), `permission_proof_reference`, `attribution_text`, `approved_for_publication`
- Display **only** rights-approved assets. Start with images; video can be added later.

**`profiles`**
- `user_id` UUID PK references auth.users on delete cascade
- `display_name` nullable, `home_city` text nullable, `language` default `sl`, `onboarding_complete` bool
- `default_max_budget_eur` nullable, `novelty_percent` int check 0–100, `created_at`, `updated_at`
- Do **not** permanently save fine-grained GPS location by default. Base ranking on selected city centroid or transient device location held only during app use.

**`user_category_preferences`**
- `user_id`, `category_id`, `weight` int default 1, composite PK; self-only operations.

**`user_vibe_preferences`**
- `user_id`, `vibe_tag`, composite PK; self-only operations.

**`saved_date_ideas`**
- `user_id`, `date_idea_id`, `created_at`, composite unique PK; self-only operations.

**`admin_users`** (or other least-privilege role representation)
- `user_id` UUID PK, `created_at`; membership must not be self-assignable. It is set by privileged backend management only. Do not expose role edits to arbitrary authenticated users.

Possible future tables should be captured in `docs/ROADMAP.md` (shared couples, date completions, challenges, notification prefs), not added unnecessarily now.

### Security / RLS requirements

- Public/anonymous users: `SELECT` **only published** date ideas, active categories, approved public media. Unpublished drafts and permission-proofs must not leak.
- Authenticated users: may read/update **only their own** profile, interests, vibes, and saves. Use RLS with `auth.uid()` and appropriate policies. No broad public profile endpoint.
- Normal mobile clients cannot edit canonical date ideas or assets. Admin operations must require secure server-side/DB authorization.
- Supabase `service_role` is never in `EXPO_PUBLIC_` environment variables, shipped code, or client logs.
- Use Supabase Storage policies so public images are approved; retain permission proof in a private location.
- Add indexes for `published`, `city`, `editorial_sort_order`, category joins, and saved user ID; optimize only after correctness.
- For account deletion: secure user-verified server endpoint/Edge Function deletes associated records and the Supabase Auth user. Never expose service-role credentials to the app; handle any legal retention transparently.

## 8. Discovery, personalization, and filtering algorithms

Keep algorithm **deterministic, explainable, and testable**. Personalization is a ranking tool, not a claim about relationship compatibility.

### Hard filters (eligibility)

1. Published only; content locale appropriate (Slovenian must exist).
2. Selected category OR semantics for multiple categories (document choice).
3. Price constraints with sensible unknown-price behavior: unknown may be excluded for a strict maximum and should be clearly communicated. `free_only` requires verified `is_free=true`.
4. Duration intersection: include a date if estimated duration overlaps chosen band, and document null-duration handling.
5. Geographic radius based on Haversine distance from current user position or selected city center. Home ideas are handled as a distinct mode, not assigned artificial coordinates.
6. Location/setting selection.
7. Commute suitability tags if enabled, explicitly **not** real travel time.
8. Keyword search (case/diacritic-insensitive where possible).

### Ranking (within filtered eligible ideas)

Build a small pure TypeScript scoring function, with tests, using example approximate factors:
- Up to **35 points** interest/category similarity.
- Up to **20 points** geographic proximity when applicable.
- Up to **15 points** preferred vibe tags.
- Up to **15 points** affordability/known price vs stated budget.
- Up to **10 points** practical suitability such as duration/setting.
- Up to **5 points** editorial boost or variety.

These are an **initial implementation suggestion**, not a scientific model. Normalize missing inputs and document weighting. Use the novelty slider to reduce category-match dominance and gently boost lesser-explored categories without ignoring hard constraints. Avoid silently changing the meaning of price/radius filters. Prevent every carousel from showing exactly the same ideas. Use stable sorting and seeded variety so the UI does not visibly reshuffle at random on every render.

Do not label cards “93% compatible”. Prefer honest explanatory microcopy such as “Izbrano glede na tvoje interese”. If no preferences exist, fall back to editorial selection and varied categories. When there are insufficient cards, show fewer rather than fabricating content.

For the “Popular” section, use real save counts from privacy-respecting aggregated data if available; otherwise label **“Izbor uredništva”**. Do not expose raw per-user analytics.

## 9. Authentication and offline demo behavior

- Guest-first discovery is required. Viewing, searching, filtering, mapping, and saving must work without an account.
- Real authenticated accounts use Supabase Auth (initially simple supported email login flow) and secure token persistence. Choose an officially supported, stable React Native auth pattern.
- If external credentials are absent, app should launch in a clearly labeled **DEMO MODE** with bundled sample content and local storage—enough to test every non-server feature. Demo credentials, fake signups, and fake server success must never be represented as production auth.
- On real backend config, query Supabase for published content and save/profile changes.
- Support expiring sessions, logout, network failures, and merge after guest sign-in.
- Avoid automatically creating backend “guest accounts” just for browsing; local guest state simplifies account-deletion obligations and privacy.
- Seed at least **20 demo ideas** across a range of categories, free/paid ranges, at-home and geolocated concepts, and multiple Slovenian cities **clearly flagged as sample/test content**. Generic activities (e.g., walk by a public river, cooking at home) are acceptable as demo examples; do not invent real business reviews, opening hours, detailed pricing, or commercial promises. Do not publish demo content to a production database by accident.
- Provide an import/seed workflow for **editorially verified actual ideas** in production. Launch should use only properly reviewed published entries.

## 10. Content workflow for the two founders

V1 can use **Supabase Studio** as the editorial dashboard—do not let building a custom CMS delay the consumer app.

Create `docs/CONTENT_PLAYBOOK.md` explaining, in plain language:
- How to create/edit a date idea (Slovenian title, one-sentence hook, description, category, city, price basis, approximate duration, tags).
- How to add a valid map location if applicable; leave coordinates null for at-home ideas.
- How to upload a photo only when they created it, purchased a compatible license, or received explicit permission; record media source/credit/approval.
- How to record venue verification date and when to remove outdated ideas.
- How to preview a draft and publish safely.
- How to choose editorially featured categories/date ideas.
- A spreadsheet/CSV import template with validated columns if practical.

**Never** copy external Instagram reels, TikTok videos, Google Maps photos, or venue marketing images without sufficient permissions. A future outreach channel to Slovenian venues can provide approved footage; treat this as a content acquisition process, not as technical permission to scrape.

## 11. Monetization-ready, not monetization-blocked

Founders have considered subscriptions but have **not confirmed** a V1 paywall or exact premium offering. Build the app so a paid tier can be introduced later without rewriting basic data access, but keep core discovery features free in V1. Do not put a decorative nonworking “Subscribe” button into production.

- Add `docs/MONETIZATION.md` explaining plausible value propositions for paid tiers (e.g., improved planning assistance, couple coordination, premium curated collections) without implementing them or inventing prices.
- When the team specifically approves premium, integrate RevenueCat with real Apple/Google purchase products and entitlements; provide restore purchase / management flows, transparent pricing, and thorough store tests.
- Full native IAP tests need a supported native development build and store setup; Expo Go preview is not proof of real purchases.
- Digital subscription policy and billing rules must be rechecked at launch for Slovenia/EEA and for both stores.

## 12. Accessibility, privacy, release quality, and data honesty

- Support current iOS/Android safe areas, font scaling, dark-mode tolerance (light mode can be the default), keyboard avoidance, touch targets, screen readers, and meaningful image alt labels.
- Graceful handling of no network, no GPS permission, no photos, no location for home ideas, no search results, no saved ideas, stale remote records, and empty backend.
- Efficient image display and caching, `FlatList` for large lists, no repeated expensive full-dataset ranking on every keystroke, and minimal geolocation requests.
- Request only foreground location permission at the point of need. Allow city input as a complete alternative; no background location tracking.
- Implement real data deletion for registered users and disclose data storage honestly. Include placeholders/configurable real privacy policy and terms URLs and mark them **release blockers** until founders publish actual documents. Consider GDPR and ensure location preferences and analytics follow a privacy-first approach; do not claim legal compliance without review.
- Avoid unconsented trackers. Analytics, if added, should be aggregated, privacy-aware, and documented.
- Ensure any in-app subscription marketing is accurate, with actual subscription value and clear billing disclosure. No subscription enforcement in V1.
- Always show estimated prices as estimates, not guaranteed current offers; only expose verified facts.
- No automatically scraped venue content or AI-fabricated business details.

## 13. Suggested code layout

Adapt to the actual generated Expo Router template, but maintain clear separation of concerns:

```text
app/
  _layout.tsx
  onboarding/
  (tabs)/
    index.tsx               # Domov
    explore.tsx             # Razišči
    map.tsx                 # Zemljevid
    saved.tsx               # Shranjeno
    profile.tsx             # Profil
  date/[slug].tsx
  filters.tsx
  auth/
components/
  date/
  discovery/
  map/
  ui/
features/
  auth/
  onboarding/
  dates/
  filters/
  recommendations/
  saved/
lib/
  supabase/
  location/
  maps/
  storage/
  i18n/
  validation/
constants/
  designTokens.ts
  categories.ts
  cities.ts
assets/
  demo/
supabase/
  migrations/
  seed/
tests/
docs/
  SETUP.md
  CONTENT_PLAYBOOK.md
  DECISIONS.md
  ROADMAP.md
  MONETIZATION.md
  QA_CHECKLIST.md
.env.example
README.md
AGENTS.md
```

Create appropriate reusable hooks, types, and repositories to separate UI from Supabase/database implementation. Map and storage services should be replaceable with mocks in tests. Keep files focused; avoid giant all-in-one screens.

## 14. Implementation plan (execute sequentially)

### Phase 0 — Repository foundation and plan

1. Inspect repo and tooling; scaffold Expo TypeScript + Expo Router if empty.
2. Establish tabs, navigation, project conventions, lint/typecheck/test scripts, theme tokens, and README.
3. Create `docs/DECISIONS.md` for key assumptions, `docs/SETUP.md` for founder setup, and `.env.example` for public environment names only.
4. Show founders how to run it locally and which external accounts/keys are actually necessary.
5. Confirm the app runs with initial empty route screens and no runtime exceptions.

**Done when:** starter launches, navigates across tabs, typecheck and lint run.

### Phase 1 — Attractive, working local V1 prototype

1. Build demo fixtures and typed data models.
2. Implement onboarding, category selection, local preference storage, Home rails, Explore/search, date details, Saved for Later.
3. Implement advanced filters and explainable recommendation/novelty behavior as pure tested functions.
4. Add graceful no-image, no-results, loading, and offline states.
5. Integrate map and demonstrate geolocated demo pins, selected-pin cards, directions link; allow no-permission city mode.
6. Polish Slovenian UI to a consistent high standard.

**Done when:** a founder can install/test and complete browse → filter → open → save → map → restart → saved remains, entirely without backend credentials.

### Phase 2 — Real backend, auth, secure persistence

1. Add migrations for categories, date ideas, joins, media, profiles, preferences, saves, admin roles.
2. Enable and verify RLS, anonymous published-only reads, per-user mutations, indexes, and storage media policy.
3. Supabase API integration, with robust loading/errors and clear demo/real boundary.
4. Implement optional login/signup/logout, profile edits, guest save/preference migration, and account deletion through a secure server-side path.
5. Test anonymous permissions and attempted cross-user reads/writes.

**Done when:** multiple test users see the same published ideas but only their own saves/preferences, and the app has functional account deletion.

### Phase 3 — Map/filter consistency and content workflow

1. Bind map markers and Explore to the same filtered data selectors.
2. Efficient map viewport load/clustering if dataset requires it.
3. Add `CONTENT_PLAYBOOK.md` and CSV template/seed import; show how founders create and publish content in Supabase Studio.
4. Implement source/rights and verification metadata usage; verify unpublished and rights-unapproved assets are not public.
5. Test geolocation denied/granted and city fallback.

**Done when:** newly published Supabase content appears in discovery and on map where applicable without changing app code.

### Phase 4 — Production hardening

1. Fix bugs found in iOS and Android manual testing.
2. Improve performance, accessibility, error reporting (only when configured), error states, and deep-link sharing.
3. Configure EAS dev/preview/prod build profiles and production app metadata placeholders.
4. Provide App Store / Play Store prep checklist and identify all outstanding credentials/policies/content approvals.
5. Run and record test suite/typecheck/lint/Expo compatibility checks; update docs honestly.

**Done when:** app is a functional release candidate, with a precise list of non-code launch tasks remaining. Do **not** claim it is live on stores unless it has actually been submitted and approved.

### Phase 5 — Only after explicit founder approval

Evaluate paid subscriptions, web version, custom admin dashboard, couple-linking, shared bucket lists, couple challenges, streaks, push reminders, and relationship-assistant features. Each gets separate requirements and acceptance criteria before implementation. Do not silently expand V1 scope.

## 15. Test plan and acceptance criteria

Automated tests should cover at least:

1. Price-free/paid/unknown behavior and price-basis handling.
2. Distance filtering with and without user location or selected city.
3. Combined filters, `OR` categories, duration overlap, home activity behavior.
4. Recommendation stability, cold-start fallback, preference-based reordering, novelty slider changing rankings without breaking hard filters.
5. Add/remove bookmarks and idempotent guest-to-account merge.
6. Slug/ID-based detail navigation; missing/retired date handling.
7. Security access patterns (using test Supabase instance or SQL verification) for anonymous published-only access, cross-user data isolation, and role elevation denial.

Manual acceptance checklist:

- [ ] First launch in Slovenian, no account required.
- [ ] Skip onboarding and still browse.
- [ ] Choose 3 favorite categories and verify recommended ordering changes.
- [ ] Deny location and choose city; all main features keep working.
- [ ] Search for a date and navigate to details.
- [ ] Free-only filter includes verified free items but excludes price-unknown entries.
- [ ] Radius + category + duration + commute + at-home filtering works in combination.
- [ ] Novelty slider visibly changes sort without showing ineligible items.
- [ ] Map shows valid Slovenia pins, pins open correct cards, home dates are not pinned.
- [ ] Save/unsave works from Home, Explore, Map preview, and Details.
- [ ] Kill/restart app; local saves remain.
- [ ] Log in; local saves merge without duplicates; second device gets synced saves.
- [ ] Log out; personal data does not appear in another account session.
- [ ] Registered user can delete account successfully.
- [ ] Unpublished draft/rights-unapproved content is not accessible as public content.
- [ ] Simulate network errors, image failure, invalid URL, and empty backend; no crashes.
- [ ] UI tested with larger text and accessibility options on iPhone + Android.
- [ ] Actual `lint`, `typecheck`, tests, and Expo compatibility checks complete with recorded results.

## 16. Founder-facing deliverables you must produce

At the end, provide:

1. **A running project**, not just Figma-style screen mockups.
2. **README.md** — exact commands for install, start, test, and build; explain demo vs real backend.
3. **docs/SETUP.md** — step-by-step beginner guide: which free accounts to create, what to copy into `.env`, how to configure Supabase, how to import date ideas, and which approvals/payments are needed for eventual store publication.
4. **docs/CONTENT_PLAYBOOK.md** — nontechnical publishing guide.
5. **docs/QA_CHECKLIST.md** — how the two founders can verify everything on a phone.
6. **docs/ROADMAP.md** — all deferred shared-couple, challenges, notifications, assistant, and subscriptions ideas.
7. **docs/DECISIONS.md** — product/architecture assumptions and rationale.
8. **Working SQL migrations and example seed data**, separated for demo and production.
9. **A list of outstanding manual tasks** that Codex cannot finish without accounts, credentials, approved footage, business facts, or App Store/Google Play review.
10. **A final audit** listing features completed, partially completed, unimplemented, and each test actually run with result. Never state an unrun test passed.

## 17. Instructions for each Codex coding session

When I ask you to implement a feature:

1. Read this file, `AGENTS.md`, and the latest `docs/DECISIONS.md` and `README.md`.
2. Restate in 2–4 bullets what will be built and which V1 acceptance tests apply.
3. Make the code changes, not just a plan. Reuse existing UI tokens and data access patterns.
4. Run the relevant tests, lint, and typecheck available in the environment; report exact commands and results.
5. Update necessary documentation and a concise changelog/next-task list.
6. Only stop early if blocked by required account access or other unresolvable external dependencies; implement everything else safely using fixtures/mocks with truthful labels.

### First command to act on now

**Inspect the repository; scaffold the app if it is empty. Create the planned Expo app and documentation. Then implement Phase 1 end-to-end: onboarding, polished Home, Explore with advanced filters, Slovenia Map, Date details, Saved for Later, sample content, guest persistence, and functioning navigation. Use working code and tests. Do not implement V2 couple features or a fake subscriptions screen.**

---

## 18. Official documentation to consult as needed

- Expo: https://docs.expo.dev/
- Expo Router: https://docs.expo.dev/router/introduction/
- Maps (`react-native-maps`): https://docs.expo.dev/versions/latest/sdk/map-view/
- Location: https://docs.expo.dev/versions/latest/sdk/location/
- Supabase React Native auth: https://supabase.com/docs/guides/auth/quickstarts/react-native
- Supabase RLS: https://supabase.com/docs/guides/database/postgres/row-level-security
- RevenueCat + Expo (future only): https://www.revenuecat.com/docs/getting-started/installation/expo
- Apple account deletion: https://developer.apple.com/support/offering-account-deletion-in-your-app/
- Google Play account deletion: https://support.google.com/googleplay/android-developer/answer/13327111

**Final priority rule:** Finished, testable V1 flows are more valuable than a broad mockup of features that do not work.
