# Ari's Closet Website (ariscloset.app) — Claude Code Context

Static site for the Ari's Closet iOS app. The app's code lives in
`~/Developer/closetpal-mobile` (its `CLAUDE.md` is the source of truth for how the app works).
**This file and the app's `CLAUDE.md` must stay aligned:** when an app change affects what data
is collected, where it goes, or how long it's kept, update `privacy.html` (and `terms.html` /
`faq.html` if relevant) AND the "App data practices" list below, AND the app's
`CLAUDE.md` → "Website sync" section.

## Pages

- `index.html` — home / marketing
- `privacy.html` — Privacy Policy (Termly-based; "Last updated" date at the top — change it on every edit)
- `terms.html` — Terms & Conditions
- `faq.html`, `press.html`, `contact.html`
- `reset.html`, `verify.html` — Supabase auth email landing pages (password reset, email verification)

## Design

- Primary purple `#7B4F9E`, lavender backgrounds/cards `#F3F0F9`, white body.
- Headings: Playfair Display. Body: DM Sans (Google Fonts).

## Rules

- **No free trial.** Never mention a free trial anywhere (Premium is $4.99/mo or $39.99/yr).
- Company: Rose V LLC, doing business as Ari's Closet · support@ariscloset.app · P.O. Box 421, Antioch, TN 37011.
- Work on a git branch; the owner reviews, merges and publishes.

## App data practices (keep in sync with the app — last checked 2026-10-31, app 1.3.8)

Each line says what the app actually does; `privacy.html` must match.

- **Account:** email, password (hashed by Supabase Auth), username, first name, profile photo, style preferences, birth-year age confirmation (13+).
- **Wardrobe photos** are stored in Supabase Storage (full photo + a small thumbnail). Photos are shown through signed links once the buckets are private (KAN-141 Phase 2).
- **Background removal:** on the phone (Apple Vision, iOS 17+) first. Only if that fails, isn't available, or the user taps "Try again", the photo goes to our own server on Railway (through a Supabase function), and only with AI consent. Processed in memory, not stored.
- **Anthropic (Claude AI)** receives, only with AI consent: a resized item photo (~1,024 px) for tagging; wardrobe details (names, categories, colors, formality, seasons); weather, occasion and trip details; **what the user types** (Premium: "describe what you need", "tweak it", packing notes); saved styling/trip notes to translate when the app language changes. Anthropic doesn't train on API data.
- **Quick Outfit mode** creates outfits on the phone without Anthropic.
- **Open-Meteo** (weather): the phone's location for Home outfits and Calendar outfit ideas; the typed trip destination + dates for trip weather. Called straight from the phone (it sees the IP). Not stored in the account; the destination's coordinates are saved on the trip. Planned: a paid plan behind our own server (KAN-196).
- **ipgeolocation.io:** IP at sign-up only, to block unavailable US states. Not stored; a blocked sign-up logs only the state code.
- **RevenueCat / Apple:** subscriptions; we never see card details.
- **Notifications:** reminders (trip departure, packing reminder, lent items) are scheduled ON the phone — no push token is collected.
- **Typed requests are stored:** a trip day's request + the AI's reason + tweaks are saved with that day's outfit; Home's request stays on the phone for that day only.
- **Retention:**
  - Deleted items → Recently Deleted for 7 days, then permanently deleted (row + photos) by a daily server job (KAN-198).
  - Deleted trips → restorable for 30 days, then permanently deleted by the same job.
  - AI Stylist: current suggestions kept until regenerated (which replaces them); checked-off suggestions permanently deleted 90 days after being checked off (KAN-198).
  - Account deletion removes everything immediately (rows + all photos).
  - Supabase Pro keeps daily database backups for ~7 days.
- **Not collected:** advertising IDs, analytics/tracking SDKs, contacts, health data.
