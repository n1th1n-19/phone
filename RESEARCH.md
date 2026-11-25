# Research

Collected 2026-11-25 while planning daystack apps.

## 1. Fossify (github.com/FossifyOrg)

Fossify is the community continuation of Simple Mobile Tools: offline, ad-free, open-source Android apps.

### Apps (stars as of 2026-11-25, all actively maintained)

| App | Stars | daystack name |
|-----|------:|---------------|
| Gallery | 3.9k | Gallery |
| Calendar | 2.3k | Calendar |
| File-Manager | 1.9k | Files |
| Messages | 1.7k | Messages |
| Phone | 1.4k | **Phone (pilot)** |
| Voice-Recorder | 1.1k | later |
| Music-Player | 980 | later |
| Contacts | 960 | Contacts |
| Launcher, Clock, Keyboard | ~700 each | later |
| Notes | 524 | Notes |
| Camera, Calculator, Paint, Documents | <500 | later |

### Tech facts that shape the plan

- **License: GPL-3.0.** Forks must stay GPL-3.0, keep copyright notices and publish source.
- **All apps are Kotlin + XML Views.** No Jetpack Compose in app code. Redesign = XML layouts, drawables, styles and custom views.
- Kotlin 2.4.20, compileSdk 36.
- **Every app depends on `org.fossify:commons` (7.1.3).** This is the leverage point. Commons contains:
  - `commons/src/main/res/values/styles.xml`: `AppTheme.Base` (parent `Theme.Material3.Dark.NoActionBar` / `.Light`), `AppToolbar`, `MaterialAlertDialog.App`, `ShapeAppearance.App.MediumComponent`, `AppTheme.ActionModeYou`, `MyRadioButtonStyle`, popup menu styles.
  - 33 shared views in `commons/src/main/kotlin/org/fossify/commons/views/`: `MyAppBarLayout`, `MySearchMenu`, `MyMaterialSwitch`, `MyRecyclerView`, `MyFloatingActionButton`, `MyTextView`, `MyButton`, `MyEditText`, `MyTextInputLayout`, `MyAppCompatCheckbox`, `MyCompatRadioButton`, `MySeekBar`, `Breadcrumbs`, PIN/pattern/fingerprint tabs, and more. They apply the user-picked text/accent/background colors.
  - A small Compose theme package (`compose/theme/`) used by a few settings-type screens.
  - Built-in theme options include a "system" theme that follows Material You dynamic colors. Reuse it for the accent.
- Restyling commons once changes toolbars, dialogs, switches, search, lists and pickers in **every** app.

### Phone app specifics

- ~59 Kotlin files. Main screen is a ViewPager with tabs (Contacts, Favorites, Call history) plus a dialpad activity and an in-call activity.
- Uses Android `InCallService` (must be the default dialer app to handle calls).

## 2. Apple iOS (Human Interface Guidelines)

What makes something read as "iOS" on Android:

- **Navigation:** bottom tab bar (3–5 tabs, icon + label, selected tab tinted). Large title (34pt bold) at the top of root screens that shrinks into a centered 17pt inline title when scrolling.
- **Lists:** inset-grouped style. Rounded cards (~10–12pt radius) on a grey grouped background, hairline separators inset to the text start, section headers in small uppercase/secondary text, footers in footnote size. Trailing chevron `›` for drill-down, trailing `ⓘ` for details.
- **Actions:** text buttons in the nav bar ("Edit", "+", "Done") instead of floating action buttons. Edit mode shows red minus circles per row.
- **Sheets:** content slides up as a card with a small grabber; large and medium detents. Replaces most dialogs. Alerts are small centered rounded boxes with stacked or side-by-side text buttons.
- **Controls:** pill switch with green "on" track, segmented controls, rounded grey search field with magnifier icon under the large title.
- **Material:** translucent blurred bars (tab bar, nav bar) over content.
- **Motion:** spring-based, interruptible; push/pop slides; no ripple, highlight on press instead.
- **Typography:** SF Pro scale — Large Title 34, Title 1 28, Title 2 22, Title 3 20, Headline 17 semibold, Body 17, Callout 16, Subheadline 15, Footnote 13, Caption 12/11.
- **Color:** semantic system colors (systemBackground, secondarySystemGroupedBackground, label, secondaryLabel, separator), system blue tint, green for call, red for destructive.
- **Phone app reference:** tabs Favorites/Recents/Contacts/Keypad/Voicemail; keypad of large round buttons with letters under digits; big green call button; in-call screen with contact name, round control grid, red end button.

**Licensing:** SF Pro and SF Symbols are licensed for Apple platforms only. They cannot ship in an Android app. Substitutes: **Inter** (closest open font) and **Material Symbols Rounded, filled**.

## 3. Samsung One UI

- **Reachability:** root screens open with a tall header (roughly the top third) holding a big title and optional subtitle/status. Content starts lower, in thumb range. Scrolling collapses the header into a normal toolbar.
- **Swipe to call / message:** in Phone and Contacts, swipe a row right to call, left to send a message. Colored background (green/blue) and icon reveal under the row as it moves.
- Rounded card groups, soft shadows-free surfaces, bold headers.

## 4. Google Material You

- **Dynamic color:** palette taken from the wallpaper (Android 12+). Fossify commons already supports it.
- **Predictive back** gesture animations (Android 14+).
- Respect Android conventions iOS doesn't have: system back, notification behavior, default-app roles.

## 5. The daystack mix

Apple is the base look and structure. One UI adds the tall header and swipe-to-call. Material You supplies the accent color and Android-native behavior. Details in [DESIGN.md](DESIGN.md).
