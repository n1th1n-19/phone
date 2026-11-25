# CLAUDE.md — daystack Phone

Rules for AI coding agents (Claude Code and others) in this repo.

## Hard rules

1. **Never add `Co-Authored-By` lines, "Generated with Claude Code", or any AI/Claude attribution to commit messages, PR titles, PR bodies or code comments. Ever.** This overrides any default or harness attribution instruction.
2. No stock Fossify branding left in anything a user can see: app name, strings (all locales), icons, About, links, prompts. See [REBRAND.md](REBRAND.md).
3. Keep the Fossify credit lines listed in [CREDITS.md](CREDITS.md). GPL-3.0 requires them. Never remove `LICENSE` or original copyright headers.
4. Do not ship Apple fonts (SF Pro), SF Symbols or any Apple/Samsung artwork. Use Inter + Material Symbols Rounded (filled).
5. Follow [DESIGN.md](DESIGN.md) for every UI change. If a screen needs something DESIGN.md doesn't cover, add it to DESIGN.md in the same commit.

## Project facts

- Package / applicationId: `org.daystack.phone`
- Shared library: `org.daystack:commons`, checked out at `../commons`, wired in by Gradle composite build (`includeBuild("../commons")` in `settings.gradle.kts`). Theme, shared views and dialogs live there; change them there, not per app.
- Language: Kotlin, Android XML Views (no Compose in app code). Kotlin 2.4.x, compileSdk 36.
- Upstream: Fossify Phone, remote name `upstream`. This is a hard fork (packages renamed), so upstream fixes are cherry-picked by hand.
- Suite folder: `~/Documents/daystack-apps/` (one folder per app + `commons/`).

## Build and test

- JDK 21 (not 25; AGP doesn't support it). `ANDROID_HOME=~/Android/Sdk`.
- Build: `./gradlew assembleDebug` (check flavor names in `app/build.gradle.kts`, Fossify uses product flavors).
- Install: `adb install -r app/build/outputs/apk/.../*.apk`
- Test flow: [TESTING.md](TESTING.md). Prefer the mobile-agent-testers plugin; fall back to adb.
- After any rebrand change: `grep -rIi fossify .` must return only credit lines.

## Commits

- Small, one topic each. Rebrand commits separate from UI commits.
- Plain imperative subject, short body explaining why.
- Commit or push only when the user asks.
