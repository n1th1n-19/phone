# Roadmap

Folder layout for the whole suite:

```
~/Documents/daystack-apps/
├── commons/     shared library (theme, views, dialogs, swipe helper)
├── phone/       ← this repo (pilot)
├── messages/
├── contacts/
├── notes/
├── gallery/
├── files/
└── calendar/
```

## Phase 0 — Docs ✅ (this commit)
Design system, research, rebrand plan, testing plan.

## Phase 1 — Setup
- Install JDK 21 and Android SDK (cmdline-tools, `platforms;android-36`, `build-tools`, `platform-tools`) into `~/Android/Sdk`.
- Create `commons/` repo the same way (docs + upstream remote `FossifyOrg/commons`).
- Merge upstream source into `phone/` and `commons/`.
- Wire composite build: `includeBuild("../commons")` in `phone/settings.gradle.kts`.
- Stock build compiles and installs. **Exit:** stock Fossify Phone running from local source.

## Phase 2 — Rebrand
- Full rename per [REBRAND.md](REBRAND.md), in `commons` then `phone`.
- **Exit:** builds, installs next to nothing named Fossify, `grep` check clean, calls work.

## Phase 3 — Commons restyle
- Tokens, Inter font, icon set.
- Large-title One UI header, search field, inset-grouped rows, iOS switch, alerts, sheets, action sheets, edit mode.
- `DsSwipeCallHelper` (swipe-to-call/message).
- **Exit:** every Fossify-derived screen in Phone already looks iOS-ish without per-app work.

## Phase 4 — Phone screens
1. Bottom tab bar + Favorites/Recents/Contacts/Keypad structure
2. Keypad
3. Recents (segmented All/Missed, edit mode)
4. Contacts + detail sheet
5. Favorites
6. In-call + incoming call
7. Settings + About
- **Exit:** [TESTING.md](TESTING.md) checklist passes on a real phone as default dialer for a week.

## Phase 5 — Next apps
Each gets its own repo, docs and plan, reusing commons:
1. **Messages** — conversation list + bubbles; swipe helper reused.
2. **Contacts** — mostly shared with Phone's contacts screens.
3. **Notes** — list + editor, quickest.
4. **Gallery** — Years/Months/All grid, Albums tab.
5. **Files** — Browse/Recents tabs, iOS Files layout.
6. **Calendar** — month/day views, the biggest.

## Later
- Widgets restyle
- Shared icon pack for the whole suite
- Optional F-Droid / GitHub Releases with signed APKs for friends
