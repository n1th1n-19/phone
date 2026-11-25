# Rebrand: Fossify → daystack

Goal: nothing a user sees says Fossify, except the required credits. This is a **hard fork**: code packages get renamed too.

## Name map

| Thing | Fossify | daystack |
|-------|---------|----------|
| Suite | Fossify | daystack |
| Phone package / applicationId | `org.fossify.phone` | `org.daystack.phone` |
| Shared library package | `org.fossify.commons` | `org.daystack.commons` |
| Library Maven coordinate | `org.fossify:commons` | `org.daystack:commons` |
| Launcher label | Phone | Phone |
| Full name (About, store-like text) | Fossify Phone | daystack Phone |
| GitHub repo | FossifyOrg/Phone | n1th1n-19/daystack-phone |

Later apps follow the same map: `org.daystack.messages`, `.contacts`, `.notes`, `.gallery`, `.filemanager` (label "Files"), `.calendar`.

## Order of work

1. Bring in upstream source (`git remote add upstream https://github.com/FossifyOrg/Phone`, merge with `--allow-unrelated-histories`, keep our docs on conflict).
2. Rename in **one dedicated commit** per repo, no UI changes mixed in.
3. Build, install, smoke test before any redesign work.

## Rename checklist

### Code
- [ ] Move source dirs: `src/main/kotlin/org/fossify/...` → `src/main/kotlin/org/daystack/...` with `git mv`.
- [ ] `package` and `import` lines in all `.kt` / `.java`.
- [ ] `namespace` and `applicationId` in `build.gradle.kts`.
- [ ] `AndroidManifest.xml`: activity/service/receiver names, provider **authorities**, custom permissions, intent actions (`org.fossify.*.ACTION_*`).
- [ ] Fully qualified view tags in layouts: `<org.fossify.commons.views.MyTextView` → `<org.daystack.commons.views.MyTextView`.
- [ ] ProGuard / R8 rules (`proguard-rules.pro`).
- [ ] String constants used by reflection, shared prefs names, file provider paths, notification channel IDs. Note: renaming prefs/channel IDs resets user settings, fine because it's a new app ID anyway.
- [ ] `settings.gradle.kts` dependency substitution: `org.daystack:commons`.
- [ ] `libs.versions.toml` coordinate rename.

### User-visible
- [ ] `app_name`, `app_launcher_name` and every string containing "Fossify" in **all** `res/values*/strings.xml` locales.
- [ ] About screen: remove Fossify links, donate, "rate us", "more apps", Thank-You app prompts. Add daystack repo link + credit row.
- [ ] FAQ entries that mention Fossify or its site.
- [ ] Launcher icon (adaptive + monochrome), round icon, splash, notification small icons → new daystack icons.
- [ ] Any links to `fossify.org` / `github.com/FossifyOrg` → daystack repo, or removed.
- [ ] `fastlane/metadata` → delete or rewrite for daystack (not publishing to stores now; delete is fine).
- [ ] `README.md`, `CHANGELOG.md` → daystack versions (keep credit).
- [ ] Crash/issue report templates pointing at Fossify.

### Commons-specific
- [ ] Shared About/FAQ/Purchase/"Thank you" screens: strip Fossify upsell flows entirely.
- [ ] Default app color palette & theme names.
- [ ] `isPro` / Fossify Thank-You checks: remove, features always on.

## Final check

```sh
grep -rIin fossify . --exclude-dir=.git --exclude-dir=build
```

Allowed hits only:
- `LICENSE` (if it names authors), `CREDITS.md`, `README.md` credit section
- About screen credit string `based_on_fossify`
- Original copyright headers in source files

Anything else is a bug.

## Credits text (use exactly)

- About row: **"Based on Fossify. Open source, GPL-3.0"** → links to https://github.com/FossifyOrg/Phone
- README: "daystack Phone is a fork of Fossify Phone by the Fossify team, licensed under GPL-3.0."

## Cost of the hard fork

Upstream fixes can't be merged directly anymore. To take one:
`git fetch upstream`, `git cherry-pick <sha>`, fix package paths in the conflict, build. Check upstream releases monthly for security/call-handling fixes.
